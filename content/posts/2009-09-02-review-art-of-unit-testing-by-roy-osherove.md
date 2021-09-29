---
title: 'Review: Art of Unit Testing by Roy Osherove'

date: 2009-09-02T17:36:02+00:00
url: /2009/09/02/review-art-of-unit-testing-by-roy-osherove/
dsq_thread_id:
  - 4443371693
tags:
  - .Net
  - Review

---
I was pleased to find recently that Roy Osherove’s Art of Unit Testing was available on Safari.  I have been following Roy’s blog for a while now, and was quite excited at the prospect of him writing a book on Unit Testing.  It was only my personal cheapness that kept me from shelling out the $25 to get the E-Book version from Manning ahead of time.  I have to say, now that I have read it, that it would have been well worth the money.  Before I get too deep I want to provide some context for what I am about to say.

**I consider myself an experienced TDD practitioner and Unit Test Writer  
** So that means that I was reading this book hoping to gain some insight.  I wanted to find out how to write better, more readable, more maintainable tests.  I was also hoping for a little bit of “atta-boy” affirmation that the way I do things is the “right” way.  The astute reader may be able to tell that in order for my first hope to be true, the second may have to get some points taken away.  This was in fact the case, and to be honest, coming out of it I feel like I’ve gotten more value from the things I’ve learned than I received from whatever ego stroking may have occurred with what I am currently doing right.

**So lets get started….  
** I was expecting the book to start out essentially as it did, some brief history about the author and an introduction to Unit Testing for those who may not be familiar with it.  I have to say I was expecting the book to be a little more TDD-centric than it was, but I think most of that was my own bias for TDD as “The Only Way To Write Software”.  Roy actually explained what TDD was, and also why he wasn’t going to harp too much on it throughout the book.  I have to say, I can see why he made the decision that he did.  I can also say that it seemed perfectly clear to me that TDD is a technique that he feels has a lot a value, which made me happy.  Since this is supposed to be a review from the perspective of an experienced practitioner of TDD and Unit Testing, I’m not going to go into anything that was touched on in the early chapters, apart from noting that they contained a general introduction to the tools, technique and philosophy of unit testing.  I can also say that, though I was already familiar with the material, I didn’t mind reading through it at all.  Overall, Roy’s writing style was light and quite pleasant, even for a technical book.</p> 

**And now into the meat of the book…  
** For me, things started getting interesting in Part 3 of the book.  This is where issues of test design and organization are addressed.  This is one of those areas that I feel like I need some guidance on, mostly because I developed my testing idioms mostly through habit, and trial and error.  I look back on tests I have written in the past (which could be as little as two days ago) and I wonder how I could have come up with such a brittle, unmaintainable nightmare.  I feel like I need guidance from the experts on what I can do better when writing my tests.  Roy delivered on these items in chapter 7 “The pillars of good tests”.  One of the lessons I took away from this was the value in testing one concept per test.  I had heard this as “one assert per test” in the past, and scoffed at the idea.  But Roy presents a very compelling argument for why this is a good idea, if you are testing multiple concepts, you don’t know the extent of the problem when your test fails.  And lets face it, the failing test is the reason we’re doing this whole thing.  I’ve seen personally the failing test that just keeps failing.  You tackle the issue from one failed assert only to rebuild, and find one right after it which fails as well.  One of the issues I’ve had with this is the redundant setup and configuration that could be required for exercising this concept, but this issue is also addressed by the straight forward recommendation of creating clear and understandable configuration methods.  In the past I have generally not been really good about applying DRY to my test setup, which, I know, is another case of treating tests differently from regular code.  Having someone in a position of authority (like Roy) say, “put your setup in separate methods so you can re-use them and make your tests more readable” made it okay to do the thing that I knew I should be doing anyway.  The key concepts covered are making tests readable, maintainable, and an accurate portrayal of the authors intent.

**Even more in depth….  
** Section 4 goes even further and talks about how to integrate unit testing into an organization which is not already doing it.  This is an interesting subject to me as I have recently moved to a company which has not been doing unit testing and TDD as part of their regular development process.  Roy draws on his experiences as a consultant to provide some really good advice for how to go about enacting this sort of change in an organization.  I particularly pleased with his candor when he describes his failed attempts at integrating unit testing.  It would have been quite easy to simply say “Based on my considerable expertise, these are the things you need to do”, but he chooses instead to share some real-world experience in a straight forward way that only adds to my respect for him as a professional.  In addition to this, he touches on techniques for integrating testing into “legacy” code (i.e. code which is not tested).  He does a good job at introducing some techniques for testing what is essentially untestable code, which a very large nod at Michael Feathers’ “Working Effectively with Legacy Code”. 

The book ends with three appendices, one discussing the importance of testability in the design process, one listing vairous testing tools (both Java and .Net), and the last listing guidelines for conducting test reviews.  This last one is nice, because it presents a concise view of all of the guidelines presented throughout the book, and provides page references where you can get the “why” behind each.  

**All in all…  
** This is a really good book, which should be part of any agile development library.  It doesn’t matter if you are writing your first unit tests, or you’re a seasoned pro, there is going to be something here for you.  I think it is great that Roy has chosen to share his experience with the developer community in this way.  I came into this book with some rather high expectations and I think they were met.

**A note on TypeMock….  
** I remember seeing some criticism floating around on twitter suggesting the book was rather pro TypeMock.  There was also the comment that Roy’s affiliation with TypeMock was not made clear early on.  I can’t say I saw either of these things when I was reading it.  For starters, I already knew Roy worked for TypeMock, so perhaps that skewed my ability to objectively judge if the disclosure was done in a timely manner or not.  I can say that the places in the book which there seemed to be a preference for TypeMock were places where he stated things like “I feel TypeMock has a better syntax in this case”, or “TypeMock is the only tool with provides these capabilities”.  For starters, the first is a statement of preference.  Sure Roy helped design the API for TypeMock, so it seems only natural that he would prefer it to other frameworks, but having used it I would have to agree with the statement.  It is a great API, and example if a fluent interface done well.  The second comment is also plain fact.  Of the mocking libraries available in the .Net space, TypeMock is the only one that allows you to swap instances of objects in place, without making changes to the classes using them.  You can argue over whether or not this is a good or a bad thing, but the fact remains that it is a feature specific to TypeMock.  Maybe I was expecting something more blatant and obvious, but I just didn’t see it.