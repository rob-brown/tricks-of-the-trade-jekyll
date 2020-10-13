---
layout: post
title:  "Single Responsibility Principle"
author: Robert Brown
date:   2016-08-20 12:09:00 -0600
categories:
---
When I was a child I would look at electronic circuits and wonder. How they could be so complicated? How could someone comprehend such a vast system? How could someone even know where to start?

Since then, I've learned that programming is the same. Though there are no component-studded circuit boards to look at. Programs are made of ethereal bits generated from numerous text files.

Now that I am older and wiser, I am able to answer my childhood questions. There are many principles for managing vast complexity. There is one I feel is more important: the single responsibility principle (SRP).

First, it's important to understand what SRP is. Bob Martin, who coined the term, defines it as

> "A class should have one, and only one, reason to change."
>
> -- [The Principles of OOD](http://www.butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod)

Though Bob Martin invented the concept, I don't like his definition. It's too ambiguous. There can be many  "reasons to change." For example, a class may need to change due to performance concerns. Or someone in your organization changed your code linting rules.

To remedy the ambiguitiy, I developed my own definition of SRP.

> Everything should do one thing (and do it well).
> Nothing else should do that one thing.

These statements are notably vague. This is intentional. Whether it be a system, service, application, file, class, or function, these rules can be applied at the appropriate granularity. For example, I could restate these rules like so

> Every function should perform one operation.
> No other function should perform that operation.

or

> Every class should represent one idea.
> No other class should represent that idea.

These statements also reveal two parts of SRP. The first statement covers [separation of concerns (SoC)](https://en.wikipedia.org/wiki/Separation_of_concerns). The second statement covers [cohesion](https://en.wikipedia.org/wiki/Cohesion_%28computer_science%29).

Simply put, cohesion means like items stick together and SoC means unlike items remain separate. The result is code that is [modular](https://en.wikipedia.org/wiki/Modular_programming). This means the pieces are joinable and interchangable. This sounds just like the electrical components mentioned earlier.

Modularity works at different levels of granularity too. In electronics, transistors make up logic gates. Logic gates make up circuits. Circuits make up systems.

Similarly in software, functions make up classes. Classes make up modules. Modules make up applications. Applications make up systems.

These hierarchies in hardware and software aren't entirely correct. However, the idea is clear. Smaller pieces are assembled to make bigger pieces.

This is how complexity is managed. By creating higher levels of abstractions, we can think, communicate, design, and build without worrying about lower levels of complexity. Occassionaly, we may need to dip down to lower levels but that's the exception not the rule.

I'll end with this quote from Edsger Dijkstra:

<a href="http://www.azquotes.com/picture-quotes/quote-the-purpose-of-abstraction-is-not-to-be-vague-but-to-create-a-new-semantic-level-in-edsger-dijkstra-70-1-0184.jpg" class="embedly-card" data-card-width="100%" data-card-controls="0">Embedded content: http://www.azquotes.com/picture-quotes/quote-the-purpose-of-abstraction-is-not-to-be-vague-but-to-create-a-new-semantic-level-in-edsger-dijkstra-70-1-0184.jpg</a>

Again, there are many ways to manage complexity. The Single Responsibility Principle is one of the most important. For some other ways to manage complexity, check out *[The Principles of OOD](http://www.butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod)*.
