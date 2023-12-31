---
title: What You Should Know About Concurrency — Part IV — Concurrency of Functional Programming Languages
description: The finale of the article series dedicated to concurrency. Concurrency of functional programming ((F#, Multi-Lisp, OCaml).
categories: [computer science]
tags: [concurrency, CPL, multi-lisp, ocaml, f#, book summary, medium]
---


![](https://cdn-images-1.medium.com/max/800/1*mRI1-TDKtXCDlHvj3KDE5A.jpeg)
_[Pixabay](https://pixabay.com/photos/tree-nature-landscape-water-3137482/)_

A very slow-paced movie, which is very common to the style of Andrei Tarkovsky, [**Offret/The Sacrifce (1986)**](https://en.wikipedia.org/wiki/The_Sacrifice)_,_ has the following opening narration:

> “You know, sometimes I say to myself, if every single day, at exactly the same stroke of the clock, one were to perform the same single act, like a ritual, unchanging, systematic, every day at the same time, the world would be changed. Yes, something would change, it would have to.”

Tarkovsky believes in the virtue of systematism and perseverance, but I should also question why it is the case. And I cannot arrive at any other answer but to exclaim — “completeness!”. It is the feeling of a complete work that is well-done is what brings satisfaction to a human soul and has some chance to change the world. And it is very difficult to achieve that without systematism and perseverance.

If I should start something, then I am obliged to complete it.[^1] If I stop reading a boring book at one point, stop watching a boring movie, stop writing a boring poem, or even radically — stop living a boring life, then my brain will tingle with a sense of incompleteness, forever.

If I can murder all of my desires, my curiosity, my interests, and inclinations, then I might achieve something in the long run. I can say that I did something, I finished my job, wrote a novel, composed an opera, built a house, planted a tree. Only in that way I can feel complete and worthy of respect.

But c’mon, let’s not be over-dramatic — everything is not that serious. Holy smokes. Let’s continue our discussion about concurrency and shuffle off this mortal coil.

We have already had an overview of the key concepts of concurrency in [**Part I**](/posts/concurrency-1), discussed some ways of providing mutually exclusive access to a resource in [**Part II**](/posts/concurrency-2), and studied the implementation of concurrency in _Ada, Java,_ and _C#_ programming languages in [**Part III**](/posts/concurrency-3). Again, let’s not forget that all these articles are mainly based on the book [**Concepts of Programming Languages**](https://www.amazon.com/Concepts-Programming-Languages-Robert-Sebesta/dp/0134997182) by _Robert W. Sebesta_.

Today comes the judgment day, with **Part IV** being dedicated to the implementation of concurrency in **functional programming languages**. If you have any difficulties with understanding the functional programming paradigm, then you can read my evergrey [**essay on Lisp**](/posts/lisp).

## Concurrency in Multi-Lisp

Multi-Lisp is a concurrent version of the Scheme programming language when concurrency is implicitly called. The **pcall** construct (macro) serves this purpose. However, its safety is under question. As a result, the **future** construct comes into the spotlight, which is considered to be more productive. Just like pcall, it evaluates function calls wrapped inside it in a concurrent thread. Here, if the parent thread needs a return value from a function that hasn’t completed its execution, then the parent thread waits for its completion. Futures resemble [forking combined with lazy evaluation](https://en.wikipedia.org/wiki/MultiLisp).

## Concurrent ML

The concurrent version of ML programming language. In CML, concurrency is supported with the help of threads and synchronous message passing. A thread is created with the help of **spawn** primitive. After the thread’s creation, its function parameter starts to execute on a new thread. It results either with produced output or through communications with other threads. Termination of threads, be it parent or child, does not impact the execution of the other.

For threads to communicate with each other, channels are used (similar to _goroutines_ and _channels_ in [**Go programming language**](/posts/go-dart). Here, two main functions — **send** and **recv** (receive) messages are used. The type of the send operation below is inferred to be integer, when recv returns the value of the channel.

```lisp
let val ch = channel()

send(ch, 10)

recv(ch)
```

Send and receive can take place when both the sender and receiver are ready (otherwise, they wait). Functions can take channels as parameters. Just like in [**Ada programming language**](/posts/concurrency-3), synchronous message passing should be able to decide which message to choose when more than one channel receives one, when the guarded command **do-od** construct does a random choosing. The [event](https://www.cml.org/For-Sorting/Colorado-Cities-and-Towns-Campaign/Events) is the synchronization mechanism in CML.

## F#

F# is a part of .NET family [^2], so its similarity to C# while implementing concurrency will look extremely strange.

```fsharp
let createThread() =   
    let th = new Thread(MethodMan)  
    th.Start()
```

Synchronization is not required for shared [**immutable**](https://stackoverflow.com/questions/2194201/what-are-the-advantages-of-built-in-immutability-of-f-over-c) data, but things are different with **mutable** data. In this case, **locks** are used. As noted in the book, a mutual heap-allocated variable is of type ref. It can be changed with the aid of a lambda expression which uses **:=** operator. The prefix **!** gets the value of ref.

```fsharp
let s = ref 0

lock(s) (fun () -> s := !s + x)
```

Learn about **mutable and unmutable objects in Python** in an [**Analytics Vidhya article**](https://medium.com/analytics-vidhya/mutable-and-immutable-objects-py-efe79e5d2d39).

**Asynchronously** called threads use **BeginInvoke** and **EndInvoke** subprograms and **IAsyncResult** interface, just like in C# (refer to the previous article on concurrency for more information).

## Parallelism in Lisp

For parallelism in Lisp, refer to [**this**](https://www.ijcai.org/Proceedings/87-1/Papers/011.pdf) paper.

That’s it about functional programming languages. It’s over, [go home](https://www.youtube.com/watch?v=IQXxpiz3qqA). But if you are interested in reviewing the whole chapter about concurrency, keep reading, because here's when Cameron goes berserk.

## Statement-Level Concurrency in High-Performance Fortran

We want to understand how a programmer can inform the compiler to optimize the execution of programs in a multiprocessor architecture. High-Performance Fortran (HPF) provides it.

The main specification statements in HPF determine the number of processors, distribution of data over their memories, and the alignment of data with other data in memory placement. **!HPF$** prefix introduces these statements, which looks cool. The statement below tells the compiler that 3 processors can be used to compile the code.

```fortran
!HPF$ PROCCESSORS prcs (3)
```

DISTRIBUTE and ALIGN specifications are used on machines when each processor has its own memory (refer to the first part of the article series).

```fortran
!HPF$ DISTRIBUTE (kind) ONTO prcs :: identifier\_list
```

The _kind_ could be BLOCK or CYCLIC. For more information about what they actually do, refer to the **section 13.10.1** in the book. The identifier\_list has the names of array variables that will be distributed.

Align, on the other hand, determines how to relate an array’s distribution to another array.

```fortran
!HPF$ ALIGN array1_element WITH array_2 element
```

Another statement is FORALL, which specifies the sequence of assignment statements to be executed concurrently.

```fortran
FORALL (index = 1:100)  
   list_1(index) = list_2(index)  
END FORALL
```

`FORALL` is a perfect match with vector machines. Two similar methods are implemented in C#, which behave like FORALL: `Parallel.For` and `Parallel.ForEach.` [^3]

## Footnotes

[^1]: Fish like to talk about flying.

[^2]: Just like the other languages in the .NET family, F# also can interoperate with the other members via the .NET Framework’s Base Class Library (BCL). It roots back to ML family languages, being similarly a strongly typed language. Standard ML, Caml, as well as Haskell influenced the design of F#, together with the previously mentioned OCaml which F# tries to imitate.

[^3]: The statements we have discussed here are only introductory and do not show the full capacity of HPF. For more information, refer to [here](http://hpff.rice.edu/).