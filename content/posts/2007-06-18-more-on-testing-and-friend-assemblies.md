---
title: More On Testing and "Friend" Assemblies
author: drrandom

date: 2007-06-18T15:33:00+00:00
url: /2007/06/18/more-on-testing-and-friend-assemblies/
tags:
  - TDD

---
So if you recall from some of my earlier posts, I've talked about the concept of the &#8220;Friend&#8221; class in C++ and how it could apply to TDD within .Net.  Well, today, with the help of Roy Osherove, I just stumbled upon the <a title="InternalsVisibleToAttribute" href="http://msdn2.microsoft.com/en-us/library/system.runtime.compilerservices.internalsvisibletoattribute.aspx" target="_blank">InternalsVisibleToAttribute</a> within .Net 2.0.  This allows you to specify within one assembly, another assembly that should have access to the **internal** members of your assembly.  This is genius, and goes a long way towards allowing you to keep your code encapsulated, while still being testable.  If we could just get them to go one step farther, and allow for access to private and protected members as well, life would be good, and there would be no more of this OOD vs TOOD junk.

The other interesting thing about this is that it could allow you to give a separate utility...say the Castle Microkernel, access to internal class constructors, and thus enforce creation of your objects through the Kernel from outside assemblies.  This is actually a feature I am desperately wanting in my current project,  but sadly, I am limited to .Net 1.1, so I can't quite get there.

Here is a quick look at how this works.  Here is a very unrealistic class in an assembly that I want to test:

<pre class="brush: csharp">class TestClass
{
    internal TestClass()
    {}

    private bool PrivateMethod()
    {
        return false;
    }

    internal bool SomeMethod()
    {
        return true;
    }

    public string PublicMethod()
    {
        return "You can see me";
    }
}
```

Now, I add the following to the AssemblyInfo.cs file:

<pre class="brush: csharp">[assembly: InternalsVisibleTo("TestAssembly")]
```

And here is what Intellisense looks like in my test class  
[<img style="border-width: 0px; border: 0;" src="/image.axd?picture=transfer%2fInternalVisibleIntellisense.png" alt="" width="576" height="248" border="0" /> ](1)

Not bad.  Overall I would say this is defiantly a good feature to have in your toolbox.  Internals are not perfect, but they are much more versatile than a lot of folks give them credit for.  Now if only I could get something like this in .Net 1.1

 [1]: /image.axd?picture=transfer%2fInternalVisibleIntellisense.png