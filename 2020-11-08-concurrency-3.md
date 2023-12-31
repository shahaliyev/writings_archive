---
title: What You Should Know About Concurrency — Part III — Implementation
description: Concurrency with Ada, Java, C#
categories: [computer science]
tags: [concurrency, CPL, ada, java, c#, book summary, medium]
---

![](https://cdn-images-1.medium.com/max/800/1*aNS7Bfse5QHceo4IdDe0dQ.jpeg)
_[Vitruvian Man](https://en.wikipedia.org/wiki/Vitruvian_Man) (1490) — Da Vinci (source: [Luc Viatour](https://lucnix.be/))_

This is the pre-final part of the article series dedicated to concurrency, heavily based on the book [Concepts of Programming Languages](https://www.amazon.com/Concepts-Programming-Languages-Robert-Sebesta/dp/0134997182) by Robert W. Sebesta. You can start reading the other parts through the links below.

*   [Part I — Introduction to the main concepts of concurrency](/posts/concurrency-1)
*   [Part II — Semaphores, Monitors, Message Passing](/posts/concurrency-2)
*   [Part IV — Concurrency in Functional Programming Languages](/posts/concurrency-4)

## Ada Concurrency

We will focus on synchronous message passing. Ada tasks consist of two **syntactic parts**: _specification_ and _body_. The **interface** of tasks is points of entry or where the messages are accepted. Just like message parameters, entry points are described in the **task’s** **specification** part.

```ada
task T begin  
entry E (parameters)  
end task T
```

**_Note:_** _In order to make the explanation easier for me and clearer for you, I will use pseudo-codes that are very similar to Ada syntax. The actual codes written in Ada you can find in the book._

A **task’s body** entry points can be specified by the **accept clause**, with the help of reserved words **accept** & **end.**

```ada
accept entry E (parameters) do  
[rendezvous operations]  
end entry E
```

Just like during the rendezvous, when an accept clause doesn’t want to accept a message, the sender's task is suspended. Similar to some other synchronization tools which we have discussed in the previous part (e.g. [Sysyphus](https://en.wikipedia.org/wiki/Semaphore_%28programming%29)), an accept clause keeps the suspended tasks in its queue.

If you still have difficulties while clearly understanding what **_rendezvous_** is, then I advise you to read [**this**](https://www.dcs.warwick.ac.uk/~sgm/cs237/lec/rendezvous/index.html) and [**this**](http://www.dalnefre.com/wp/2010/07/message-passing-part-1-synchronous-rendezvous/comment-page-1/#:~:text=By%20contrast%2C%20message%2Dpassing%20in,sender%20and%20receiver%20continue%20independently.)**.** It’s an important term that needs a perfect understanding. However, simply, _synchronous rendezvous_ is when both the sender and receiver agree to proceed synchronously.

Tasks that have graduated from a drama school are called **actor tasks**. They don’t need entry points, and hence, do not wait for a rendezvous to proceed. By contrast, a **server task** consists of a code within an accept clause and nothing outside. It can only react to other tasks.

If an Ada task sends a message, it needs to know the entry name of that task. However, in contrast to [CSP](https://en.wikipedia.org/wiki/Communicating_sequential_processes), a task entry doesn’t have to know the name of the task from which it will accept messages.

Tasks can be statically or dynamically created. **Statically created tasks** start their execution at the same time as the statements where the task is declared. If the task is declared in the main program, the execution of the task begins at the same time as the first statement in the main program.

A task with more than one entry point, requiring them to receive messages in any order, uses a **select** statement which encloses the entries.

```ada
task body task T begin  
  loop  
    select 
      accept entry E1 (parameters) do  
      [rendezvous operations]  
      end entry E1  
      or  
      accept entry E2 (parameters) do  
      [rendezvous operations]  
      end entry E2  
    end select  
  end loop  
end task T
```

The possible code between an accept clause and **or** (or between an accept clause and **end select**) is called **extended accept clause,** which is executed only after the preceding accept clause and is not a part of rendezvous.

As a way of **cooperation synchronization**, a guard in the form of a **when** (when not) clause may delay rendezvous. If the accept clause is **closed**, that is — if its boolean guard returns false, then no rendezvous can take place.

```ada
when Full(Buffer) =>  
accept entry E (parameters) do  
[rendezvous operations]  
end entry E
```

A special statement, **terminate**, can be used at the select clause, which, if selected would mean that the task has finished its execution but not terminated.

But now we need to understand how **competition synchronization** — mutually exclusive access to a resource — can be implemented at Ada. If a task controls access to a resource, then competition synchronization can be achieved by declaring the data structure within the task, as only one accept clause can be active at a time.

However, if tasks are nested, then the nested task may also access the shared data structure. Therefore, in order to keep the integrity of the data, synchronized tasks should not define other tasks.

**Section 13.6.3** introduces two great examples of competition synchronization, which I advise you to have a look at. There you will also find an example of an extended accept clause if it was not very clear for you from the definition, as it wasn’t for me.

**Protected objects** is an important concept of Ada 95. Although enclosing the data structure within a task implies competition synchronization, it undermines the efficiency of the rendezvous mechanism. Therefore, the usage of protected objects is an alternative (and better) way of implementing mutually exclusive access to a resource. It makes the code simpler and more efficient.

A protected object is more like a monitor, which is described in the previous part. It can be accessed either by **subprograms** or by **entries** that are similar to accept clauses.

Protected subprograms can arrive in two forms: _protected procedures & protected functions_. **Protected procedures** provide mutually exclusive read-write access, whereas **protected functions** provide concurrent read-only access to the data of the protected object.

Within the body of a protected procedure, the current instance of the protected unit is a variable, whereas, in protected function, it is defined as a constant which allows concurrent read-only access. Again, for the example, I highly encourage you to refer to the book **section 13.6.4.**

Entries differ from subprograms by their capacity for guard implementation. They use the reserved word **entry** instead of accept.

Finally, let’s briefly note that, in distributed systems, message passing is a better model of concurrency, when processes can execute parallelly in separate processors.

## Java Threads

Methods named **run** are the concurrent units of Java, which are executed in a **thread** process. Java threads are **lightweight tasks**, and as explained in the first part, lightweight tasks run in the same address space. In contrast, Ada tasks are **heavyweight tasks**. Java threads require less [overhead](https://en.wikipedia.org/wiki/Overhead_%28computing%29#:~:text=In%20computer%20science%2C%20overhead%20is,special%20case%20of%20engineering%20overhead.) than Ada tasks, however, due to being heavyweight, Ada tasks can be distributed to different processors with different memories on different computers in different places. When Java tasks can’t.

As the book points out, there are two ways to define a run method. One is defining a subclass of the predefined **Thread class** and override its run method. If the subclass has a necessary natural parent, we should define a subclass that inherits from its parent and implements the **Runnable interface,** which requires a **Thread object.** Any class that implements Runnable must define run.

class ClassName implements Runnable {...}

By contrast to Ada, in Java, all run methods are actors, and they communicate with each other with the help of the **join** method and through shared data

The [**Thread class**](https://docs.oracle.com/javase/7/docs/api/java/lang/Thread.html) is not the natural parent of any other classes and is the only class available for writing concurrent programs in Java. It includes five constructors, as well as methods and constants. Thread subclasses override its run method. The **start** method of Thread can start its thread after calling the run method, which may sometimes not work when the start method includes the initialization required.

```java
class JavaThread extends Thread {  
  public void run() {...}  
}

Thread th = new JavaThread();  
th.start();
```

Java **scheduler** determines which thread will run at a given time. Its algorithm is dependent on where it is implemented, however, the scheduler usually uses the round-robin algorithm, trying to give an equal time slice to each equally important thread.

Below are the descriptions of some common methods of the Thread class:

*   The **yield** method of the running thread requests processor usage, when it is immediately put in the task-ready queue. After that, the scheduler may choose it as the next thread to execute if it has the highest priority.
*   The **sleep** method can block a thread in an integer number of milliseconds given as a parameter, after which the thread it put in the task-ready queue.
*   The **join** method delays (blocks) a thread’s execution until a run method in another thread completes its execution. To prevent a possible deadlock in this case, join may have a parameter in milliseconds which will determine how long the calling thread will wait until the other thread completes. Now, if the other thread gets blocked somehow, the caller is not risking to fall into deadlock.
*   The **interrupt** method can tell a thread that it should stop its execution. It doesn't stop its execution, rather, it sends a message to the thread which sets a bit in the thread object. This bit is checked with the **isInterrupted** method, which may be missed if a thread is sleeping or waiting at the time when _interrupt_ is called. In that case, an exception — **InterruptedException** — is thrown, which awakens the thread.

Threads may have different **priorities**. A default priority of a thread is of the thread that created it. A thread created at main has the priority NORM\_PRIORITY, which is often 5. MIN\_PRIORITY and MAX\_PRIOIRITY usually have values 1 and 10. The method **setPriority** changes the priority of a thread, when the **getPriority** method returns its priority. The scheduler chooses threads based on their priorities.

The **Semaphore** **class** is defined at the package _java.util.concurrent.Semaphore_. It implements a counting semaphore that doesn’t have a queue for storing _thread descriptors_ (again, refer to the first and second parts). The two methods of this class are **acquire** and **release** (recall the Dutch vocabulary lesson).

As a way of **competition synchronization**, Java methods (not constructors) can be synchronized. Synchronized methods must acquire a lock that every Java object possesses before they are allowed to execute. Obviously, after the completion, the lock is released.

```java
public synchronized int getResource() {...}
```

When the number of statements dealing with shared data structure is significantly less than the number of other statements in the method, it is better to synchronize the code segment that changes the shared structure, instead of the whole method. As noted in the book, it can be down with the help of the following general-form statement:

```java
synchronized (expression) {... // statements}
```

The statement can be a **single** or **compound statement**, during the execution of which the object is locked.

Every object has an **intrinsic condition queue**, which is implicitly supplied by the synchronized methods that have attempted to execute on it while it was being operated upon by another synchronized method. After a synchronized method completes its execution, a method in the intrinsic condition queue is transferred into the task-ready queue.

An alternative to intrinsic condition queue is the **Condition interface**. Not only it uses a queue associated with the Lock object, but it also declares **await,** **signal,** and **signalAll**, alternatives to _wait,_ _notify,_ and _notifyAll_ which we will discuss in a second. As _notifyAll_ is an expensive operation, here, the more efficient signal method can be used instead.

**Cooperation synchronization,** on the other hand, is implemented with the **wait, notify,** and **notifyAll** methods, which are defined in the root class of all Java classes called **Object.**

If a thread calls _wait_ on an object, then they are put into the wait list of that object. The _notify_ method is called to tell a waiting thread that an even it may have been waiting for has occurred. Even though, _notifyAll_ is more often used than _notify_, which transfers all of the threads from an object’s wait list to the task-ready queue.

These three methods can be used within a synchronized method. The wait method is always called in a while loop that depends on a condition that the method is waiting. The wait method and any other code that calls wait catches _InterruptedException._

```java
try {  
  while (!someCondition)  
    wait();  
   ...  
}  
catch(InterruptedException e) {...}
```

For Java cooperation/competition synchronization example, refer to the book **section 13.7.5.**

**Non-blocking synchronized** access classes to primitive type variables, references, and arrays, are defined in the _java.util.concurrent.atomic_ package. An atomic class ensures uninterrupted (atomic) operations, so the locks are not required. It is called _fine-grained synchronization_ — just a single variable, and it doesn’t require suspension and rescheduling of threads.

An alternative to synchronized method and blocks which use implicit locks is **explicit locks.** There are three methods in the **Lock interface**, declared as **lock, unlock,** and **trylock**. ReentrantLock class implements the Lock interface.

```java
Lock lock = new ReentrantLock();

Lock.lock();  
try {   
  ...   
}   
finally {   
 Lock.unlock();   
}
```

The **finally** clause guarantees that the lock is unblocked in any case.

Explicit locks are needed in the following cases:

1.  If it is needed to acquire a lock but the application cannot wait for it forever, then the **tryLock** method is used. It has a time parameter. If the lock is not acquired within the specified time limit, then the statements after tryLock are executed.
2.  When flexibility in the structure of the program is demanded. Implicit locks demand unlock at the end of the statement in which they are located. Explicit locks can be unblocked anywhere in the code.

A danger of using explicit locks is that it is possible to omit an unlock. While in implicit locks it is not the case.

## C# Threads

Based on Java threads with some important differences.

C# methods can run in their own thread instead of in the method named run. C# threads, after their creation, are associated with predefined **ThreadStart** delegate.

A **Thread object** is created whenever we create C# threads.

```csharp
public int MethodMan(int x) { ... }

Thread th = new Thread(new ThreadStart(MethodMan));  
//th is delegate
```

In C#, just like in Ada, and unlike Java, threads have two categories: **actors** and **servers.** Actor threads are not called but started, and their execution must be requested through the method **Start**.

Similarly, the **Join method** in C# does the same as what **join** does for Java — forces a thread to wait for another thread to finish its execution.

The public static method of Thread, **Sleep,** is similar to Java implementation, with the difference that it doesn’t throw any exceptions, and hence, no need for a _try_ block.

```csharp
th.Start();                                     
th.Join();    // Join can also take a time paremeter  
th.Sleep(100) // time parameter in milliseconds
```

The **Abort** method terminates a thread, which, just like Java implementation of _interrupt,_ doesn’t actually kill the thread, but throws _ThreadAbortException_.

A **server thread** runs when called through its delegate. These threads interact with other threads, provide some service, and must have synchronized execution with other threads.

In C#, the delegate method named **Invoke** is a synchronous call, which is the same as the so-called default call of a function.

`myFunc()` is identical to `myFunc.Invoke()`

The asynchronous call is initiated through the delegate instance method **BeginInvoke**, which has two distinct parameters in addition to the parameters of the delegate method: _AsyncCallback_ and other of type **_object_.** BeginInvoke returns an object implementing _IAsyncResult interface. EndInvoke_ instance method is also defined which we will see in the example. _IsCompleted_ property determines if the thread finished its execution or not.

Let’s take the method we have created in the beginning of the section dedicated to C# and make an asynchronous call.

```csharp
public int MethodMan(int x);

Thread th = new Thread(new ThreadStart(MethodMan));

IAsyncResult res = th.BeginInvoke(10, null, null);

while(!res.IsCompleted) { ... }

int val = EndInvoke(res);
```

**Synchronization** of C# threads can take place in three different forms:

1.  Interlocked class
2.  Monitor class
3.  lock statement

The **Interlocked class** is used when only operations are incrementing and decrementing of an integer. The atomic _Increment_ and _Decrement_ methods use an integer as a parameter.

Interlocked.Decrement(ref counter);

The **lock statement** marks a critical section in a thread.

```csharp
lock(token) { ... }
```

The **Monitor class** of the namespace _System.Threading,_ defines five methods:

1.  Enter
2.  Wait
3.  Pulse
4.  PulseAll
5.  Exit

**Enter** marks the beginning of synchronization, **Wait** suspends the execution and instructs the _Common Language Runtime (CLR)_ of .NET that thread wants to resume its execution. The **Pulse method** notifies a waiting thread that it can run again. **PulseAll** does the same thing as _notifyAll_ in Java. **Exit** ends the critical section.

The lock statement is compiled into a monitor and is a shorthand for a monitor.

For well-explained differences between Java and C# threads, I will direct you to the book **section 13.8.3** because I have no other job to do. Although, let’s note that, unlike Java, lock variables don’t need to be unblocked, which makes the code more error-proof in C#.

![](https://cdn-images-1.medium.com/max/800/1*a-zmvuYgqLPtvF-4H9pv5A.jpeg)

_Wreckers Coast of Northumberland (1836) by_ [_J. M. W. Turner_](https://en.wikipedia.org/wiki/J._M._W._Turner)

The cover image by a painter with an amazing name, as Medium.com was cropping the main image to its specific parts and I couldn’t fix that.