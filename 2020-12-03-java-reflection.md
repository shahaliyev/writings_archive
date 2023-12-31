---
title: The Class Class in Java. Introducing the Concept of Reflection
description: Discussing reflection in Java.
categories: [computer science]
tags: [java, medium, programming languages]
---

Before discussing the concept of reflection and its Java implementation, let me bring a quote from the book [Concepts of Programming Languages](https://www.amazon.com/Concepts-Programming-Languages-Robert-Sebesta/dp/0134997182) by Robert W. Sebesta:

> “A discussion of reflection is not a perfect fit into a chapter on object orientation, but it is even a worse fit into any other chapter of this book.”

As it begs, I am going to mock the concept of reflection throughout the essay while understanding its importance.

## Defining Reflection

Having run-time access to modify language types and structure can be useful. To put it in even more complicated terms, programs need to **introspect** (examine) its **metadata** (types and structure).

Reflection is primarily applied to the construction of software tools — IDEs, debuggers, class browsers, etc.

## The Class Class

As it often makes little sense to theorize much, let’s jump on to see how reflection is implemented in Java.

`java.lang.Class` namespace defines the primary class for metadata, the class Class, which provides methods for examining types and members of the program objects. Java run-time system instantiates an instance of Class for each object.

`getClass` obtains the Class object of an object. For example, `“hello”.getClass();` obtains the object of String.

If there is no object of a class, attaching `.class` to the end will obtain the class name: `String.class` The same can be done to a class with no name: `int[][].class`

## Four Methods

All I got is four methods. All these methods are needed to get the Class of a method.

The first method is the`getMethod` method. The `getMethod` method (defined in the class Class) searches a class to find a specific public method defined in a class or superclass.

The second method is `getMethods` method. The `getMethods` method (defined in the class Class) returns the array of all the public methods defined in a class or superclass.

The third method is `getDeclareMethod` method that searches for a specific method declared in a class.

The fourth method you can guess by yourselves.

## The Fifth Method

If the Class object of an object is known and a particular method is found, that method can be called through the Method object of the method with the invoke method: `method.invoke(...);`

## Drawbacks of Reflection

[Java documentation](https://docs.oracle.com/javase/tutorial/reflect/index.html) admits the power of reflection, but despite that, Java has a very limited capacity for it. The main issue of Java designers with reflection was probably its **security**. Reflection also doesn’t allow certain Java Virtual Machine optimizations, causing **slow performance**. Exposing private fields **violate the rules of abstraction** and can negatively **affect portability.**

## Drawforwards of Reflection

All the statically typed languages struggle with the aforementioned shortcomings of reflection. But the world is not bound to statically typed languages and reflection plays an integral role for most dynamically typed languages.

In [**Lisp**](/posts/lisp), for example, reflection is routinely used. Other interpreted languages, JavaScript, Perl, and Python, keep the symbol table during implementation for accessing useful type information.

## Conclusion

The Concepts of Programming Languages book doesn’t even discuss methods for constructors (e.g. `getConstructor` ), and the topic of reflection, despite being important, is very limited for Java programming language.