---
typora-root-url: pic4
---

<div style="width:60%;height:150px;text-align:center;border-top:none solid red;border-left:none;border-bottom:none;margin-left:0%;margin-right:20%;display:inline-block">
    <div style="border:4px solid #999999;border-radius:8px;width:95%;height:100%;background-color: rgb(227, 227, 227);">
        <div style="width:100%;height:30%;text-align:center;line-height:60px;font-size:20px;font-family:'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;">VE270 - Fall 2021</div>
        <div style="width:100%;height:18%;text-align:center;line-height:50px;font-size:26px;font-family:'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;"><b>RC4 Logic Optimization</div>
        <div style="width:100%;height:57%;text-align:center;font-size:16px;line-height:32px;font-family: 'Courier New', Courier, monospace;font-weight:300;"><br><b>TA: Wu You<br></b></div>

[TOC]

# Criteria

<img src="full_optimization.png" style="zoom:100%;" />

- Decrease delay (number gate levels)

  Assume every gate level has 1 delay, if not specified.

  **Critical Path**: longest delay path(s) from an input to output.

- Reduce size (number of transistors)

  Assume every gate input requires 2 transistors and ignore inverters, if not specified.
  
- Lower Power usage (not important in VE270).

> Above represents an optimization satisfying both criteria. However, most optimization we meet satisfy one criteria but worsen the other one, in which situation we need a **trade off**.

<img src="/tradeoff_optimization.png" style="zoom:100%;" />

# Algebraic Simplification

> Please refer to RC3.

# K map

> K map is used to achieve simplified **two-level** circuit.

## Procedure

1. Write down truth table / sum-of-minterm of the logic circuit.
2. Create a table with variable-labelled entries where **adjacent entries differ by only one variable**.
3. Feed the outputs of truth table / sum-of-minterm to corresponding entries in K-map. (Each entry corresponds to a minterm.)
   * Note that the **order** of truth table and K map may differ. Be careful and avoid potential mistakes.
4. Combine **adjacent** entries of into several groups. (As large & few groups as possible.)
   * Note that each group contains $2^n$ entries.
   * Each group must be a rectangle.
   * Corners and edges may wrap around, as long as adjacent entries differ only by one variable.
   * Don't Cares can be treated as either 1 or 0. It’s your call. Utilize don’t cares to simplify your equation.
5. Write out the optimized logic equation by summing all groups.

<img src="/Kmap.png" style="zoom:100%;" />

**Small tips on how to convert logic equation to sum-of-minterm**

1. AND with sum of the missing literal, one at a time until all the missing literals are considered.
2. Remove the duplicated minterm.

e.g: `F = A’C + A’BD + AB’C + BCD`

## Key Concept

<font color=#FF0000>  TODO: Implicant, PI, Essential PI </font>
