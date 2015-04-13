---
layout: post
title: "STL Iterators and Performance"
date: 2008-05-30 11:02
comments: true
categories: [c++,stl]
---

The Standard Template Library (STL) for C++ provides a set of powerful and flexible templated container classes.  Never again will you have to hand-craft a doubly-linked list (and get your pointer arithmetic mixed up) -- just use `std::list<T>`.

Now most of the idiomatic C++ code I've read that uses STL iterators uses the prefix `operator++` to move the iterator forward.  And so I had long ago adopted this too, with a vague recollection of having read somewhere that it performed better.  But why?  Good question...

<!--more-->

The use of the post-increment operator is well established idiomatic code for looping (in both C and C++):

```c++
  for ( unsigned i = 0; i < array_length; i++ )
  {
    // do something with array[i]
    // ...
  }
```

When the indexing variable is an ordinal type (usually an integer), the incrementing order makes no difference.  The expressions `++i` and `i++` are identical in this case.  And so many people developing in C++ simply use the same style in C++ with their iterator code.  Why would it matter?

Well, it turns out that it does.  If the indexing variable is a class, such as an iterator, there are some subtle differences between the pre-increment and post-increment operators that may have a big impact on performance.

Referring back to Scott Meyer's wonderful [More Effective C++](http://www.amazon.com/More-Effective-C%2B%2B-Addison-Wesley-Professional/dp/020163371X/ref=pd_bbs_sr_1/002-4241626-5806441?ie=UTF8&s=books&qid=1190249817&sr=8-1) book, I tracked down the explanation (item 6, page 31), which I will attempt to reproduce here in simplified form.  The signature of the pre-increment `operator++` for a class T is:

``` c++
  T& operator++(); // prefix
```

while the post-increment operator had a dummy parameter added to make the signature unique:

``` c++
  const T operator++( int ); // postfix
```

But that isn't the only difference - notice that the prefix operator returns a _reference_ to the object itself (permitting chaining of method calls), while the postfix operator returns a _const object_, semantically defined as the previous value.  (This is for consistency with the behaviour of ordinal types.)

The result is that the pre-increment operator modifies the object in-place, whereas the post-increment operator will result in temporaries being created, invoking the constructor and destructor.  So something as simple as `++it` versus `it++` turns out to have some significant side-effects when applied to an object with overloaded operators.  Since this is the case with virtually all iterators in the STL, as well as many other similar objects, it is worth investigating.

So I decided to quantify the difference, to see how much of a difference it made.  I wrote a test program that created a very large (5 million) array of integers, and iterated over the array.  I used a high-resolution timer to time two versions of the loop, identical save for one being pre-increment and one being post-increment.  After a warmup of 3 runs, I ran the test 10 times, collected the timings and compared them.

The results were very interesting: on an Intel workstation, the pre-increment code took an average of 14.1ns per call, while the post-increment code took 27.6ns per call.  That's a significant difference of 49%, or nearly twice as fast!  A plot of 10 iterations each is shown below:

![](/img/results_ppc_g4.png)

So - it really can improve the performance of your code to write:

``` c++
  mylist::const_iterator it;
  for ( it = numbers.begin(); it != numbers.end(); ++it )
  {
    // do something with *it
  }
```

For small arrays, it may not make much of a noticeable difference, but every cycle counts, and a little consistency goes a long way.  Using the prefix increment will enable your application to scale better.

Using the prefix form won't make your actual loop run twice as fast, as the iterator increment is only one component contributing to the performance, and the looping overhead may indeed be swamped by the execution time of the body.  So as always, YMMV!

I have provided the test source in C++, along with an R script to summarise and plot the output.  You can download it via the attachment linked at the end of this article.  The code should compile on any POSIX-conforming system with a decent C++ compiler.  And if you do your own performance tests, please drop me a line and let me know what you found.

As one final note, the wonderful [Boost++ library](http://www.boost.org/) provides the `foreach` module, which provides a powerful wrapper to iterate over containers with a nice clean simple syntax:

``` c++
int total = 0;
foreach( int num, numbers )
{
    total += num;
}
```

And yes, it uses the pre-increment operator for maximum performance.

The source used for this article:

 * [iterperf source on GitHub](http://www.github.com/gavinb/iterperf/)
