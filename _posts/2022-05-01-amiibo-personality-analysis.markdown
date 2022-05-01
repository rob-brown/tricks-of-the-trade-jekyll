---
layout: post
title:  "Amiibo Personality Analysis"
author: Robert Brown
date:   2022-05-01 07:02:00 -0600
categories: amiibo
permalink: /posts/amiibo-personality-analysis
---
## Summary

A while back, Ske reverse-engineered the SSBU amiibo personality calculation. The calculation is fairly complicated and this article will explain it to a level anyone else can understand.

## Personality Calculation

Amiibo have 66 different attributes that define their behavior. 34 of those directly affect their personality.

The personalities are broken up into eight different branches:

* `def`
* `agl`
* `ofn`
* `rsk`
* `gen`
* `ent`
* `cau`
* `dyn`

Each of these branches have a list of attributes that add or subtract points. The number of points depends on the level of the attribute. Attributes vary from 0.0 to 1.0.

Here's an example of the `def` branch.

If the amiibo's `offensive` attribute is above 0.26, then the `def` score is decreased by 18 points. If the `offensive` attribute is above 0.76, then the `def` score is further decreased by 20 points.

If the amiibo's `grounded` attribute is above 0.25, then the `def` score is increased by 18 points. If the `grounded` attribute is 0.76, then the `def` score is further increased by 18 points.

All the branches, except `dyn`, require an amiibo to either have or not have an attribute above a certain level. For example, the `rsk` branch must have `appeal` (taunt) above 0.25. The `def` branch must *not* have the `offensive` attribute above 0.26. If a branch does not meet this requirement, then it cannot be that personality.

Each of the branches has three personalities. The nominal personality is determined by the number of points scored for that branch. For example, if the `ofn` branch has over 235 points, then the personality is `Offensive`. If the score is over 180, then it's `Aggressive`. If the score is over 100, then it's Enthusiastic.

SSBU calculates the score for each of the branches. Whichever branch has the highest score, defines the amiibo's personality. If no personality meets the minimum requirements, then an amiibo is defined as `Normal`. There's also a special case. If an amiibo has attributes all equal to 0, then it it `Normal`.

## Personalities

The following lists some of the requirements for each of the personalities. For the full details, see the [raw data](https://github.com/jozz024/smash-amiibo-editor/blob/main/resources/personality_data.json). For attribute details, see [this guide](https://docs.google.com/document/d/1L3c-QKr46ATTSxaicPHNFq5uW-uRytVViPRvdM93IQo/edit). For a visual representation, check out [this spreadsheet](https://www.icloud.com/numbers/045RmNMqRpVCxSSivldqLtWsg#Amiibo_Personalities).

### Normal

This personality is the least interesting. It's the default if no other personality fits.

### def (Cautious, Realistic, Unflappable)

#### Description

This personality must use its shield a lot and walk on the ground. It must not not do anything too aggresive like going off stage. It shouldn't run and collecting items is too risky. Overall, this is a hard personality to get because many attributes subtract from it and not many add.

#### Must NOT have

`offensive` >= 0.26

#### Increases

* `grounded`
* `shield master`
* `shield catch master`

#### Decreases

* `offensive`
* `dash`
* `cliffer`
* `item throw to target`
* `dragoon collector`
* `smashball collector`
* `special flagger`

#### Personality Scores

* `Unflappable` >= 240
* `Realistic` >= 180
* `Cautious` >= 100

### agl (Light, Quick, Lightning Fast)

#### Description

This personality must move fast. It must be aggressive but not in the air. Collecting items is ok but anything that requires charging or would leave the amiibo in a vulnerable state is too risky.

#### Must have

`dash` >= 0.26

#### Increases

* `offensive`
* `dash`
* `dash attacker`
* `item collector`
* `carrier broker`

#### Decreases

* `air offensive`
* `smash holder`
* `critical hitter`
* `shield master`
* `hammer collector`
* `special flagger`
* `homerun batter`
* `club swinger`
* `charger`

#### Personality Scores

* `Lightning Fast` >= 225
* `Quick` >= 170
* `Light` >= 100

### ofn (Enthusiastic, Aggressive, Offensive)

#### Description

The amiibo must be aggressive and in the face of its opponents. Attacking off stage is encouraged. Items should not be used. If they are picked up, then they should be thrown at an opponent immediately.

#### Must have

`near` >= 0.25

#### Increases

* `near`
* `offensive`
* `attack out cliff`
* `dash`
* `air offensive`
* `dash attacker`
* `meteor master`
* `item throw to target`

#### Decreases

* `feint shooter`
* `item collector`
* `dragoon collector`
* `smashball collector`
* `hammer collector`
* `special flagger`

#### Personality Scores

* `Offensive` >= 235
* `Aggressive` >= 180
* `Enthusiastic` >= 100

### rsk (Reckless, Thrill Seeker, Daredevil)

#### Description

The amiibo must play aggresively off stage, especially dunking. The amiibo should be in the air a lot, preferrable off stage. Melee counter attacks and parries are encouraged but projectile counters are not. Going for risky items is encouraged.

#### Must have

`attack out cliff` >= 0.26

#### Increases

* `near`
* `offensive`
* `attack out cliff`
* `air offensive`
* `cliffer`
* `feint master`
* `feint counter`
* `dash attacker`
* `critical hitter`
* `meteor masher`
* `just shield master`
* `hammer collector`
* `special flagger`
* `carrier broker`

#### Decreases

* `grounded`
* `feint shooter`

#### Personality Scores

* `Daredevil` >= 220
* `Thrill Seeker` >= 160
* `Reckless` >= 80

### gen (Versatile, Tricky, Technician)

#### Description

This personality must parry. It will show off its skills including countering, parrying, dunking, and using most items.

#### Must have

`just shield master` >= 0.26

#### Increases

* `attack out cliff`
* `feint master`
* `feint counter`
* `feint shooter`
* `catcher`
* `attack cancel`
* `meteor masher`
* `shield master`
* `just shield master`
* `shield catch master`
* `item collector`
* `item throw to target`
* `dragoon collector`
* `smashball collector`
* `item swinger`
* `homerun batter`
* `club swinger`
* `dash swinger`
* `item shooter`

#### Decreases

* `air offensive`
* `carrier broker`

#### Personality Scores

* `Technician` >= 150
* `Tricky` >= 120
* `Versatile` >= 80

### ent (Show-Off, Flashy, Entertainer)

#### Description

This personality must taunt a lot. Dunking, charging regular/smash attacks, and parrying are encouraged. Items should be used as intended and not just thrown.

#### Must have

`appeal` >= 0.25

#### Increases

* `attack out cliff`
* `smash holder`
* `critical hitter`
* `meteor masher`
* `just shield master`
* `dragoon collector`
* `smashball collector`
* `hammer collector`
* `special flagger`
* `homerun batter`
* `club swinger`
* `death swinger`
* `charger`
* `appeal`

#### Decreases

* `100 keeper`
* `item throw to target`

#### Personality Scores

* `Entertainer` >= 230
* `Flashy` >= 170
* `Show-Off` >= 90

### cau (Cool, Logical, Sly)

#### Description

This personlity must not parry. The amiibo should stay on the ground and use its shield a lot. It should stay far away from its opponents and counter projectiles if necessary. This is also a hard personality to get since many attributes take away from it and not many add to it.

#### Must NOT have

* `shield master` >= 0.26

#### Increases

* `grounded`
* `feint master`
* `feint shooter`

#### Decreases

* `near`
* `offensive`
* `attack out cliff`
* `dash`
* `air offensive`
* `cliffer`
* `feint counter`
* `smash holder`
* `dash attacker`
* `critical hitter`
* `meteor master`
* `shield master`
* `just shield master`
* `hammer collector`
* `special flagger`
* `charger`

#### Personality Scores

* `Sly` >= 215
* `Logical` >= 160
* `Cool` >= 80

### dyn (Laid Back, Wild, Lively)

#### Description

This personality is quite different from the others. It has no specific attribute requirement. It's similar to `rsk` except it doesn't counter or parry. It should attack a lot, especially off stage. Charging attacks is good. Holding items is strongly encouraged. What's particularly unusual is it's discouraged from throwing, swinging, or shooting items. This personality will simply hold items and never use them.

#### Must have

(Nothing)

#### Increases

* `offensive`
* `attack out cliff`
* `air offensive`
* `smash holder`
* `dash attacker`
* `critical hitter`
* `meteor master`
* `shield master`
* `dragoon collector`
* `smashball collector`
* `hammer collector`
* `special flagger`
* `homerun batter`
* `club swinger`
* `death swinger`
* `carrier broker`
* `charger`

#### Decreases

* `catcher`
* `attack cancel`
* `just shield master`
* `item throw to target`
* `item swinger`
* `item shooter`

#### Personality Scores

* `Lively` >= 210
* `Wild` >= 160
* `Laid Back` >= 80
