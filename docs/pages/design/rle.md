---
layout: default
title: Run-Length Encoding
nav_order: 3
parent: General Design
---

# Run-Length Encoding

ARCZIP uses [Run-Length Encoding](https://en.wikipedia.org/wiki/Run-length_encoding) across the entire string after all blocks have been assembled.

Length of runs should be specified after the character if a character is repeated more than once.

Ex. `CxAA-aAABBBRRRAAAA-yAAbAARRRBB-BAAAdddAAzAA` -> `CxA2-aA2B3R3A4-yA2bA2R3B2-BA3d3A2zA2`