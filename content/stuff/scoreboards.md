# Simple Minecraft Scoreboards Guide

This document describes how I understand Minecraft scoreboard system.

Scoreboard can be used to keep track of scores. Scores can be calculated manually or set to built-in values such as health or deaths.

## Objectives

Objectives are used to initialize keeping track of a score.

### Creating a objective

To create a objective type:
```/scoreboards objectives add <objective name> <criteria>```
Objective name must not contain a space(s).

Criteria describes what this score is supposed to keep track of. There is a large ammount of built-in values most of which are self explanitory.

#### Criterias

- dummy - Use this if you want to only manipulate the objective manually. This criteria is never manipulated by the game itself
- food - Player's hunger bar from 0 to 20. Readonly
- health - From 0 to 20. Readonly
- armor - From 0 to 20. Readonly
- level - Player's level. Readonly
- xp - Player's xp in points. When player gets xp using `/xp add player n level`, this score *will not* be updated. Readonly
- level - Player's xp in levels. Works as expected. Readonly
- minecraft.* - self explanitory
- ...

### Manipulating a objective value

To manipulate a objective value use:
```/scoreboard players operation <player1> <objective1> <operaion> <player2> <objective2>```

This is used to apply a <operation> to two values. First is <player1>'s <objective1> value and second is <player2>'s <objective2> value.

#### Operations

- %= -
- *= -
- += - 
- -= - 
- /= - 
- <  -
- =  -
- >  -
- >< - 
