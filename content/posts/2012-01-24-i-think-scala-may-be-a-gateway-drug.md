---
title: I think Scala may be a gateway drug
author: drrandom

date: 2012-01-24T06:52:00+00:00
url: /2012/01/24/i-think-scala-may-be-a-gateway-drug/
dsq_thread_id:
  - 3825329965
tags:
  - Functional Programming
  - Scala

---
As I have been trying to learn more about Scala, there have been several paths that I’ve had to follow.  One is getting acquainted with the state of Java development, since ultimately Scala exists within the Java ecosystem.  Another is finding my way around the Scala libraries, tools, and idioms.  But there is a third that seems to be somewhat deeper, and that is coming to grips with the functional nature of the language.

Being a hybrid object-oriented/functional language means that for people used to imperative development, you can start with the idioms you already know, and add in the things you don’t.  In my case I'm really comfortable with OO programming, and I’ve gotten over the initial paradigm shift that C#/.Net 3.5 brought in with Lambdas, Closures, and Linq-style functional composition.  With that I could quickly latch on to some of the basics in Scala, like using the `map` method on a list to transform it, since it is effectively the same as `select` in C#.

Thing began to get a little shakier once I started digging deeper into some of the functional aspects of the language.  [List Comprehensions](1) in Scala took me rather off-guard until I realized that in .Net, list comprehensions are called “Linq queries”, though the syntax was still tripping me up.  I also started digging in to [Higher Kinded Types ](2) aka [Type Constructor Polymorphism ](3), and in looking for examples inevitably I was led to more functional constructs.  Eventually I found myself looking at things like [Functors ](4), [Monoids ](5), and the dreaded [Monad ](6). The problem I ran into, though, was that for the most part, these concepts were described in the various blogs in terms of their equivalents from purely functional languages, and the most often cited purely functional language was [Haskell ](8). 

Clearly the only way I was really going to understand these concepts was to learn Haskell….if nothing else I needed to at least be able to understand those crazy type signatures with all of their arrows pointing all over the place. Almost against my own will I’ve been forced to read a lot about Haskell, and to be honest I’m really glad that I did.

As strange as functional concepts are when coupled with the already familiar Object Oriented world, getting your head around a purely functional language is harder.  You have to forget about things like “classes” for containing data, and encapsulation within objects, not to mention polymorphism via sub-classes (or interfaces).  The concepts of encapsulation and polymorphism still exist in Haskell, they’re just different.  More importantly there are elements of the language that are brilliantly simple and elegant (at least to me).  The default style seems to be taking small pieces of functionality, and composing them to make something that is powerful (something of the holy grail in the OO space).  The downside to this is that you can have very small segments of code which are extremely dense, and as a noob it’s difficult to understand why some things happen in the way they do.  But things are getting easier with more exposure.

At this point I’m wanting to really grok the language and the idioms used to build software in a purely functional way, and I’ve gone well beyond just learning enough to understand references in blog posts about Scala.  As such I’ve set myself a goal of completing a project in Haskell that I’m keeping under my hat for the time being…mostly because I don’t know that I’ll ever actually finish it.  One of the more interesting aspect of this for me is the fact that I really don’t know where to begin to build something in Haskell.  Normally I would start thinking about objects and relationships, and that doesn’t apply here.  It’s an interesting state to be in.

So now the question is “do I abandon Scala/C#/Wahtever?”  Of course not.  As impressive and powerful as Haskell is (and trust me, it is a lot of both), and in spite of the fact that there are native compilers for every platform known to man (more or less), and the libraries available are extensive, it doesn’t seem to have a huge footprint.  It’s kind of a shame, since there are things like STM and compile-time parallelization (yes, that’s right), and seems like a good fit distributed computing in general.  For now I’ll let it open the door to new and different ways of solving problems….and maybe eventually see if I can sneak something small into production.

 [1]: http://en.wikipedia.org/wiki/List_comprehension
 [2]: http://en.wikipedia.org/wiki/Kind_(type_theory)
 [3]: http://en.wikipedia.org/wiki/Type_theory#Type_polymorphism
 [4]: http://en.wikipedia.org/wiki/Functors
 [5]: http://en.wikipedia.org/wiki/Monoid
 [6]: http://en.wikipedia.org/wiki/Monad_(functional_programming)
 [7]: http://www.haskell.org/haskellwiki/Haskell