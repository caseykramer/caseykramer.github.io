---
title: To TOOP or OOP? How to decide?
author: drrandom

date: 2007-05-29T21:12:42+00:00
url: /2007/05/29/to-toop-or-oop-how-to-decide/
dsq_thread_id:
  - 6140321339
tags:
  - Uncategorized

---
Its been a while now, but [Roy Osherove ](1) posted [some ](2) [articles ](3) about Testable Object-Oriented Programming.  That is, designing your code to be testable by default.  The part of this that is most interesting is that he suggests that sometimes you need to sacrifice encapsulation in favor of being able to test your code.  This is where one of the biggest flaws with TDD (at least in my opinion) begins to show.  I think the idea of making code testable breaking encapsulation is one of the only arguments against TDD that I have heard that I can't give a good defense for, and it makes me crazy.

Overall I would consider myself a competent OO programmer...I might even venture to say that I'm really good, so this issue is one that bugs me.  I think encapsulation is one of the most powerful tools out there for making good OO code.  So now I'm a big fan of TDD, which does not allow me to use this tool as much as I would like....so what gives?

**The Problem  
** There are many cases (at least that I've seen) where it makes sense to restrict the visibility of pieces of your object model.  In this context the situation is generally calling for classes/methods to be Internal (Package in Java), or Protected Internal.  I have a feeling, even though I have no evidence to back it up, that this is probably the least-used of all the visibility constraints.  What I've found, though, is that using it allows you to create well-factored classes, that abide by the Single Responsibility Principle, and still present a minimal interface to other layers/users of your code.  There is also the problem of exposing internal dependencies so that those dependencies can be injected during testing (this is where stubs or mock objects come into play).  In many cases there is no real reason to expose this information, and in fact making it available is probably a bad thing because it will make code that is consuming your object more aware of the implementation than it needs to be...And Steve McConnell did a good job at letting us know this is bad.

These are extremely brief introductions to the problems, and even though these are simple examples, such things can get ugly quickly in a large project.  The main reason being that all of a sudden there are many, many more things available to the developer who is just trying to use your class to get some work done.

**Some Solutions  
** There are some solutions which can reduce the complexity somewhat.  In the case of using Internal objects, there is always the option of including your test classes in the same project as the code you are testing.  Microsoft seems to have done this with EnterpriseLibrary 1.1.  The biggest downside to this, however, is that in a lot of cases you don't want to ship your tests with the rest of your production code, so you have to figure out how to do some clever things like conditional compilation to avoid compiling those classes into your production assemblies.  Granted with a build tool like NAnt this becomes easier, but if your not familiar with it, there is a rather steep learning curve.

In the realm of exposing implementation details, one possible course of action is to use a Inversion Of Control container like the [Castle ](4) <a title="IoC components" href="http://www.castleproject.org/container/index.html" target="_blank">Microkernel/Windsor</a> tools.  With these you can write your objects so that they request instances of the dependencies from the IoC container, and not create new instances themselves (or expect you to provide them).  This begs the question of where the IoC container lives, though.  In the case of testing you would want to fill the container with mocks or stubs of the appropriate objects, so that the class your testing gets those objects instead.  In some cases this may mean that your IoC container needs to live in a central repository where all of the objects can get to it...or you could pass around your IoC instance, instead of your dependencies.

The other solution, which was the point of the 2nd post from Roy on TOOD, is to use a tool like <a title="TypeMock" href="http://www.typemock.com/" target="_blank">TypeMock</a>, which has the remarkable ability to mock objects and give you access to the internal/protected/private members in your tests.  Pretty serious stuff.  It doesn't solve the problem of dependency injection completely, though.  There is also the issue of cost if you want all of the really nifty features (though the Community Edition seems to include the ability to mock all methods/properties of an object).

**The Ultimate Solution**  
In my mind what is needed to really bridge the divide between the issues here (which are testability and good OO practices like encapsulation) is to make support for TDD a first-class feature of a language or runtime.  What I mean by that is that the .Net Framework, for example, would have knowledge of test classes and test methods, and loosen the visibility restrictions while running a test.  Most likely this would mean some serious changes to the CLR, but it seems like it should be possible to set up a sandbox environment for testing, where the CLR would evaluate the objects being tested, and then allow full access to all fields and methods.  It would have to be seriously limited, and there may even need to be some restrictions around when and where this sort of behavior can happen, but ultimately it seems like the best solution.  It seems like a stretch, but in my mind it is the only real solution to the problem.  Until that point, we are stuck making decisions about where to draw the line between testability and good OO design.

 [1]: http://weblogs.asp.net/rosherove/default.aspx "ISerializable"
 [2]: http://weblogs.asp.net/rosherove/archive/2007/02/25/why-you-should-think-about-ootp-object-oriented-testable-programming.aspx "TOOP 1"
 [3]: http://weblogs.asp.net/rosherove/archive/2007/03/02/testable-designs-round-2-tooling-design-smells-and-bad-analogies.aspx "TOOP 2"
 [4]: http://www.castleproject.org "Castle Project"