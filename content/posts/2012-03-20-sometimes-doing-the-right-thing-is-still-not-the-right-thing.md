---
title: Sometimes doing the right thing is still not the right thing
author: drrandom

date: 2012-03-20T10:14:00+00:00
url: /2012/03/20/sometimes-doing-the-right-thing-is-still-not-the-right-thing/
dsq_thread_id:
  - 3825327029
tags:
  - .Net
  - 'C#'
  - IoC
  - Tips

---
Tentatively subtitled: “How scale can make fools of us all”

This is going to be a real life war story…cause I haven’t done one of those in a while, and this particular case really ticked me off. Here’s the scoop: I’ve got a “service” which is called by other parts of the system. And by “service” I don’t mean something running in its own process and waiting for SOAP/REST requests or messages, I simply mean something that has a defined entry point (a static method in this case), where you pass in some data, and get something back.

Like many others, I’m sure, I’m using an IoC container to wire up bits so that I can have a big ball of interfaces “to make testing easier” (one of these days I’ll break that rather nasty habit and figure out a better way to do thing, but I’m getting off topic). Specifically, I’m using [Windsor ](1) for my dependency injection because it seems to have become the Container de jure among the devs that actually are using containers at work (StructureMap was in there for a while too, but it seems to have faded). As many of you may know, Windsor is one of those containers that tracks instances for you so that it can use a [Lifecycle ](2) rule to decide whether to give you an already existing instance of an object, or create a new one for you. It will also automatically call Dispose() on IDisposable objects that it may be tracking, thus helping ensure proper cleanup of resources.

In my case I had everything set up using the Transient [lifestyle ](3), because each request was essentially stateless, and there really wasn’t a lot of expense involved in creating a new instance of the objects. Because I’ve done my homework, I know that if you’re using Transient objects in Windsor, you should explicitly call Release on the container to release the object when you’re done with it, otherwise you’re likely to get a memory leak, since the container would be holding on to an instance of the object, not letting the GC do its thing. So, I made sure I did that, and my code looked something like this:

```csharp
var myService = _container.GetService<IMyService>();
try
{
    myService.DoWork();
}
finally
{
    _container.Release(myService);
}```

The one thing to point out here, is that my reference to `_container` was a singleton, so I would get it set up the first time and then use the pre-configured container after that. So, where is the problem? Anyone? Well, I didn't see anything wrong with it. And neither did the person doing the code review. But, as you might guess from the fact that I’m writing about this, there was a problem, and here’s how it manifested itself:

Approximately 6 days after this went to production, one particular set of servers in one of our data centers (lets say for the sake of this post we have 2) started kicking out OutOfMemoryExceptions during calls to the service. My first thought was, “strange, but I’m doing the right thing here and releasing, so its probably just something else eating up memory and my code is suffering”. To help demonstrate this I even set up a test running 1000 calls to the service in a while loop and watching the memory…nothing unusual, hovered around 33MB. So I fired up the most excellent [dotTrace memory profiler ](4), and it confirmed.

4 more days go by and our operations folks come and beat the crap out of me because they have had to reboot production servers every couple of hours. Ok, they didn’t beat the crap out of me, but they wanted to, and they did send along a dump, which one of the other devs who is a wiz with [windbg ](5) was able to translate into something meaningful for me. The dump showed thread contention in ReaderWriterLockSlim.WaitOnEvent(), and about 200MB worth of an object called [Castle.Microkernel.Burden ](6). And here are some other interesting details: The service is called by all kinds of different servers; Web servers, SOAP servers, REST servers, but none of these were showing problems. The only one that was having issues was a server that was set up to process asynchronous SOAP requests (don’t ask). And each server could process up to 20 at a time.

Armed with this information I did some googling, and discovered that the Burden object is the thing you leak when you don’t call Release() on the container in Windsor…._But I **was** calling release!_ I found a [blog post by Davy Brion ](8) that talked about getting leaks when using your own Windsor container with NServiceBus, and how to deal with it….seemed interesting, but it also seemed like something that didn’t apply, since the problem there was that NServiceBus didn’t know about calling Release() since it was written with a container that didn’t keep references. It did lead me to the [source code for the release policy ](8), which showed me something very interesting.

The Windsor object tracking is basically doing some reference counting. The ReaderWriterLockSlim is being used to manage the count of instance references, so when you create a new instance it is incremented, and when you release an instance it is decremented. In either case you’re doing a write, so you’re calling a ForWriting() method on a lock wrapper, which is effectively trying to do a write lock (at some point down the call stack)….very interesting. At this point I decided to see if I could reproduce the problem, and so I took my earlier test running 1000 calls in a loop, and kicked it up a few notches on the concurrency scale, and set it up to run calls in a while loop until the thread was canceled. I fired up 25 threads to do this, launched the little console app and waited. Sure enough I was able to see in process monitor that memory was rising….there were some spots where a large collection was taking place, but it wouldn’t release everything, and so soon my little app which started at around 40 MB was using 50 MB, then 60 MB. It was the concurrency! The multiple requests were stacking up new instances of object, and new instances of the Burden object faster than they could be collected because the whole thing was bottle-necked by the ReaderWriterLockSlim!

So I plugged in a version of Davy’s code to fix the NServiceBus issue, only I decided since I was managing this container local to my service, and I was also dealing with any Disposables myself, that I would not let it track anything (there is actually a built-in policy object for not tracking anything…just realized that). Plugged it in, fired up the test, and I had a little console app that ran for about an hour and hovered at about 40MB of memory in use.

We actually did an emergency deployment to push this to the effected set of servers in production, and I’m happy to say that so far I’ve not seen an issue….of course our logs stopped showing the OutOfMemory exceptions about 24 hours before we pushed the fix, so we have that to help out our feeling of doubt that the issue is resolved. And even though I could create something suspicious locally, we were never able to recreate the production issue in QA. One of the interesting things about our environment is that we have a lot of customers who do things that we don’t exactly expect, or want, them to do. It looks like in this case we had some customers who were doing a lot of asynchronous calls and they just managed to stack up in a way where things got ugly.

 [1]: http://www.castleproject.org/container/index.html
 [2]: http://stw.castleproject.org/Windsor.Lifecycle.ashx
 [3]: http://stw.castleproject.org/Windsor.LifeStyles.ashx
 [4]: http://www.jetbrains.com/profiler/
 [5]: http://msdn.microsoft.com/en-us/windows/hardware/gg463009
 [6]: https://github.com/castleproject/Windsor/blob/master/src/Castle.Windsor/MicroKernel/Burden.cs
 [7]: http://davybrion.com/blog/2010/02/avoiding-memory-leaks-with-nservicebus-and-your-own-castle-windsor-instance/
 [8]: https://github.com/castleproject/Windsor/blob/master/src/Castle.Windsor/MicroKernel/Releasers/LifecycledComponentsReleasePolicy.cs