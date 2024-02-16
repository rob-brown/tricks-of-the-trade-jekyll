---
layout: post
title:  Anatomy of an amiibo ID
author: Robert Brown
date:   2024-02-15 18:51:00 -0600
categories: amiibo
---
## Intro

You may have heard about data miners finding game details and releasing them before the game is ready. One such thing is amiibo IDs.
However, I've been guessing amiibo IDs long before data miners even found them. Often times a game has amiibo support added before
the amiibo is released. Guessing the IDs is fun for unlocking new items and behavior before the amiibo figure is available.

With the release of Sora for Super Smash Bros. Ultimate, I'm going to reveal how I've been guessing the amiibo IDs. Once Nintendo
finds this article, future IDs may be less guessable. Though this is a fun exercise in reverse engineering and knowing the
process is enlightening.

## DISCLAIMERS

The bulk of the data for this article came from [AmiiboAPI](https://github.com/N3evin/AmiiboAPI) with the rest coming from my own research.

I do not condone cloning amiibo and selling them as your own. Even though I unlock features early, I subsequently buy amiibo once
they do become available.

## Structure

The structure of amiibo IDs has been known for a long time. Here's the ID for Zelda and Loftwing released with the remaster of
The Legend of Zelda: Skyward Sword.

```
01010300 04140902
```

That's the ID represented in [hexadecimal](https://en.wikipedia.org/wiki/Hexadecimal) form. Here are the parts broken out:

`010`: Game series = The Legend of Zelda

`1`: Character = Zelda

`03`: Variation = Zelda with Loftwing

`00`: Form = Figure

`0414`: Serial number = 414 hexadecimal 1044 decimal

`09`: amiibo series = The Legend of Zelda

`02`: Format version = 2

The first three bytes (`010103`) contain the key information for the character. The remaining five bytes (`0004140902`) contains metadata.
Most games only look at the first two or three bytes. This allows them to be compatible with future amiibo without updating the game.

The rest of this post will dive into the details of each portion.

## Game Series

Not to be confused with the "amiibo Series". The game series represents where the character originated from, ex. Link -> The Legend
of Zelda. For game series with many amiibo, there will be multiple game series IDs assigned. How this portion of the ID is more elusive
and will be covered in detail later in the article.

## Character and Variation

The character is the most important information in the amiibo ID. This is often the only information a game is concerned about.

For a single game series ID, there are 15 possible amiibo. Some series have much more than that. So when it exceeds 15, it add a new
game series ID. The subsequent series ID is usually sequential. However, The Legend of Zelda series uses `010` but some Breath of
the Wild figures use `014`.

Speaking of The Legend of Zelda, here are some characters:

* `0` Link
* `1` Zelda
* `2` Ganondorf
* `3` Midna and Wolf Link
* `4` Daruk
* `5` Urbosa
* `6` Mipha
* `7` Revali

You might be thinking, where is the Sheik amiibo? This is where variation comes in. Zelda is variation `00`. Sheik is variation `01`
(25 year-old spoiler...Sheik is Zelda).

The variation is used to distinguish the "Player 2" figures. Specifically the Super Smash Bros. Cloud, Corrin, and Bayonetta figurines.

The variation has also been used to distinguish game-specific amiibo. For example, Zelda & Loftwing, Cat Peach, and Cat Mario are
different variations from their base character. Scanning these amiibo into their respective games gave different rewards than
scanning other variants.

The use of variants is not consistent. For example, shortly after the Skyward Sword and Super Mario 3D world remasters release came
Metroid Dread. The Metroid Dread Samus amiibo gives different rewards than other Samus amiibo but it's not a variant. This means that
Metroid Dread must look at the full amiibo ID to distinguish this amiibo.

To further confuse matters, Toon Link is a variant of Link. However, there is no Young Link variant. Super Smash Bros. Ultimate
treats "Link" and "Young Link" as different characters. So, the game must look at the full ID. This means if there is a new
"Young Link" amiibo released, it will be treated as Link unless the game is updated.

Whoever made the Skylander amiibo completely ignored the variation and serial number. Hammer Slam Bowser and Dark Hammer Slam Bowser
share the same ID. Same for Turbo Charge Donkey Kong and Dark Turbo Charge Donkey Kong.

Currently, the character with the most variations is Isabelle from Animal Crossing. She has three figures and seven cards wearing
the following six outfits.

* `00` Summer
* `01` Winter
* `02` Kimono
* `03` Dress
* `04` Tropical
* `05` Sweater

### Character ID

Though I treat game series, character, and variation as three different things they really get mashed into one thing: the character
ID. Let's take character ID for Pikachu: `191900`. It can be helpful to think of the game series having four digits, in this case
`1900`. Then the character number is 19 hexadecimal or 25 decimal. Adding `1900` and `19` gives the base ID `1919`. Append the
variation `00` to get the full character ID of `191900`.

## Form

The form determines their physical representation. The options are few but Nintendo occassionally surprises us with new forms.

* `00` Figure
* `01` Card
* `02` Yarn
* `03` Band

You've likely seen the figures. They are the most common. You may have also seen cards such as the many Animal Crossing cards.

The other types are less common. With the Yoshi series came several figures crocheted from yarn. This is a reference to Yoshi's
Wooly World where Yoshi and others are made of yarn.

The newest form are wrist bands. You receive these at the Super Nintendo World theme parks. This is a clever way to introduce more
people to the amiibo functionality within games.

## Serial Number

By itself, the serial number is mundane. It's simply a sequentially-assigned number. What's more interesting is in what's missing.
There are several gaps. These gaps could indicate canceled amiibo. Or some card series carved out more numbers than needed.
More on this below.

## amiibo Series

Not to be confused with the "Game Series". For amiibo figures, this determines the base the figure stands on. Each base has a
slightly different design. The most notable difference between game series and amiibo series is the Super Smash Bros. series.
It contains characters from a variety of games, ex. Legend of Zelda, Mega Man, Pikmin, Metroid.

These are the current series:

| amiibo Series ID | Series Name |
|:---|:---|
| `00` | Super Smash Bros. |
| `01` | Super Mario |
| `02` | Chibi-Robo! |
| `03` | Yoshi |
| `04` | Splatoon |
| `05` | Animal Crossing |
| `06` | 8-bit Mario |
| `07` | Skylanders |
| `09` | Legend Of Zelda |
| `0A` | Shovel Knight |
| `0B` | ... |
| `0C` | Kirby |
| `0D` | Pokemon |
| `0E` | Mario Sports Superstars |
| `0F` | Monster Hunter |
| `10` | BoxBoy! |
| `11` | Pikmin |
| `12` | Fire Emblem |
| `13` | Metroid |
| `14` | Others |
| `15` | Mega Man |
| `16` | Diablo |
| `17` | Power Pros |
| `18` | Monster Hunter |
| `19` | Yu-Gi-Oh! |
| `1A` | ... |
| `1B` | Xenoblade Chronicles |
| ... | ... |
| `FF` | Super Nintendo World |

## Format Version

The final byte is the "format version". So far it's always `02`. If a new format is ever made, then this number could be
different. It's not likely, so just expect this to always be `02`.

## Game Series Assignment

Now to dig into the most critical data, the game series. For existing series, it's trivial to look up the game series ID. But
for new games, how can you guess the series ID?

The game series numbers are arbitrarily assigned. Fortunately, people (especially programmers) like organization. I've
deduced some patterns through what I call "playing solitaire with the data". Basically categorizing data and finding
similarities.

I'll start by dumping a large table of data then explain what it means.

| Series ID | Name | Publisher/Developer |
|---:|:---|:---|
| 000-002 | Super Mario Bros. | Nintendo |
| 004 | x |
| 008 | Yoshi | Nintendo |
| 00C | Donky Kong | Nintendo |
| 010 | Legend of Zelda | Nintendo |
| 014 | Breath of the Wild | Nintendo |
| 018-051 | Animal Crossing | Nintendo |
| 054 | x |
| 058 | Star Fox | Nintendo |
| 05C | Metroid | Nintendo |
| 060 | F-Zero | Nintendo |
| 064 | Pikmin | Nintendo |
| 068 | x |
| 06C | Punch Out | Nintendo |
| 070 | Wii Fit | Nintendo |
| 074 | Kid Icarus | Nintendo |
| 078 | Classic Nintendo | Nintendo |
| 07C | Mii | Nintendo |
| 080 | Splatoon | Nintendo |
| ... | ... | ... |
| 09C-09D | Mario Sports Superstars | Nintendo |
| ... | ... | ... |
| 0A4 | ARMS | Nintendo |
| ... | ... | ... |
| 190-1D4 | Pokémon | Nintendo/GameFreak |
| ... | ... | ... |
| 1F0 | Kirby | Nintendo/HAL Labs |
| 1F4 | BoxBoy | Nintendo/HAL Labs
| ... | ... | ... |
| 210 | Fire Emblem | Nintendo |
| ... | ... | ... |
| 224 | Xenoblade | Nintendo/Monolith Soft |
| 228 | Earthbound | Nintendo |
| 22C | Chibi Robo | Nintendo |
| ... | ... | ... |
| 320 | Sonic | Sega |
| 324 | Bayonetta | Sega/Platinum Games |
| 328 | x |
| 32C | x |
| 330 | x |
| 334 | Pac-Man | Bandai Namco |
| 338 | Dark Souls | Bandai Nameco |
| 33C | Tekken | Bandai Namco |
| 340 | x |
| 344 | x |
| 348 | Mega Man | Capcom |
| 34C | Street Fighter | Capcom |
| 350 | Monster Hunter | Capcom |
| 354 | x |
| 358 | x |
| 35C | Shovel Knight | Yacht Club Games |
| 360 | Final Fantasy | Square Enix |
| 364 | Dragon Quest | Square Enix |
| 368 | x |
| 36C | x |
| 370 | x |
| 374 | Super Mario Cereal | Kellogs |
| 378 | Metal Gear Solid | Konami |
| 37C | Castlevania | Konami |
| 380 | Jikkyou Powerful Pro Baseball | Konami |
| 384 | x |
| 388 | x |
| 38C | Diablo | Blizzard |
| 390 | x |
| 394 | x |
| 398 | x |
| 39C | x |
| 3A0 | Persona | Atlus/Sega |
| 3A4 | x |
| 3A8 | x |
| 3AC | x |
| 3B0 | x |
| 3B4 | Banjo and Kazooie | Rare/Microsoft |
| 3B8 | x |
| 3BC | x |
| 3C0 | x |
| 3C4 | x |
| 3C8 | Fatal Fury | SNK |
| 3CC | x |
| 3D0 | x |
| 3D4 | x |
| 3D8 | x |
| 3DC | Minecraft | Mojang/Microsoft |
| 3E0 | x |
| 3E4 | x |
| 3E8 | x |
| 3EC | x |
| 3F0 | Kingdom Hearts | Disney/Square Enix |
| 3F4 | x |
| 3F8 | x |
| 3FC | x |
| 400 | x |

The series IDs `000`-`319` are reserved for Nintendo. They don't appear to follow a distinct pattern. So I won't say more about them.

`320` and beyond are for third-party developers. Notice that I set up the table so that each entry ends with `0`, `4`, `8`, or `C` (12 decimal).
I call each entry a "slot". One slot is can have 64 amiibo (ex. `3740`-`377F`) ignoring variants. Smaller publishers (ex. Yacht Club
Games, maker of Shovel Knight) are assigned a single slot. Larger publishers that make multiple games are given five slots. This is
enough for 320 amiibo (ignoring variants) across five game series.

The latest amiibo, Sora from Kingdom Hearts, was assigned series ID `3F0`. Following this pattern, if a new publisher released an amiibo,
it's series ID would be `404`.

You might think this is a large waste of space slicing out five slots at a time. However, from `404` to `FFC` inclusive, there are 767 slots.
That's enough for 153 more major publishers. amiibo have been around for almost a decade. The numbers aren't running out soon.

## Possible Cancelations

The following tables show gaps in the serial numbers. There's also some speculation about what might have used those numbers. At the time of
writing, the latest serial number is `043E`.

| Range | Size | Speculation |
|:---|---:|:---|
| `3B` | 1 | Possibly Chibi-Robo amiibo |
| `1D9`—`237` | 95 | Probably Animal Crossing cards, increase 405 to 500 |
| `23C` | 1 | Maybe another Skylander or Pokémon |
| `2C3`—`2E0` | 30 | Probably more Mario Sports Superstars, increase 90 to 120 |
| `31F`—`34A` | 44 | Probably Animal Crossing cards, increase 56 to 100 |
| `351` | 1 | Probably a Toon LoZ amiibo (Ganondorf?) |
| `357` | 1 | Probably a BotW amiibo (Calamity Ganon?) |
| `39A` | 1 | Probably a Link's Awakening amiibo (Marin?) |
| `3D8`—`40B` | 52 | Probably Animal Crossing cards, increase 48 to 100 |
| `416`—`417` | 2 | Possibly Splatoon 3 or Tears of the Kingdom |
| `42F` | 1 | More Power-Up bands? (Link?) |
| `431`—`432` | 2 | More Power-Up bands? (Donkey Kong? Diddy Kong?) |
| `436`—`43C` | 7 | Maybe Splatoon 3 or Xenoblade but probably not |

## Conclusion

For just being eight bytes of data, there is a lot that can be inferred. It's similar to how the
[periodic table of elements came about](https://en.wikipedia.org/wiki/Periodic_table#History). Or the
[German tank problem](https://en.wikipedia.org/wiki/German_tank_problem).
Next time you see a mess of data, see if you can tease out those little patterns.
