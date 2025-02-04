---
layout: default
title: Base70
nav_order: 2
parent: General Design
---



# Base70

ARCZIP uses encoding similar to [Base64](https://en.wikipedia.org/wiki/Base64) in many blocks but removes digits and characters that apply Discord styling. 

In this documentation, `B70()` will be used to denote a conversion of a number using Base70.

Ex. `B70(Number of cards in the court deck)` with 26 cards would be `B70(26)='a'` 

## Base70 Encoding from Number to Character

Available as a CSV [here](../../assets/arczip_base70.csv) 

{% include base70_table.html %}