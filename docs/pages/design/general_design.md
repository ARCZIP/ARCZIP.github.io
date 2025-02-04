---
layout: default
title: "General Design"
nav_order: 2
permalink: /design
has_toc: false
---

# General Design
## Blocks

ARCZIP data is broken up into multiple Data Blocks each with their own unique layout.

These blocks are separated from one another by a `-` character and all start with a unique character ID for that section (or multiple characters if there is ever a need for >70 blocks).

- All blocks are optional except for the GameInfo block (ID `A`) but tools that import ARCZIP may require some blocks
  - For example, a stream overlay may require blocks for map data

- Each block is responsible for representing and compressing some amount of game data.
- Some blocks are only present when a certain Card or Fate is present and represent the data tied to that Card/Fate.
- Blocks may be excluded from compression if not needed for a use case.
  - As stated above, many applications rely on all Blocks for functionality so this is only recommended if you know the client you are compressing for

Specification for individual blocks are documented in the Block Documentation section

## Base70

ARCZIP uses encoding similar to [Base64](https://en.wikipedia.org/wiki/Base64) in many blocks but removes digits and characters that apply Discord styling. 

In this documentation, `B70()` will be used to denote a conversion of a number using Base70.

Ex. `B70(Number of cards in the court deck)` with 26 cards would be `B70(26)='a'` 

### Base70 Encoding from Number to Character

Available as a CSV [here](../../assets/arczip_base70.csv) 

<details>
  <summary>Click to expand Base70 Encoding Table</summary>
  {% include base70_table.html %}
</details>

## Run-Length Encoding

ARCZIP uses [Run-Length Encoding](https://en.wikipedia.org/wiki/Run-length_encoding) across the entire string after all blocks have been assembled.

Length of runs should be specified after the character if a character is repeated more than once.

Ex. `CxAA-aAABBBRRRAAAA-yAAbAARRRBB-BAAAdddAAzAA` -> `CxA2-aA2B3R3A4-yA2bA2R3B2-BA3d3A2zA2`