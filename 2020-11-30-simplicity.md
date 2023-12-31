---
title: Google’s Best Practices on Site Reliability Engineering
description: Explaining Site Reliability Engineering on Code Simplicity.
categories: [computer science]
tags: [sre, google, simplicity, medium]
---

![](https://cdn-images-1.medium.com/max/800/0*9CsRuoE_gUfararD)
_Photo by [Safar Safarov](https://unsplash.com/@codestorm?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)_

> "First principles, Clarice. Simplicity. Read Marcus Aurelius. Of each particular thing ask: what is it in itself?" ~ Anthony Hopkins

## Simplicity is predecessor of reliability

It might as well be Google’s most prolific realization.

## Boring Doesn’t Necessarily Mean Bad

J. D. Zeik and David Mamet’s [Ronin (1998)](https://en.wikipedia.org/wiki/Ronin_%28film%29), one of the greatest spy movies of all time, lacks unnecessary details: each word and action in the movie leads to somewhere. Certainly, Mamet is a son of Hollywood and he is incapable of understanding Nouvelle Vaguers. [He calls Godard boring](https://www.youtube.com/watch?v=fHgxci2_OGs&feature=youtu.be), which probably is true, but he can’t notice that **boring doesn’t necessarily mean bad.**

It turns out, most of the things that are true for movies are false for software development, but probably except for one: **boringness — not only could be but actually IS a positive attribute of software.**

Surprises are not good; you don’t want to be spontaneous in your coding, you need predictability, lack of excitement. By not following the virtue of boredom, you start adding unnecessary complexities into the system, which reduces its reliability.

## Kill Your Darlings

It’s probably Stephen King’s most famous writing advice — **no matter how good your characters or paragraphs are, you need to possess the boldness to get rid of them if they are unnecessary and distract the reader from the main plotline.** David Mamet follows that advice (of course, he didn’t learn it from Stephen King). Contemporary Hollywood follows that advice. I have probably even read that advice in Hemingway’s “A Moveable Feast”. Even if there are great writers who don’t give a damn about this rule, they still follow it time after time.

In **Site Reliability Engineering**, killing your darlings is a must. Commenting your code out? Hell no, delete it! No need for adding unnecessary complexities into your code, as [it may explode at some point](https://www.sec.gov/litigation/admin/2013/34-70694.pdf).

## Skin in the Game

Alright, Nassim Nicholas Taleb isn’t good, but the term went mainstream. You need to live your life according to your own philosophy, have skin in the game, or what you say loses its credibility. Google knows it as well. For example, the [**Go programming language**](/posts/go-dart) that was developed at Google has strict rules for its syntax: you can’t declare a variable or import a library if you won’t use them in the future. Or: Go lacks a useful feature called generics just because it would add complexity to its language design. There must be something about simplicity that Google programmers and engineers try to strictly abide.

## Software Bloat & Negative Coding

> “The greater the number of laws and enactments, the more thieves and robbers there will be.” (Lao-Tzu)

As software evolves, it becomes slower and bigger due to the necessity of adding new features. It is called **software bloat**, and its shortcomings are even more obvious from the SRE perspective. **The more complex your code gets there is more chance for new defects and bugs to emerge.**

**Negative coding** comes in handy here. Similar to killing your darlings, it is a good practice to time after time return back to your code and **delete hundreds of lines if they are no longer useful.**

## Minimal APIs

> “A small API is a hallmark of a well-understood problem.”

## Modularity is Simplicity

The best practices of object-oriented programming are best for a reason; that rules also apply to the design of distributed systems. **You need to be able to make changes (e.g. fix bugs) to the parts of the system in isolation without affecting the entire system.** The more complex the system gets, the more crucial it is to separate responsibility between APIs and between binaries.

## Release Simplicity

Generally, **it is better to make simple releases rather than complex ones.** As a specific example, when you commit your code into a version control system, it is advisable to have extremely simple tags that track changes only in one area. Having lots of changes in one release that are unrelated makes it difficult to understand the program in the long run. Another example is **gradient descent** in machine learning when we search for a solution, taking small steps at a time and considering its impact on the system.

## References & Further Reading

*   The chapter this article is based upon, written by Max Luebbe,  
    edited by Tim Harvey: [https://sre.google/sre-book/simplicity/](https://sre.google/sre-book/simplicity/)
*   Google’s books on the Site Reliability Engineering: [https://sre.google/books/](https://sre.google/books/)
*   My previous article on: [Site Reliability Engineering and Google’s Blameless Postmortem Culture:](/posts/postmortem)
*   My previous article on: [Site Reliability Engineering and Frontend Load Balancing](/posts/load-balancing)
*   Stoicism went mainstream and is definitely not as good as it seems like, but still — “First principles, Clarice. Simplicity. Read Marcus Aurelius”: [http://classics.mit.edu/Antoninus/meditations.html](http://classics.mit.edu/Antoninus/meditations.html)