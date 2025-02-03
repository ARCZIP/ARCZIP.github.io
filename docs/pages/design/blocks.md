---
layout: default
title: Blocks
nav_order: 1
parent: General Design
---

# Blocks

ARCZIP data is broken up into multiple Data Blocks each with their own unique layout.

These blocks are separated from one another by a `-` character and all start with a unique character ID for that section (or multiple characters if there is ever a need for >70 blocks).

- All blocks are optional except for the GameInfo block (ID `A`) but tools that import ARCZIP may require some blocks
  - For example, a stream overlay may require blocks for map data

- Each block is responsible for representing and compressing some amount of game data.
- Some blocks are only present when a certain Card or Fate is present and represent the data tied to that Card/Fate.
- Blocks may be excluded from compression if not needed for a use case.
  - As stated above, many applications rely on all Blocks for functionality so this is only recommended if you know the client you are compressing for

Specification for individual blocks are documented in the Block Documentation section