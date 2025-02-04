---
layout: default
title: "A - Game Info" 
nav_order: 1
parent: Block Specification
---

# A - Game Info Block


{: .warning }
This block must always be included in ARCZIP

Concatenate all characters from the table below together into one string. Don't forget to terminate the block with a `-` character.

{% capture s_1 %}
`A` - The block ID for Game Info
{% endcapture %}

{% capture s_2 %}
Encode a single character representing the current ARCZIP version number.  
Currently this is `{% include versionnumber.md %}`.
{% endcapture %}

{% capture s_3 %}
- If this is a Blighted Reach game, Encode `B70(0)` => `A`.
- Otherwise (if it is Base Game):
    - Generate a number representing the Clusters in play by either
      - Create a binary number where the Nth digit going **right to left** is a 0 if the Nth Cluster is out of play, 1 if it's in play.
      - Encode `B70()` of this number
  
Example - If Clusters 4 and 6 are out of play, `010111 => 23 => B70(23) => 'X'`

Example 2 - If Clusters 2 and 5 are out of play, `101101 => 45  => B70(45 ) => 't'`
{% endcapture %}

{% capture s_4 %}
Encode the player order around the table and current initiative holder.

The mapping is available as a CSV [here](../../assets/arczip_player_order.csv)

Or reference the following table. (Initiative holder is first and bolded)

<details>
  <summary><b>Click to expand Player Order Encoding Table</b></summary>
    {% include player_order_table.html %}
</details>
{% endcapture %}

{% capture s_5 %}
Encode the player whose turn it is:
- Red: `A`
- White: `B`
- Teal: `C`
- Yellow: `D`

If it is no players turn, encode the player who will play next.
{% endcapture %}

{% capture s_6 %}
Encode the current chapter number with `B70` (Ex. Chapter 3 = `B70(3)`=>`D`)

If playing Blighted Reach Act III, add 5 to the value before encoding (Ex. Chapter 2 = `B70(7)`=>`F`)
{% endcapture %}

{% capture s_7 %}
Encode the initiative and ambition declared markers based on the table below

{:custom-table}
|Initiative Seized|Zero Marker on Lead Card|Character|
|:----------------|:-----------------------|:--------|
|False            |False                   |`A`      |
|False            |True                    |`B`      |
|True             |False                   |`C`      |
|True             |True                    |`D`      |

{% endcapture %}

{% capture s_8 %}
- If playing the campaign, encode the value of the event dice to a single character
  - Get the value of the event die based on the mapping below.
  - Add the value of the number die to this.
  - `B70()` the sum.
- If playing base game, you can skip.

{:custom-table}
|Event Die Symbol|Value|
|:---------------|:----|
|▲ (Crisis)      |0    |
|▲ (Edict)       |6    |
|☾ Crisis        |12   |
|☾ Edict         |18   |
|⬢ Crisis        |24   |
|⬢ Edict         |30   |

{% endcapture %}


<table class="custom-table">
  <thead>
    <tr><th>Index</th><th>Data</th></tr>
  </thead>
  <tbody>
    <tr><td>1</td><td>{{ s_1 | markdownify }}</td></tr>
    <tr><td>2</td><td>{{ s_2 | markdownify }}</td></tr>
    <tr><td>3</td><td>{{ s_3 | markdownify }}</td></tr>
    <tr><td>4</td><td>{{ s_4 | markdownify }}</td></tr>
    <tr><td>5</td><td>{{ s_5 | markdownify }}</td></tr>
    <tr><td>6</td><td>{{ s_6 | markdownify }}</td></tr>
    <tr><td>7</td><td>{{ s_7 | markdownify }}</td></tr>
    <tr><td>8</td><td>{{ s_8 | markdownify }}</td></tr>
  </tbody>
</table>