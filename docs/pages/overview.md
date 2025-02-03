---
layout: default
title: "ARCZIP Overview"
nav_order: 1
permalink: /
---

# ARCZIP

ARCZIP is an in-development common data format for storing the game state of [Leder Games' Arcs](https://ledergames.com/products/arcs).

{: .fs-1 }
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