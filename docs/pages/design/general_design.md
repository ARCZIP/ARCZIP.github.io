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

  <table class="custom-table">
    <thead>
      <tr>
        <th>Number</th>
        <th>Base70 Encoded Character</th>
      </tr>
    </thead>
    <tbody>
    <tr><td>0</td><td>A</td></tr>
    <tr><td>1</td><td>B</td></tr>
    <tr><td>2</td><td>C</td></tr>
    <tr><td>3</td><td>D</td></tr>
    <tr><td>4</td><td>E</td></tr>
    <tr><td>5</td><td>F</td></tr>
    <tr><td>6</td><td>G</td></tr>
    <tr><td>7</td><td>H</td></tr>
    <tr><td>8</td><td>I</td></tr>
    <tr><td>9</td><td>J</td></tr>
    <tr><td>10</td><td>K</td></tr>
    <tr><td>11</td><td>L</td></tr>
    <tr><td>12</td><td>M</td></tr>
    <tr><td>13</td><td>N</td></tr>
    <tr><td>14</td><td>O</td></tr>
    <tr><td>15</td><td>P</td></tr>
    <tr><td>16</td><td>Q</td></tr>
    <tr><td>17</td><td>R</td></tr>
    <tr><td>18</td><td>S</td></tr>
    <tr><td>19</td><td>T</td></tr>
    <tr><td>20</td><td>U</td></tr>
    <tr><td>21</td><td>V</td></tr>
    <tr><td>22</td><td>W</td></tr>
    <tr><td>23</td><td>X</td></tr>
    <tr><td>24</td><td>Y</td></tr>
    <tr><td>25</td><td>Z</td></tr>
    <tr><td>26</td><td>a</td></tr>
    <tr><td>27</td><td>b</td></tr>
    <tr><td>28</td><td>c</td></tr>
    <tr><td>29</td><td>d</td></tr>
    <tr><td>30</td><td>e</td></tr>
    <tr><td>31</td><td>f</td></tr>
    <tr><td>32</td><td>g</td></tr>
    <tr><td>33</td><td>h</td></tr>
    <tr><td>34</td><td>i</td></tr>
    <tr><td>35</td><td>j</td></tr>
    <tr><td>36</td><td>k</td></tr>
    <tr><td>37</td><td>l</td></tr>
    <tr><td>38</td><td>m</td></tr>
    <tr><td>39</td><td>n</td></tr>
    <tr><td>40</td><td>o</td></tr>
    <tr><td>41</td><td>p</td></tr>
    <tr><td>42</td><td>q</td></tr>
    <tr><td>43</td><td>r</td></tr>
    <tr><td>44</td><td>s</td></tr>
    <tr><td>45</td><td>t</td></tr>
    <tr><td>46</td><td>u</td></tr>
    <tr><td>47</td><td>v</td></tr>
    <tr><td>48</td><td>w</td></tr>
    <tr><td>49</td><td>x</td></tr>
    <tr><td>50</td><td>y</td></tr>
    <tr><td>51</td><td>z</td></tr>
    <tr><td>52</td><td>!</td></tr>
    <tr><td>53</td><td>#</td></tr>
    <tr><td>54</td><td>$</td></tr>
    <tr><td>55</td><td>%</td></tr>
    <tr><td>56</td><td>&</td></tr>
    <tr><td>57</td><td>(</td></tr>
    <tr><td>58</td><td>)</td></tr>
    <tr><td>59</td><td>+</td></tr>
    <tr><td>60</td><td>,</td></tr>
    <tr><td>61</td><td>.</td></tr>
    <tr><td>62</td><td>/</td></tr>
    <tr><td>63</td><td>:</td></tr>
    <tr><td>64</td><td>;</td></tr>
    <tr><td>65</td><td><</td></tr>
    <tr><td>66</td><td>></td></tr>
    <tr><td>67</td><td>[</td></tr>
    <tr><td>68</td><td>]</td></tr>
    <tr><td>69</td><td>{</td></tr>
    <tr><td>70</td><td>}</td></tr>

    </tbody>
  </table>

</details>

## Run-Length Encoding

ARCZIP uses [Run-Length Encoding](https://en.wikipedia.org/wiki/Run-length_encoding) across the entire string after all blocks have been assembled.

Length of runs should be specified after the character if a character is repeated more than once.

Ex. `CxAA-aAABBBRRRAAAA-yAAbAARRRBB-BAAAdddAAzAA` -> `CxA2-aA2B3R3A4-yA2bA2R3B2-BA3d3A2zA2`