---
title: Review – TypeMock Racer and Unit Testing Multithreaded Applications
author: drrandom

date: 2009-06-19T16:31:00+00:00
url: /2009/06/19/review-typemock-racer-and-unit-testing-multithreaded-applications/
dsq_thread_id:
  - 4910612403
tags:
  - .Net
  - Review

---
The folks at [TypeMock ](1) have released a new UnitTesting tool aimed specifically at catching deadlocks in multithreaded code called [TypeMock Racer ](2), and what's more they are <a href="http://blog.typemock.com/2009/06/easy-deadlock-detection-get-free.html" target="_blank">offering free licenses to folks willing to review it during the 21 day free trial period</a>.  As anyone who knows me can testify to, I am a whore for free-bees, so I decided to take them up on this.

**For the impatient, here is the executive summary:  
** This is a good tool.  Period.  It is, however, also a very specific tool that is intended to help find very specific problems.  If you are not doing any multithreaded code, there is no need to have it in your toolbox.  The cost ($890 US) is high enough that it doesn't really seem worth it to get it &#8220;just in case&#8221; (unless, of course, you right a nice review on your blog and get a free license).  If you **_do_** work with multithreaded code, and you** _are_** concerned about deadlocks, this tool will save you lots of time, which ultimately means money.

**Now, for the details**

First of all, this is a tool from TypeMock, so I expected some pretty incredible things, even at the 1.0 release.  After all, their flagship product [TypeMock Isolator ](3) is phenomenally powerful, so much so that the free license I was given for <a href="http://www.drrandom.org/PermaLink,guid,5200ee4d-d92b-4931-9005-705f523228b1.aspx" target="_blank">posting an advertisement</a> for the release of their ASP.Net product has gone largely unused.  I'm just scared of it.  It's like having access to a 50 horse-power tablesaw with no blade guard.  I may be careful enough to use it correctly most of the time, but the fact that it can take off my forearm without pausing to ask my permission makes me reluctant to get too close.  I do know that there are some very real problems that the tool can solve that just can't be done with any other tool, so I have every intent of getting up my courage, grabbing a first aid kit, and jumping in to see what I can do...eventually.

But Racer is different.  It is a very powerful tool with a very specific purpose.  It makes it easier to run tests in multiple threads, and detect deadlocks.  As far as I can tell, it just detects deadlocks, not race conditions, as it's name seems to suggest.  Not that this is bad, just that it only covers half of the rather shaky ground that is traveled while working with multithreaded code.  It is arguable, however, that dealing with deadlocks is the more difficult of the two problems, so getting this one tacked first is not a bad thing.

**So how does it work?**

Fairly straight forward really.  Start with a regular unit test, that is exercising some code that is utilizing locks in an attempt to be thread safe.  It looks like Racer supports just about every form of Synchronization supported by .Net, so you can feel free to use whichever mechanism you are more comfortable with to get the work done.  Now that you have a test, the simplest way to make use of Racer, is to add the Typemock.Racer library reference to your test project, and then add a ParrellelInspection attribute to the test.  This causes Racer to do it's thing, which by default means running the test once with a single thread, and then run it again using multiple threads (2 by default).  If there are no problems, nothing much new happens.  You see some additional information about the test being run first with one thread, and then multiple, and something about scenarios executed...nothing that exciting.  The coolness happens when you actually get a deadlock.  For starters you test output includes a bunch of new information, the most noticeable of which is a big &#8220;DEADLOCK discovered when running the following&#8221; message.  Also is a description of the scenario that caused the deadlock.  Something like &#8220;Thread 1 acquired lock A, Thread 1 released lock A, Thread 2 attempted to acquire lock B, etc&#8221;.  Cooler yet is a message that says &#8220;To reproduce or debug this, copy the following attribute, paste it on top of your test and rerun the test:&#8221;, followed by something that looks like 

<pre class="brush: csharp">[SpecificScenario("2", 1, 2, 1, 1, 1, 1, 1, 2, 1, 1, 1, 2)]```

Which, while being completely incomprehensible to humans, causes your test to run in the specific configuration which caused the deadlock to occur.  This means that you can accurately recreate the exact situation that lead to the problem.  If you have ever had to try to track down a deadlock in a live system, you will realize that this information just saved you countless hours of trying to recreate the production environment, and lots of trial and error getting things into the state that caused the problem.  One word: Brilliant!

**So what is the down side?**

I have to say that I've not yet been able to figure out how Racer determines what scenarios to run, or what the bits in the &#8220;SpecificScenario&#8221; attribute mean(well, the first string parameter is the number of threads, and the other numbers refer to the specific threads, and match the summary of the scenario, but beyond that, not a clue).  It would be interesting to know these things, but not really critical, as long as you are confident all appropriate scenarios are being executed.

There is also an interesting feature that I can't quite get my head around.  When you run a test with a deadlock, an image is generated, which is supposed to be a visual representation of the scenario that created the deadlock.  Here is an example:

<img style="border: 0;" src="/image.axd?picture=transfer%2ftmp442.tmp.png" alt="" width="282" height="177" border="0" />  
 

Now, I see the three objects I was locking against (sync1, sync2, and sync3), and I guess the odd rhombus shaped objects represent the threads, but I'm not really sure what the diagram is trying to tell me.  This is, no doubt, something which is still fairly raw in this early version.  I think it could be very useful if it were clearer what the shapes and arrows represent, but at this point it is simply a cleaver bit of kit that you can show somebody so that they can be confused too.

The last issue I can see with it currently is the price.  At $890 US for a single user license, it isn't an impulse buy.  Granted, I think it can pay for itself easily after finding a few deadlocks in some production code (the earlier they are found the more that find is worth), but I don't see it being a terribly easy sell for management, at least if you are not actively trying to correct a threading issue.  I feel pretty fortunate that I work for a company that understands the value of good tools, and is willing to provide them if there is a need, but I think it would take some convincing to get management to agree to purchase Racer simply because it is a good tool, and could really pay off &#8220;one day&#8221;.  If we were facing a threading issue, and I could demonstrate that racer would allow us to find it, and accurately reproduce it, it would be a fairly easy sell. 

**So overall**

I think this is an excellent tool.  Based on the fact that this is an early release, I can only see it getting better over time.  It is a rather pricey tool, however, so you may have to do some convincing to get the boss-man to get you a license.  There is a 21-day trial, however, so if you find yourself in a situation where you either have, or you could conceivably have, a risk of deadlock, then grab the trial, and use the first detected deadlock as justification to get a full license. 

 [1]: http://www.typemock.com/
 [2]: http://www.typemock.com/learn_about_typemock_racer.php
 [3]: http://www.typemock.com/learn_about_typemock_isolator.php