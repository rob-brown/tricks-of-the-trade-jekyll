---
layout: post
title:  "Rubber Duck Development"
date:   2017-05-07 09:23:00 -0600
categories:
---
[Rubber duck debugging](https://en.wikipedia.org/wiki/Rubber_duck_debugging) is a common technique for finding bugs. In essence, one explains step-buy-step how the code works. Often the explaining process is sufficient to discover the error. In my experience, developers use rubber duck debugging after they are burnt out from other debugging techniques. If building up a thorough understanding is so useful, why wait until debugging to do it?

## How

During the design phase of software development I detail how some feature/component/library/algorithm X should work. Here are some aspects I include:

1. List any assumptions.
2. List any pre-conditions, post-conditions, and invariants.
3. Detail step-by-step how X should work.
4. List the possible failures.
5. Describe how to handle each failure.
6. Describe how X interacts with other parts of the product.

## Why

By going through this process, I often foresee any problems before they happen. I can ensure the error is handled appropriately. It can also save time from going down a wrong path. Other times it may help me find a way to do without X.

This process also makes the development process almost mechanical. All the thinking was upfront. When writing an algorithm, for example, I simply do the following:

1. Create a base function.
2. Write a function stub for each step and call it in the base function.
3. Implement each stubbed function.
4. Test the results.

As an added benefit, I have my thought process documented. When I re-visit some part of an app, I can more rapidly regain my previous mental model. If someone else needs to understand something I built, he or she can see exactly what I was thinking.

## What About Iterative Problems?

Not all problems can be solved upfront. They require iterative exploration. Document your thought process as you go. Once you've come up with a working solution, circle back and run through this process. You may be surprised by what you missed.

Remember, take time upfront to think out a problem. Understanding a problem and it's potential solutions can save or avoid much time lost in debugging.

---

#### Note

I use a programmer's notebook called [Quiver](http://happenapps.com/#quiver). (Disclaimer, I'm *not* compensated to endorse Quiver.) It has great tools such as tags and full-text search. I use it to keep daily and topical notes. Other programmer's notebooks can work just as well. A simple text editor can work, but it requires more effort to organize.
