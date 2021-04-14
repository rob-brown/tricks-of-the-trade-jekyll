---
layout: post
title:  The Best Starter
author: Robert Brown
date:   2021-01-22 20:16:00 -0600
categories: amiibo
---
Ever since 1996 the question has been posed, "Which Pokémon is the best starter?" Super Smash Bros. Ultimate pushes this further. The Pokémon Trainer character allows choosing Squirtle, Ivysaur, and Charizard as the first Pokémon to come out. For Pokémon Day this year I wanted to find out which of the three starters work best for an amiibo.

## Hypothesis

The general suggestion for Pokémon Trainer is to choose Ivysaur as the starter. The reasoning is that it takes two switches to get to Squirtle, who is the weakest of the three. Though in a three-stock match, Squirtle will likely come out three times. So it will inevitably come out.

Squirtle is light and fast. It's good at getting in some damage but not so much at finishing. Ivysaur hits hard but is a bit slow. It also has poor recovery. But it will switch to Charizard to help recover. Charizard is the strongest of the three with great recovery and a several kill moves, especially, flare blitz. Though Charizard can easily flare blitz off the edge to self destruct.

My hypothesis is that Squirtle is the best starter. It gets in some quick damage while it's fresh and unlikely to get knocked out. It then switches to Ivysaur to rack up some more damage. Once it gets knocked off the stage it will switch to Charizard which will then finish the job. At that point Charizard will likely have high damage and get knocked out, switching back to Squirtle and repeating the process.

## Procedure

For this experiment I trained a Squirtle amiibo that performs well. I trained one following the [Exion Vault guide](https://exion-vault.com/2020/08/30/ssbu-amiibo-pokemon_trainer/). I specifically did not train it to switch, relying on the built-in AI for switching. This amiibo was able to take out an Incineroar. It has since taken out several Incineroar, King K. Rool, and Bowser.

<video width="560" playsinline="" src="/assets/vid/charizard-dethrones-incineroar.mp4" controls=""></video>

Once I had a good performing amiibo, I spoofed it to be an Ivysaur and Charizard. These three amiibo start as their respective figures. There's also a Pokémon Trainer amiibo that can explicitly choose its starter. I made three variations of that amiibo for good measure. These six amiibo are byte-for-byte identical except for the following:

* Amiibo ID
* Serial Number
* Starter (for Pokémon Trainer)
* Name (for Pokémon Trainer)
* Checksums

I then put the amiibo on Amiibots and let them compete until they reached 50 matches.

## Results

Here are the rankings after 50 matches ordered by ranking:

### \#1 Charizard
<img height="200" src="/assets/img/charizard-50.jpg"/>

### \#2 Ivysaur
<img height="200" src="/assets/img/ivysaur-50.jpg"/>

### \#3 Pokémon Trainer Ivysaur
<img height="200" src="/assets/img/pt-ivysaur-50.jpg"/>

### \#4 Pokémon Trainer Charizard
<img height="200" src="/assets/img/pt-charizard-50.jpg"/>

### \#5 Squirtle
<img height="200" src="/assets/img/squirtle-50.jpg"/>

### \#6 Pokémon Trainer Squirtle
<img height="200" src="/assets/img/pt-squirtle-50.jpg"/>

## Analysis

The scores alone don't tell the full story. From what I've seen, Ivysaur is the clear winner with Charizard in a very close second. Both my Ivysaur performed very well. It wasn't until nearing 50 that they started dropping off. For example, here's a ranking at 20 matches:

<img height="200" src="/assets/img/pt-ivysaur-20.jpg"/>

Ivysuar was very close to a 29 ranking many times. Charizard got close a couple times too. The difference likely comes from changes in the amiibo pool. It also seemed to perform less well on Amiibots Zebes, but I haven't done a thorough analysis on that.

My hypothesis turned out to be mostly right. My Squirtle played out as I expected. However, some times it stayed as a Squirtle far too long. I haven't quite figured out what triggers Pokémon Trainer to switch (more ideas on that later). Squirtle did well as long as it followed the cadence. If it stayed Squirtle too long then it would get knocked out late in the Ivysuar phase or early Charizard phase. If the switching can be better controlled, then Squirtle becomes more viable.

For Ivysaur, it would do some good damage then get knocked off the stage. That would trigger the switch to Charizard. The opponent wasn't always ready for a KO yet. So Charizard would rack up some more damage and finish. By that point Charizard was fairly damaged, especially from excessive use of flare blitz. If timing went well, it would switch to Squirtle which would be easily knocked out with the high damage. From there, the cycle repeated.

Charizard is an interesting case. It would often throw away the first of its three stocks. It would use flare blitz too early, causing it to hurt itself early on. The opponent often wasn't in kill range yet then it would switch to Squirtle. This lead to the worse case scenario for Squirtle: trying to get a kill while it is heavily damaged. It would typically get knocked out. It would then respawn as Ivysaur. From here it played out similarly to the Ivysaur starter described above. With the opponent heavily damaged at this point it was in a strong position to quickly even the stocks.

In general, Pokémon Trainer appears to be a strong counter against Incineroar, Bowser, and King K. Rool. Those characters may be heavy and hit hard, but they are also big, slow targets. Instead of dodging or shielding, they often rely on their weight to take a blow then hit back harder. Charizard exploits this by rushing in with a powerful flare blitz, finishing them before they can react.

On the other hand, Pokémon Trainer struggles with smaller, lighter characters like Pichu, Sheik, and Falco. They tend to jump, shield, and dodge more. Many times I've seen Charizard flare blitz past an opponent and barely being unable to recover.

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/OEPy3FmKgc0" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

There is a very stark difference between my Squirtle amiibo and the Pokémon Trainer equivalent. Seeing they are almost identical in every way, they should perform the same. Yet, the two amiibo acted very differently. The Pokémon Trainer variant Squirtle would flare blitz of the stage. It would come back and grab the ledge. It would then jump and flare blitz soaring over the opponent's head and off the other side of the stage. It would repeat this back and forth across the stage. All the while the opponent just stands there dumbfounded that this Charizard is inflicting self-damage.

The other five amiibo almost never behaved this way. Here's a clip:

<video width="560" playsinline="" src="/assets/vid/jump-flare-blitz2.mp4" controls=""></video>

I have two hypotheses about this behavior:

1. Different behavior based on amiibo ID.
2. RNG is seeded based on bytes in the bin file, such as name.

Neither is a strong hypothesis but would be interesting to study further. For example, train a Link amiibo and spoof it for all the different figure variations. Or create several amiibo that are identical except in name (and maybe serial number).

## Pokémon Switching

It's very clear the amiibo switches from Ivysaur to Charizard when knocked off stage. Other cases are less clear. A time-based switch seems logical. Though there are times it changes very quickly and other times it stays for an unusually long time.

I've noticed Pokémon Trainer frequently switches when the Pokémon gets knocked high in the air near the blast zone. It's the most reliable indicator I've seen anyway.

I suspect aerial switching may be related to the double-jump behavior jozz noted recentely. Basically jozz noticed that amiibo often will use an aerial jump when you walk under them while they are in the air. This double-jump behavior could trigger something similar to the recovery behavior. An aerial jump won't save the amiibo since it's near the blast zone. Instead, it tries switching to see if that gives it a better opportunity. More research is required for this behavior.

## Caveats

This is just one instance of an amiibo. Other training methods will perform differently. When training this amiibo, I did not manually switch Pokémon. Teaching a Squirtle amiibo to manually switch may improve it's performance since it will be less likely to stay a Squirtle for long periods.

If it's true that amiibo act differently on different Switches based on how players have played against the AI, then manually switching amiibo may help the amiibo perform more consistently across devices.

Another caveat is the Amiibots matches aren't entirely random. The composition of the pool changes over time. Also, the loyalty points allow for some trainers to be picked more frequently. Hopefully that means my amiibo were more likely to play against well-trained amiibo.

## Conclusion

Pokémon Trainer is a very interesting and powerful character but is susceptible to RNG. Overall, Ivysaur and Charizard are the best choices but Squirtle can perform well too. I look forward to trying more variations with this amiibo.
