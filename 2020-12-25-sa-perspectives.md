---
title: "Software Architecture Concepts: Architectural Perspectives"
description: Software Systems Architecture book summary on architectural perspectives.
categories: [computer science]
tags: [SSA, software architecture, book summary, medium]
---

![](https://cdn-images-1.medium.com/max/800/0*w5yCEWTuVUZCWIIP)
_Photo by [JOHN TOWNER](https://unsplash.com/@heytowner?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)_

In the [introductory article](/posts/sa-intro), we have already described some key concepts about software architecture. Now it’s time to dive in and learn about a concept called perspectives, which is a very important notion for a software architect to understand.

Even though [**views and viewpoints**](https://frmusazade.medium.com/software-architecture-views-and-viewpoints-113d1592fe3a) are powerful, the need for specifying quality properties remain. On this matter, **architectural perspectives** come in handy. Instead of defining yet another viewpoint to describe the quality properties of a product, we define something “orthogonal” to viewpoints — perspectives (coined by the [**Software Systems Architecture**](https://www.viewpoints-and-perspectives.info/) authors, Nick Rozanski & Eoin Woods):

> An **architectural perspective** is a collection of architectural activities, tactics, and guidelines that are used to ensure that a system exhibits a particular set of related quality properties that require consideration across a number of the system’s architectural views.

Although perspectives are often referred to as _cross-cutting concerns_ or _nonfunctional requirements,_ it is preferable to not use the latter term.

**Architectural tactics** may have a goal of achieving satisfactory system performance by defining different processing priorities for different parts of the system’s workload. The concept of tactic should not be mistaken for design pattern. Tactic is a thing more general and less constraining.

A perspective provides a framework. You never work with perspectives in isolation but use them with each view of your architecture. That is, you _apply_ a perspective to a view.

## Important Perspectives

According to Rozanski and Woods, the following are the most important perspectives for large information systems:

*   Security
*   Performance and Scalability
*   Availability and Resilience
*   Evolution

Applying perspectives as early as possible to the design of the architecture is often preferable. The aforementioned book defines the perspectives in the following standard manner:

*   **Applicability.** Which views are most likely to be affected by the perspective?
*   **Concerns.** What are the quality properties that the perspective addresses?
*   **Activities.** Which steps are there for applying the perspective to the views? — identifying quality properties, analyzing views against them, making decisions to improve the views.
*   **Architectural tactics.** What are the most important tactics for achieving its quality properties?
*   **Problems and pitfalls.** What are the most common things that can go wrong and how to avoid them?
*   **Checklists.** What is the list of questions that will help you to make sure that you have addressed everything above?
*   **Further reading.** What are the sources for further information?

## Applying Perspectives

Although it could be the case that all the perspectives are applied to all the views, usually, due to time and resource constraints, only some perspectives are applied to some views. An example table for applying perspectives is as follows:

![](https://cdn-images-1.medium.com/max/800/1*R1b_qj8_SDqV-wl36O67Pg.png)

## Insights, Improvements, Artifacts

Applying perspectives can lead to insights, improvements, or artifacts.

### Insights

Applying a perspective almost always leads to the creation of some sort of a model, which demonstrates whether the architecture meets its required quality properties or if it is deficient in some areas.

### Improvements

If insights tell you that some quality properties are unmet, then the architecture needs improvements. You may need to change the existing model in a view, or create additional models, or perhaps both.

### Artifacts

Some outcomes are going to be discarded, while others, which we call artifacts, should be preserved, as they keep important architectural information. Artifacts are usually captured as documents, models, or implementations.

## Perspective Pitfalls

Although perspectives are essential, there could arise some pitfalls along the way. For example, there can be conflicts between solutions suggested by different perspectives. Therefore, choosing perspectives wisely is a software architect’s main responsibility.

## References & Further Reading

*   [Software Systems Architecture](https://www.viewpoints-and-perspectives.info/) (book)
*   [Introduction to Software Architecture Concepts](/posts/sa-intro) (article)
*   [Software Architecture: Views and Viewpoints](https://frmusazade.medium.com/software-architecture-views-and-viewpoints-113d1592fe3a) (article)