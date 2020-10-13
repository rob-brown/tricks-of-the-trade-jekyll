---
layout: post
title:  "Naming Colors"
author: Robert Brown
date:   2016-10-03 21:00:00 -0600
categories: ios
---
It's often said that the 2 hardest problems in Computer Science are:

1. Naming things
2. Cache invalidation
3. Off-by-one errors

The problem I want to focus on is naming things, specifically colors.

I've frequently seen colors named like this: `red1`, `red2`, `red3`. Basically, there are three different shades of red. The names alone don't differentiate between them.

Other projects will name colors slightly better, ex. `red`, `darkRed`, `darkerRed`, `evenDarkerRed`. This naming convention tells you how the color relate to each other. It sadly breaks down if there are too many similar colors.

The solution is to use more specific names, ex. `salmon`, `coral`, `maroon`, and `scarlet`. The challenge is that most people can't correctly identify fuchsia (let alone spell it).

To remove human judgement, I've been using a [tool intended for the color blind](http://www.color-blindness.com/color-name-hue/). You just enter the RGB value of the color, and it gives you the closest color name. A quick search reveals other, [similar tools](http://gauth.fr/2011/09/get-a-color-name-from-any-rgb-combination/).

Different tools report slightly different colors. Find one you like and stick with it. This ensures consistency and removes ambiguity. Your team will thank you.
