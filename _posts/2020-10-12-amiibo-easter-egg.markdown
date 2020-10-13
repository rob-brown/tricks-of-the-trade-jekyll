---
layout: post
title:  "Amiibo Easter Egg"
author: Robert Brown
date:   2020-10-12 17:58:00 -0600
categories: amiibo
---
While delving down into the bits of Amiibo, I found what I believe is an Easter egg in the Amiibo.

## Hexadecimal

Before I go further I need to explain different number systems. If you are already familiar with hexadecimal, then feel free to skip this section.

Typically we work in decimal (base 10). This is likely because we have 10 fingers. There are other number systems frequently used. Computers work in binary (base 2). Effectively all 0s or 1s. Long strings of 0s and 1s are hard to read. By converting binary to hexadecimal (base 16), we can make it more manageable. Hexadecimal uses 0-9 and A-F. This changes one byte from eight binary characters to two hexadecimal characters. To distinguish hexadecimal numbers from decimal numbers, they are often prefixed with `0x`, ex. `0x1234`.

## Amiibo Character Numbers

Every Amiibo contains eight bytes that identify what character the figurine, card, or plush represents. For the purposes of this post, I'm going to focus on the first two bytes (16 bits).

The first 12 bits indicate the game series, ex. [Super Mario Bros.](https://amiibo.life/amiibo/super-smash-bros) and [Legend of Zelda](https://amiibo.life/amiibo/the-legend-of-zelda). The remaining four bits determine the character. Together these 16 bits are typically the same for Amiibo of the same character. For example, [Link's Awakening Link](https://amiibo.life/amiibo/the-legend-of-zelda/link-link-s-awakening) and [Majora's Mask Link](https://amiibo.life/amiibo/the-legend-of-zelda/link-majora-s-mask) share the same 16 bits (`0x0100`). The remaining 6 bytes (48 bits) differentiate them from each other.

## Pokémon Character Numbers

Typically, the character numbers are assigned sequentially. For example, the Super Mario Bros. series starts with `0x000` to `0x002`. The first several characters look like this:

| Character | Number |
|:---|:---|
| Mario | `0x0000` |
| Luigi | `0x0001` |
| Peach | `0x0002` |
| Yoshi | `0x0003` |
| Rosalina and Luma | `0x0004` |
| Bowser | `0x0005` |
| Bowser Jr. | `0x0006` |
| Wario | `0x0007` |
| Donkey Kong | `0x0008` |
| Diddy Kong | `0x0009` |

The Pokémon series doesn't follow that pattern. Let's look at a few of them:

| Character | Number |
|:---|:---|
| Ivysaur | `0x1902` |
| Charizard | `0x1906` |
| Squirtle | `0x1907` |

2, 6, and 7 are not sequential but any longtime Pokémon fan will recognize them. They are the national Pokédex numbers for those Pokémon.

Let's look at the other Pokémon and subtract `0x1900` from each of their numbers.

| Character | Number | Number - `0x1900` | National Pokédex |
|:---|:---|---:|---:|
| Ivysaur | `0x1902` | 2 | 2 |
| Charizard | `0x1906` | 6 | 6 |
| Squirtle | `0x1907` | 7 | 7 |
| Pikachu | `0x1919` | 25 | 25 |
| Jigglypuff | `0x1927` | 39 | 39 |
| Mewtwo | `0x1996` | 150 | 150 |
| Pichu | `0x19AC` | 172 | 172 |
| Lucario | `0x1AC0` | 448 | 448 |
| Greninja | `0x1B92` | 658 | 658 |
| Incineroar | `0x1BD7` | 727 | 727 |

It's a perfect match.

You may claim this is just careful organizing, not an Easter egg. It probably is. Though it's a lot of wasted assignable numbers for Pokémon that haven't been released. The only good reason I can see to do this would be if Nintendo released a large number of Pokémon Amiibo. This could be done through cards like the [Animal Crossing cards](https://amiibo.life/amiibo/animal-crossing-cards-series-1). It would be interesting to see [Pokémon TCG cards](https://en.wikipedia.org/wiki/Pokémon_Trading_Card_Game) scannable as Amiibo.

## One More Thing

Here's the thing that really caught my attention. Does `0x1900` have any significance? Look through the character numbers again and one might stand out. Mewtwo's character number is `0x1996`. Any longtime Pokémon fan will recognize that number. 1996 is the [year Pokémon Red, Green, and Blue released in Japan](https://en.wikipedia.org/wiki/Pokémon_Red_and_Blue). This could be complete coincidence, especially with it being Mewtwo of all Pokémon. Still it's too good to ignore.

## Wait, What About Pokémon Trainer?

Actually, there are three Amiibo I haven't mentioned: [Pokémon Trainer](https://amiibo.life/amiibo/super-smash-bros/pokemon-trainer), [Shadow Mewtwo](https://amiibo.life/amiibo/pokemon/shadow-mewtwo), and [Detective Pikachu](https://amiibo.life/amiibo/pokemon/detective-pikachu). They don't fit the convention of the other Amiibo.

Personally I would have assigned Pokémon Trainer the number `0x1900`. That would avoid interfering with Pokédex numbers. Instead Nintendo assigned him the number `0x1D40`. Subtracting `0x1900` gives `0x440` or 1088. I don't see any significance to those numbers. At the time of this writing, the highest national Pokédex number is [Zarude at 893](https://bulbapedia.bulbagarden.net/wiki/Zarude_%28Pokémon%29). Give it a few years and we may have a conflict on this number.

The Shadow Mewtwo card and Detective Pikachu figurine are particularly unusual. Nintendo assigned them the numbers `0x1D00` and `0x1D01`, respectively. Typically Nintendo assigns the same character number to different Amiibo even if they come from different Amiibo series. For example, the [Hammer Slam Bowser figurine](https://amiibo.life/amiibo/skylanders-superchargers/hammer-slam-bowser) from the [Skylanders SuperChargers series](https://amiibo.life/amiibo/skylanders-superchargers) has the character number `0x005` matching the [Bowser](https://amiibo.life/amiibo/super-mario/bowser) and [Wedding Bowser](https://amiibo.life/amiibo/super-mario/bowser-wedding-outfit) figurines from the Super Mario Bros. series.

Another oddity is that Shadow Mewtwo and Detective Pikachu don't work as trainable Amiibo in Super Smash Bros. Ultimate. Scanning Shadow Mewtwo disappointingly does nothing and Detective Pikachu gives you a spirit as consolation.

Calculating `0x1D00` - `0x1900` gives 1024. Detective Pikachu is one later at 1025. These are also outside the current national Pokédex. Any programmer will quickly notice that 1024 is a power of 2 (2^10). Outside of that I see no significance.

## Conclusion

This post is already longer than I anticipated. I hope you enjoy this fun discovery hidden in the depths of Amiibo.
