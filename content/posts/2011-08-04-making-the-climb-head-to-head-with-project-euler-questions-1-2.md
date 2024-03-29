---
title: 'Making The Climb: Head-To-Head with Project Euler (questions #1 & #2)'
author: drrandom

date: 2011-08-04T01:50:00+00:00
url: /2011/08/04/making-the-climb-head-to-head-with-project-euler-questions-1-2/
tags:
  - Making the Climb

---
I’m going to take a brief intermission in my [Scala series ](1), and show a head-to-head comparison of some code in Scala and C#.  To do this, I’m going to go with the first and second problems from [Project Euler ](2).  If your not familiar with the site, it’s a playground full of problems that are absolutely perfect for functional languages (cause they tend to be mathematical functions).  So let’s get started with Question #1:

> _If we list all the natural numbers below 10 that are multiples of 3 or 5, we get 3, 5, 6 and 9. The sum of these multiples is 23.  
>_ _Find the sum of all the multiples of 3 or 5 below 1000._

We’ll start with the C# version:

<pre class="brush: csharp" name="code">public class Problem1
{
   public int GetResult()
   {
       return Enumerable.Range(1, 999).Where(i => i % 3 == 0 || i % 5 == 0).Sum();
   }
}```

With the magic of Linq this is pretty easy (I’ll show later how Linq is basically a way to do list comprehensions in C#, but that’s for one of the longer posts).  Now, on to the Scala version (which you can paste into your REPL or [SimplyScala ](3) if you want):

<pre class="brush: scala" name="code">(1 until 1000).filter(r => r % 3 == 0 || r % 5 == 0).reduceLeft(_+_)```

Now, comparing these too, they are fairly similar. Creating the initial range of numbers is a little easier in Scala (and using the `until` &#8220;keyword&#8221; means we don't have to use `999` like in C#). Instead of the `Where` Scala uses the more traditional `filter` function, and we have to do a little more work and use the `reduceLeft` function with the special `_+_` I [talked about before ](1), but overall they are quite similar. 

So let's move on to Question #2. It is:

> _Each new term in the Fibonacci sequence is generated by adding the previous two terms. By starting with 1 and 2, the first 10 terms will be:  
> 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, ...  
> By considering the terms in the Fibonacci sequence whose values do not exceed four million, find the sum of the even-valued terms._

Seems pretty straight forward.  We want a Fibonacci sequence generator, then we simply need to filter out odd values, and sum the even values that are less than 4 million to get our answer.  Lets start with C#:

<pre class="brush: csharp">public class Problem2
{
    private IEnumerable<int> Fibonacci()
    {
        var tuple = Tuple.Create(0, 1);
        while (true)
        {
            yield return tuple.Item1;
            tuple = Tuple.Create(tuple.Item2, tuple.Item1 + tuple.Item2);
        }
    }

    public int GetResult()
    {
        return Fibonacci().Where(i => i % 2 == 0).TakeWhile(i => i < 4000000).Sum();
    }
}```

This one is a little more involved because we have to generate a Fibonacci sequence. I decided to use an iterator and the magical `yield` keyword to make a never-ending sequence (well, never ending until we end up with an overflow exception that is), but beyond that the solution is very similar to problem #1. 

Now for the Scala version:

<pre class="brush: scala" name="code">lazy val fib:Stream[Int] = Stream.cons(0,Stream.cons(1,fib.zip(fib.tail).map(p => p._1 + p._2)))
fib.filter(_ % 2 == 0).takeWhile(_ <= 4000000).reduceLeft(_+_)
```

Well, isn’t this interesting….The first line is the equivalent of our C# iterator.  It’s creating a _lazy stream_ which is a stream who’s contents are calculated as they are needed (just like our iterator).  The difference here is that Scala doesn’t try to evaluate it all if you just type `fib` into the REPL..it will give you the first few results and then say &#8220;Look, I could go on, but you didn't tell me how far to go so I'm just going to stop here&#8221;, and it spits out a `?` to let you know that there could be more. This means that Scala has a deeper understanding that this thing may well never end.  Keeping this in mind we're actually calculating it's contents recursively (after hard-coding the 0 and 1 values)) by using the `zip` function, which will take a list and combine it with another list into a collection of `Tuples`. For the second list, which gets passed in to the `zip` function on `fib` we're specifying `fib.tail` which is our list, minus the first element. So if our list starts out looking like `List(0,1,...)` then `fib.tail` is `List(1,...)`. That means the initial call to `zip` creates the tuple `(0,1)`. Now, from there we use the `map` function (translate this to `Select` in Linq-ease) to return the sum of the first and second items from out tuple. So now we have just created the third element in our sequence: `1`. So the next time round this all happens again, only on the next elements in the two sequences respectively. So the `zip` function returns a tuple with the second and third element in the sequence: `(1,1)`, and low and behold the 4th element in our sequence is born. This will go on until you stop asking for values, or you get an overflow exception. The entire time the evaluation of what is in the sequence is exactly one element ahead of what is being returned. Kinda mind bending, no? Now for the second line, we once again have an almost one-to-one map to our C# code. We filter out the odd values, take all the values less than 4 million, and then sum up the results.

Hopefully this has been at least a little bit enlightening…I’ll continue on making some more detailed forays into the world of Scala, but I thought an occasional one to one comparison might be help shed some light on some of the places where Scala offers some added elegance to what is possible in C#…as well as those spots where it doesn’t.

 [1]: http://www.drrandom.org/2011/07/31/MakingTheClimbChroniclingTheJourneyFromCToScala.aspx
 [2]: http://projecteuler.net
 [3]: http://www.simplyscala.com/