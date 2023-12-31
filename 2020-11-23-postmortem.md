---
title: "Blameless Postmortem Culture: How Google Turns Failures into Successes"
description: Google’s Best Practices on Site Reliability Engineering
categories: [computer science]
tags: [sre, google, posmortem, medium]
---

![](https://cdn-images-1.medium.com/max/800/0*gdi5kuVDbwF6K7Mb)
_Photo by [Kai Wenzel](https://unsplash.com/@kai_wenzel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)_

> "I never learn. I either lose or win." ~ Unknown

## Google’s Postmortem Philosophy

Mistakes are inevitable. Even the highest concentration can produce irrational errors. If left unchecked, these errors can multiply, triply, and lead to catastrophic outcomes. What is under our control, however, is that we can fix the problems on time and re-establish the status quo. **We can also document our mistakes, define important conclusions, and make it available for others to learn from.**

For this, we need a **well-defined process based on best practices**.

A great book, published by Google and freely available for online reading, [**Site Reliability Engineering: How Google Runs Production Systems**](https://sre.google/sre-book/table-of-contents/) defines postmortem as follows:

> “A postmortem is a written record of an incident, its impact, the actions taken to mitigate or resolve it, the root cause(s), and the follow-up actions to prevent the incident from recurring.”

As the goal standing behind the concept of postmortem is clear now, let’s jump on to understand what actually **blameless postmortem culture** is and how Google approaches the postmortem culture to reap the best out of it.

## What Triggers Postmortems

Postmortems take time and effort. Therefore, **not only it is a matter of interest to conduct a postmortem, it is even more crucial to know when to do it.**

> “Postmortems are expected after any significant undesirable event.”

From my own experience as an International Chess Master, I know that strong chess players don’t analyze all the games at the same depth. The lost games demand more attention. Also, the mistakes in a drawn game, when you couldn’t manage to convert your winning advantage, are usually more significant than the mistakes in a won game.

Similarly, there is no need to conduct a postmortem after every minor incident in a company. The well-known [Pareto principle](https://en.wikipedia.org/wiki/Pareto_principle) is valid here. Therefore, you should clearly define criteria on what exactly makes an undesirable event significant, so that you will not be throwing stones at every barking dog.

Common **triggers for a postmortem**, defined in the aforementioned book, are as follows:

*   User-visible downtime or degradation beyond a certain threshold
*   Data loss of any kind
*   On-call engineer intervention (release rollback, rerouting of traffic, etc.)
*   A resolution time above some threshold
*   A monitoring failure (which usually implies manual incident discovery)

## Blameless Postmortem Culture

Originated from avionics[^1] and healthcare, blameless culture focuses on identifying the causes of an incident without pointing fingers at an individual or a group. That is, **postmortems should assume that everyone in the team had good intentions and did their best with the information they had.** The blamelessness of a postmortem creates a nurturing environment for individuals and teams to admit their mistakes on time without the fear of punishment.

> “Best Practice: Avoid Blame and Keep It Constructive”

## Conducting Postmortems

There are a couple of important suggestions one should take into account while conducting postmortems.

First and foremost, it must be **real-time**. The collection of data and ideas must be rapid and easily accessible. Google docs can be a helpful tool in this regard. It’s also preferable to have **email notifications**. Overall, everything that eases the **accessibility** and **transparency** of a postmortem is a good practice.

## Reviewing Postmortems

When the first draft is ready, an important step is to review the postmortem and make it ready for publishing. Usually, senior engineers of a company assess the draft for its completeness based on **predefined criteria**. The SRE book suggests the following template criteria:

*   Was key incident data collected for posterity?
*   Are the impact assessments complete?
*   Was the root cause sufficiently deep?
*   Is the action plan appropriate and are resulting bug fixes at appropriate priority?
*   Did we share the outcome with relevant stakeholders?

> “Best Practice: No Postmortem Left Unreviewed”

**Once the reviewing is finished, the postmortem is added to the team’s or organization's repository.** It makes it possible for others to learn from already committed mistakes. As mentioned already, transparency is very important. An example Google postmortem document can be found in [this link](https://sre.google/sre-book/example-postmortem/).

## More Postmortem Culture

Google implements several methods to encourage postmortem culture in the company. For example, at the end of each month, the **Postmortem of the month** is published in the monthly newsletter.

Furthermore, Google moderates the **Google+ postmortem group** to keep everyone in the company informed about the significant events and the lessons learned from them. The group members discuss different postmortems and share their opinions about the best practices of postmortem.

**Postmortem reading clubs** host regular meetings, discuss the recent (or not recent) postmortem events and the aftermath of incidents. Participants have constructive dialogue about lessons learned and the conclusions of the postmortem.

Last but not least, in a **Wheel of Misfortune** exercise, similar to [Disaster Role Playing](https://landing.google.com/sre/sre-book/chapters/accelerating-sre-on-call#xref_training_disaster-rpg), a previous postmortem is simulated, when engineers have similar role-playing.

## More Postmortem Best Practices

Another best practice is rewarding people in front of others for admitting their mistakes. That nurtures a blameless postmortem environment in the company.

> “Best Practice: Visibly Reward People for Doing the Right Thing”

It is also advisable to track the effectiveness of each postmortem. Asking for feedback is stated as the final best practice in the SRE book:

> “Best Practice: Ask for Feedback on Postmortem Effectiveness”

## The Future of Postmortem

Finally, postmortem culture must be constantly improved. For example, the implementation of **machine learning** to predict the company’s weaknesses, and **not allowing duplicate mistakes** will be worth the effort.

## References and Further Reading

*   The chapter this article is based upon, written by John Lunney and Sue Lueder, edited by Gary O’ Connor: [https://sre.google/sre-book/postmortem-culture/](https://sre.google/sre-book/postmortem-culture/)
*   Google’s books on the Site Reliability Engineering: [https://sre.google/books/](https://sre.google/books/)
*   An article by John Allspaw on Blameless Postmortem and a Just Culture: [https://codeascraft.com/2012/05/22/blameless-postmortems/](https://codeascraft.com/2012/05/22/blameless-postmortems/)
*   An example postmortem: [https://sre.google/sre-book/example-postmortem/](https://sre.google/sre-book/example-postmortem/)

## Footnotes

[^1]: Pointed out by [Thewriteyard](https://medium.com/u/e67f52d1e55c).