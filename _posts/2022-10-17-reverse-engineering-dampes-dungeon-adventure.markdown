---
layout: post
title:  Reverse Engineering Dampé’s Dungeon Adventure
author: Robert Brown
date:   2022-10-17 20:49:00 -0600
categories: amiibo
---
## Intro

Dampé’s Dungeon Adventure is my favorite feature in the Link’s Awakening remake. Since the dungeons can be stored on an amiibo figurine, the data is available to be easily read from it. I’ve been meaning to decode that data. Now three years later I finally got to it. It took about a week to decode the data for this post. Most of that time was [cataloging the 187 available chambers](https://www.icloud.com/numbers/0f0fVIs3UOVA_-RFASWRlPB_w#Link's_Awakening_Dungeon_Catalog).

## Dungeon Data

Like all games that store data to an amiibo, the game has 216 bytes available. Link’s Awakening more or less uses that full space. I will go through each of the significant portions of that data. My explanations will use hexadecimal format, meaning that values are base 16 using 0-9 and A-F.

### Basic Info

The first four bytes are in the following format: `0x03XX0100`. Three of the four bytes appear to always be the same. The `XX` portion changes depending on the challenge chosen. Dampé has 24 challenges numbered `0x00` to `0x17`. There’s also a free-play mode which has the value `0x30`. So, must dungeons you see on amiibo will probably start with `0x03300100`.

The tutorial levels are mostly the same as the free-play mode. The main difference is that they may have some extra restrictions: no sword, limited hearts, or limited time. There are also chamber placement restrictions. Some chambers are placed by Dampé. In the chamber data, these locked chambers are indicated by a `0x0000` chamber value (more on this in a moment).

The next four bytes are a mystery. I haven’t decoded them yet. They remain constant for a dungeon arrangement. Changing the dungeon changes these bytes, even if the same chamber is placed in the same spot. It seems like a random ID or perhaps a salt to make cryptanalysis on the tamper protection harder. Any time these bytes change, the dungeon must be played through to save it to an amiibo.

### Chamber Data

The next 128 bytes are the most interesting. This is the chamber data. A dungeon is an 8x8 grid (up to 64 chambers). Each chamber gets two bytes. The chambers are stored in row-major format, meaning all of the first row is stored. After that is the second row and so on.

Looking at the raw data, a chamber will look like this: `0x6201`. The data is stored in little-endian format. Basically that means the least-significant byte is stored first. This can be more efficient for computers to work with but the bytes are backwards from how we would normally read them. So, let’s flip it around: `0x0162`.

There are 4 hex digits. We’ll ignore the first digit for now. The latter three are the important bits that determine the room. In short, these three digits represent coordinates: dungeon, y-coordinate, and x-coordinate.

The dungeons are numbered `0x0` to `0xA`. If you look at the catalog of chambers, you’ll notice dungeon `0x8` is not used. That is the Wind Fish’s egg. There are no chambers from that dungeon.

Now for a brief history lesson. The original release of Link’s Awakening had the Wind Fish’s Egg as the final dungeon. In Link’s Awakening DX (released on Game Boy **Color**, there was a new dungeon released: the **Color Dungeon**. This is dungeon `0x9`. It is optional, though you can play through it fairly early in the game if you know where it is.

But wait, I said it goes to `0xA`. What about that? Dungeon `0xA` isn’t a dungeon. It’s a grab bag of extra chambers. They are mostly some mini-bosses from the mini-dungeons.

Now for the x-y coordinates. You’ll notice that the y-coordinate comes first though we typically use the x-coordinate first. Remember the chambers are stored in row-major order. The y-coordinate determines the row. Then the x-coordinate picks the chamber within that row.

The x-y coordinates are not coordinates in your dungeon. Rather they are the coordinates of the chamber in the original dungeon.

If these are coordinates, then what about dungeons with multiple floors? There are two dungeons with multiple floors: The Key Cavern with two floors and the Eagle’s Tower with “four” floors.

If you map out all the chambers for the Key Cavern, they look like this:

<img height="200" src="/assets/img/key-cavern.jpg"/>

Doing the same for the Eagle’s Tower:

<img height="200" src="/assets/img/eagles-tower.jpg"/>

There’s an interesting thing to note about the Eagle’s Tower. The fourth floor is unreachable. You have to collapse the fourth floor into the third floor in order to reach it. In reality, you are probably traveling to and from the third and fourth floors. Internally, the game is probably re-mapping the stairs and doors after you break the pillars. It’s a similar trick to how the multiple floors works in the first place to make a three-dimensional dungeon into a two-dimensional dungeon (which is really just a one-dimensional list of chambers anyway).

The keen eye will notice these two dungeons still fit into an 8x8 grid. This means you could logically create a multi-floor dungeon yourself even though Dampé’s map will show it as a single floor.

There are a few chambers that don’t exist in the original dungeons. The primary chambers being where you can play one of the nightmares as if they were a mini-boss. If you check these coordinates against the dungeon maps, they point to a location with no chamber. Perhaps through modding or glitches you could get to these chambers in the original dungeons. Or maybe it’s just a convenient organization system. There are a few chambers that don’t perfectly match their original counterparts.

That was a long explanation of the coordinate system. As promised, back to the first hex digit. Dampé’s Dungeon Adventure lets you add “plus effects” to a chamber. This can alter it in some way such as doubling the enemies or randomly dropping bombs/hearts/rupees. Here are the values of each plus effect:

| Effect | ID |
|:---|---:|
| No effect | 0 |
| Bombs | 1 |
| Rupees | 2 |
| Hearts | 3 |
| Wallmaster | 4 |
| Double enemies | 5 |
| Shadow Link | 6 |

The Wallmaster and Shadow Link enemies are limited to one per dungeon. The other effects can be added multiple times. Though the double enemy modifier may only be used on chambers with enemies.

Since the numbers are continuous instead of powers of 2 (1, 2, 4, 8, …) naturally a chamber may only have one effect. They cannot stack.

### Bookkeeping Data

After the chamber data are two names, each up to 32 bytes. The first name is the owner who created the dungeon. The second is the name of the best completion time. They are almost certainly the same name unless you pass the data to another account via amiibo.

Note that the names are from the Nintendo Switch account name, not the Link’s Awakening save file name.

The names are store in UTF-8 format. It’s a complicated format but here’s a brief explanation. A character (called a grapheme) can be made of one or more code points. A code point can be between 1-4 bytes. All the ASCII characters (think Roman letters, Arabic numerals, common symbols) all fit in one byte. Accented letters, other alphabets, less-common symbols are more than one byte.

Nintendo Switch account names are limited to 10 characters. Seeing that a single code point can be up to four bytes and graphemes can have more than one code point, it’s conceivable that you could overflow the 32-byte limit on these names. On the Nintendo Switch keyboard, I don’t think there are any characters you can type that is more than two-bytes long. So 32 bytes is plenty.

The next data field is the best completion time. It is a four-byte integer (little endian) measured in centiseconds. Yes, centiseconds. The game never shows more than two decimal points. So there’s no need to use a more standard unit like milliseconds. This make the value more compact.

Even though four bytes could store a larger value, the timer is capped at 59:59.99. I made a full 64-chamber dungeon, including every boss and mini-boss. I was able to complete that labyrinth well under the hour limit.

Note that you must first play through and beat a dungeon before you can save it to an amiibo. There will always be a completion time.

<img width="95%" src="/assets/img/labyrinth.png"/>

### Equipment

The next four bytes (really just two) are also interesting. They contain all the equipment you have when you saved the dungeon. Since you can’t get a chamber until you’ve beaten the associated dungeon, you will always be able to beat it (barring any mods or sequence skips). This also ensures that someone else who plays the dungeon will be on the same footing as you. So they can’t get a faster completion time just by having better equipment.

The equipment is stored in a bit field. This means each binary digit determines the gear you have. 1 means you have it, 0 means you don’t.

Even though this is a little endian number, I’m going to treat it as big endian for simplicity. It seems to fit better treating it that way. Here are the values of each item. Some values are guessed because you have to beat two dungeons before Dampé lets you build your first dungeon.

| Equipment | Value |
|:---|---:|
| Sword | 1 |
| Magic Powder | 2 |
| Roc’s Feather | 4 |
| Power Bracelet | 8 |
| Ocarina | 16 |
| Bottle from Ghost | 32 |
| Bottle from Fishing | 64 |
| Bottle from Dampé | 128 |
| Pegasus Boots | 256 |
| Shield | 512 |
| Bomb | 1024 |
| Bow | 2048 |
| Hookshot | 4096 |
| Boomerang | 8192 |
| Magic Rod | 16,384 |
| Shovel | 32,768 |

The next four bytes work the same. I call these the enhancements. They change your equipment. For example, upgrading the Power Bracelet to Powerful Bracelet, having a fairy in a bottle, or learning a song on the ocarina.

| Enhancement | Value |
|:---|---:|
| Fairy in bottle 1 | 1 |
| Fairy in bottle 2 | 2 |
| Fairy in bottle 3 | 4 |
| Secret Medecine | 8 |
| Flippers | 16 |
| Ballad of the Wind Fish | 32 |
| Mango’s Mambo | 64 |
| Frog’s Song of Soul | 128 |
| Koholint Sword | 256 |
| Mirror Shield | 512 |
| Powerful Bracelet | 1024 |
| Double Magic Dust | 2048 |
| Double Bombs | 4096 |
| Double Arrows | 8192 |
| Red Mail | 16,384 |
| Blue Mail | 32,768 |

The red and blue mail are mutually exclusive. Through modding you could give yourself both. Not sure how the game would handle that visually.

### Tamper Protection

The final four bytes are a CRC. Basically it’s a value that can be used to verify the data hasn’t been tampered with. In order to modify the data on an amiibo, you would also need to update this value accordingly. Otherwise, the data will be rejected. Super Smash Bros. Ultimate has a similar CRC for its data. Though it puts the CRC as the first four bytes instead of the last four.

## Missing Data

So what’s missing? The number of hearts you have are not recorded. So someone playing a shared dungeon could have more or less. This could give them an advantage to get a faster time but a good player won’t get hurt much anyway.

There are no time limits. That’s a feature strictly of a few Dampé challenges. Setting the dungeon type to one of those challenges might get you a time limit.

The challenges that don’t allow you a sword don’t actually change the equipment. That’s also a feature of the Dampé challenges. Though conceivable you could modify the equipment list to take away the sword and other equipment/enhancements.

There’s no way to name a dungeon. Your dungeon will automatically be named “Shrine/Cave/Maze/Labyrinth of [Name]”. The name is determined by the number of chambers in the dungeon.

| Classification | Number of Chambers |
|:---|---:|
| Shrine | 3-10 |
| Cave | 11-20 |
| Maze | 21-30 |
| Labyrinth | 31-64 |

No descriptions either. These are determined by the dungeon level. Only Dampé’s challenges have semi-meaningful descriptions.

There is no creation date (for dungeons you've made) or received date (for chambers you've received via amiibo). Both of those are stored in the game itself.

How stairs are connected are not in the data. Instead, they recalculated each time a chamber is placed. The stairs are shown in one of the four corners of the tile. The game then calculates the distance between every pair of stairs. It then sorts them by distance (ascending). When distance is equal, it sorts by row (ascending). When row is equal, it sorts by column (ascending). It then runs through the list and pairs up the stairs. Since it uses a greedy algorithm you can fabricate where all the stairs are close together but one or more pairings are far apart. This would not be a good algorithm to minimize digging tunnels. Though it can be used to draw a happy little tree.

<img width="95%" src="/assets/img/dungeon-stairs1.png"/>

<img width="95%" src="/assets/img/dungeon-stairs2.png"/>

<img width="95%" src="/assets/img/dungeon-stairs3.png"/>

The UI has seven distinct colors assigned in this order:

1. Green
2. Cyan
3. Magenta
4. Red
5. Yellow
6. Blue
7. Orange

Knowing that order, you can follow along with the screenshots above to confirm the algorithm works as described.

## Resources

For full details of the chambers, [check out the chamber catalog](https://www.icloud.com/numbers/0f0fVIs3UOVA_-RFASWRlPB_w#Link's_Awakening_Dungeon_Catalog). It almost certainly contains errors or missing details. There is a lot to note for 187 chambers.

I also used HexFiend for viewing the files. It has a feature to use TCL scripts to parse out binary data. I've included my script below. Note that the amiibo data must first be decrypted for this binary template script to be useful.

<script src="https://gist.github.com/rob-brown/fbff18427994325aeadd32af3afdc11c.js"></script>

## Updates

20 Oct 2022: Added details about the stair-pairing algorithm.
