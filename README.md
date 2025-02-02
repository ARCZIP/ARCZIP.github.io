# ARCZIP

ARCZIP is common data format for storing the game state of [Leder Games' Arcs](https://ledergames.com/products/arcs).

ARCZIP is in active development.


Disclaimer: This project is not an official Leder Games product. Arcs and its related materials are the copyright of Leder Games.

## Why create a common data format?
Having a common data format allows many applications related to the game to easily communicate with each other. Some examples:

- Transferring an in-progress game freely between a Virtual Tabletop Platform (Tabletop Simulator, Tabletop Playground, hrf.im) and the physical board game (with state created/loaded via an app)
- Creating mid-game scenarios to be shared and replayed.
- Export from a Virtual Tabletop to a discord bot or stream overlay to provide game info to viewers or log moves to a replayable format
- Discord bot to play the game asynchroncously with a server side implementation of game logic

## Requirements

The ARCZIP data format must:
- Support capturing the game state such that it can be recovered the game could be played
- Handle both Base game and the Blighted Reach expansion
- Be text-based and sharable over Discord (< 2000 characters)
- Be compressable and decompressable in any programming language (no dependency on specific libraries)
- Have versioning encoded to enable expansion for new game content

---

# General Design

## "Blocks"

ARCZIP data is broken up into multiple data Blocks each with their own unique layout.

These blocks are separated from one another by a `-` character and all start with a unique character ID for that section (or multiple characters if there is ever a need for >70 blocks).

- All blocks are optional except for the GameInfo block (ID `A`) but tools that import ARCZIP may require some blocks
  - For example, a stream overlay may require blocks for map data

- Each block is responsible for representing and compressing some amount of game data.
- Some blocks are only present when a certain Card or Fate is present and represent the data tied to that Card/Fate.
- Blocks may be excluded from compression if not needed for a use case.
  - As stated above, many applications rely on all Blocks for functionality so this is only recommended if you know the client you are compressing for

Specification for individual blocks are documented in the [Block Documentation](#block_docs) section

## Base70

ARCZIP uses encoding similar to [Base64](https://en.wikipedia.org/wiki/Base64) in many blocks but removes digits and characters that apply Discord styling. 

In this documentation, `B70()` will be used to denote a conversion of a number using Base70.

Ex. `B70(Number of cards in the court deck)` with 26 cards would be `B70(26)='a'` 

<details>
  <summary>Base70 Encoding from Number to Character</summary>

|Number|Base70 Encoded Character|
|--|--|
|0|`A`|
|1|`B`|
|2|`C`|
|3|`D`|
|4|`E`|
|5|`F`|
|6|`G`|
|7|`H`|
|8|`I`|
|9|`J`|
|10|`K`|
|11|`L`|
|12|`M`|
|13|`N`|
|14|`O`|
|15|`P`|
|16|`Q`|
|17|`R`|
|18|`S`|
|19|`T`|
|20|`U`|
|21|`V`|
|22|`W`|
|23|`X`|
|24|`Y`|
|25|`Z`|
|26|`a`|
|27|`b`|
|28|`c`|
|29|`d`|
|30|`e`|
|31|`f`|
|32|`g`|
|33|`h`|
|34|`i`|
|35|`j`|
|36|`k`|
|37|`l`|
|38|`m`|
|39|`n`|
|40|`o`|
|41|`p`|
|42|`q`|
|43|`r`|
|44|`s`|
|45|`t`|
|46|`u`|
|47|`v`|
|48|`w`|
|49|`x`|
|50|`y`|
|51|`z`|
|52|`!`|
|53|`#`|
|54|`$`|
|55|`%`|
|56|`&`|
|57|`(`|
|58|`)`|
|59|`+`|
|60|`,`|
|61|`.`|
|62|`/`|
|63|`:`|
|64|`;`|
|65|`<`|
|66|`>`|
|67|`[`|
|68|`]`|
|69|`{`|
|70|`}`|
</details>

  


### Run-Length Encoding

ARCZIP uses [Run-Length Encoding](https://en.wikipedia.org/wiki/Run-length_encoding) across the entire string after all blocks have been assembled.

Length of runs should be specified after the character if a character is repeated more than once.

Ex. `CxAA-aAABBBRRRAAAA-yAAbAARRRBB-BAAAdddAAzAA` -> `CxA2-aA2B3R3A4-yA2bA2R3B2-BA3d3A2zA2`

![](./site%20ids.png)

# <a name="block_docs"></a>Block Documentation