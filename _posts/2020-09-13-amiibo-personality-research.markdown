---
layout: post
title:  "Amiibo Personality Research"
author: Robert Brown
date:   2020-09-13 12:41:00 -0600
categories: amiibo
permalink: /posts/amiibo-personality-research
---
## Summary

Amiibo work by randomly selecting moves. By emphasizing effective moves and punishing ineffective moves, we influence the probability distributions of which moves are chosen. As said in The Hunger Games the phrase "May the odds be ever in your favor" is aptly applicable to Amiibo. We can't make a perfect Amiibo but we can push the odds as far in our favor as possible.

An Amiibo's behavior is defined by two parts: move set and personality. When asked about personalities, the immediate responses are often "personality doesn't matter" or "you can win with any personality." The reasoning: how ever you train an Amiibo a suitable personality will follow. This is a very pragmatic approach. But what about an empirical approach?

The question remains is this really the best way to find a personality? Or is it a local maximum? There are several personalities that are hard to get without spirits so they are rarely seen. Could we push the odds 10% (or even 1%) in our favor by choosing a different personality?

## Procedure

To better understand how personalities work, I devised the following experiment. It's designed to limit as many variables as possible.

1. Create 8 Amiibo of the same character.
2. Reset each Amiibo.
3. Use spirits to create each of the personalities (ignoring Normal).
4. Scrub the Amiibo of any spirit stats (type, attack, defense).
5. Disable learning.
6. Copy the move set from a known, good Amiibo into each Amiibo.
7. Play each of the personalities against each other on Final Destination in a first to 5 wins match with three stocks.
8. Collect the stats.
9. Back out to the menu between matches to ensure any learning is cleared.

You'll notice I only created 8 Amiibo instead of 24 or 25. This is due to two things. First, without an Amiibots-like automated setup, running lots of matches is not reasonable. Having 24 Amiibo play each other would result in 276 matches. Using 8 only requires 28 matches, roughly a 90% reduction.

Second, the personalities are related. This flowchart from [ArklaineGenesis](https://twitter.com/ArklaineGenesis/status/1108550519847415808) shows how spirits can be used to change to different personalities.

<img src="http://tricksofthetrade.pro/images/amiibo-personality-guide.png" width="700">

Though when I ran through the process myself, I found an error. I was unable to get Thrill Seeker from Enthusiastic. I was able to get Thrill Seeker from Reckless. That leads me to believe the main personality branches look like this:

<img src="http://tricksofthetrade.pro/images/amiibo-personality-main-branches.png" width="700">

It has a nice symmetry to it. There are eight personalities with three different variations. As an aside, each character conveniently has three different skins. This made it easier to identify the Amiibo I created, along with naming them with the personalities.

It's simple to confirm these branches. If you start with a fresh Amiibo, look for a spirit that moves it from Normal to the first stage of a branch. If you give the Amiibo three of these spirits total, you should end up with the final stage of the branch.

For this experiment I chose the 8 final stage personalities:

* Daredevil
* Entertainer
* Lightning Fast
* Lively
* Offensive
* Sly
* Technician
* Unflappable

These personalities should be the most distinct.

As for the character, I chose Link for several reasons:

* Link is my main and I know him best.
* He is a high-tier Amiibo and one of the most common in tournaments.
* I have trained a Link Amiibo that performs decently.
* Link has a wide range of attacks: grounded/aerial and melee/range with lots of kill options. He even has items. Any personality should find a home with Link.

There are some caveats with this experiment.

* Different move sets may work better with different personalities.
* Different Amiibo may work better with different personalities.
* These matches were Link vs. Link. Other personalities may work better against different characters.
* Theses were vanilla matches. Spirits could change the results.
* Even with first to five wins there weren't many rounds run.
* The final stage personalities may be so specialized they aren't as effective. The first or second stage personalities may be more balanced.
* Personality labels are not granular. There are many underlying stats, ex. Aggression, Defensiveness, Edgeguard, Anticipation, and many more. Several of those are two bytes each. That gives over 65,000 possible values for each stat. Even within one personality there is a lot of variation.
* This means giving different spirits likely would have given slightly different personalities.

## Analysis

Enough about the procedure. Let's look at the results.

For each match I recorded wins/loses, KOs/falls, and damage given/received. From that I derived several key metrics.

The full data set is [here](https://gist.github.com/rob-brown/50463be906f7d6d98d54a58d0acc5dea).

### Match Wins

How many first-to-five matches the personality won.

<script src="https://gist.github.com/rob-brown/50463be906f7d6d98d54a58d0acc5dea.js?file=rank-by-match-wins.csv"></script>

### Win Rate

Matches won to total games.

<script src="https://gist.github.com/rob-brown/50463be906f7d6d98d54a58d0acc5dea.js?file=rank-by-win-rate.csv"></script>

### Damage Given Per Game

Average damage per game.

<script src="https://gist.github.com/rob-brown/50463be906f7d6d98d54a58d0acc5dea.js?file=rank-by-damage-given-per-game.csv"></script>

### Damage Given Per KO

Average damage a personality did to get kill. Lower is better. This metric may be more important than damager per game. This means a personality can get kills sooner.

<script src="https://gist.github.com/rob-brown/50463be906f7d6d98d54a58d0acc5dea.js?file=rank-by-damage-given-per-ko.csv"></script>

### Damage Received Per Fall

Average damage before dying. Higher is better. Shows how long a personality can survive.

<script src="https://gist.github.com/rob-brown/50463be906f7d6d98d54a58d0acc5dea.js?file=rank-by-damage-received-per-fall.csv"></script>

Surprisingly, the more aggressive personalities survived longest.

### KOs Per Game

Average number of kills per game. For a three-stock game, closer to 3 is best.

<script src="https://gist.github.com/rob-brown/50463be906f7d6d98d54a58d0acc5dea.js?file=rank-by-kos-per-game.csv"></script>

### KOs Per Fall

Number of kills per own death. Greater than 1 is good.

<script src="https://gist.github.com/rob-brown/50463be906f7d6d98d54a58d0acc5dea.js?file=rank-by-kos-per-fall.csv"></script>

## Conclusion

The Offensive personality is a clear winner with Lightning Fast in a close second. Offensive is top is every metric except one. Lightning Fast was able to kill at a slightly lower damage on average.

Sly only won a single game. It played too safely and just wasn't able to get reliable kills. These personalities show the best defense is a good offense.

Clearly personality does matter. Most people I've talked to end up with an Enthusiastic personality. With Enthusiastic in the Offensive branch, they are already in a good position. This could explain the sentiment that personality doesn't matter. If you always end up with a personality optimized for kills then you can focus more on tuning an Amiibo's move set. Again, it's a very pragmatic strategy.

This research would not have been possible without all the modders out there, especially fudgepop01. Thank you for your contributions to the Amiibo community.
