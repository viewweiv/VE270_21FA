---
typora-root-url: pic3
---

<div style="width:60%;height:150px;text-align:center;border-top:none solid red;border-left:none;border-bottom:none;margin-left:0%;margin-right:20%;display:inline-block">
    <div style="border:4px solid #999999;border-radius:8px;width:95%;height:100%;background-color: rgb(227, 227, 227);">
        <div style="width:100%;height:30%;text-align:center;line-height:60px;font-size:20px;font-family:'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;">VE270 - Fall 2021</div>
        <div style="width:100%;height:18%;text-align:center;line-height:50px;font-size:26px;font-family:'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;"><b>RC3 Boolean Algebra</div>
        <div style="width:100%;height:57%;text-align:center;font-size:16px;line-height:32px;font-family: 'Courier New', Courier, monospace;font-weight:300;"><br><b>TA: Wu You<br></b></div>

[toc]

# Boolean Algebra

> Boolean Algebra a method of logic circuit optimization. Other methods will be discussed later.  

## Concept

### Basic operators:

- Parenthesis `(` `)`
- NOT `’`
- AND `•`
- OR `+`

### Terminology

Given the equation: `F = a'bc + abc' + ab + c`

- Variable: `a` `b` `c`

- Literal: `a'` `b` `c` `a` `b` `c'` `a` `b` `c`

- Product term (AND of literals): `a'bc` `abc'` `ab` `c`

- Sum term (OR of literals): none (a case would be `a+c'`, but not existed in F)

- Sum-of-products (SOP): `F` is in SOP form

  Given another equation: `G = (a+b)c + d`, `G` is not in SOP form

## Theorem

### Single Variable

<img src="/theorem_single_variable.png" style="zoom:67%;" />

### Multiple Variables

<img src="/theorem_multi_variables.png" style="zoom:67%;" />

### De Morgan's Law

- (a) `(x + y)' = x'y'`
- (b) `(xy)' = x' + y'`
- When *opening* the bracket, assign outside NOT operator to both items and change the relationship between them from AND to OR / from OR to AND
- three variables (can be verified using truth table): `(x + y + z)' = x'y'z'` `(xyz)' = x' + y' + z'`

### XOR Properties

<img src="/XOR_property.png" style="zoom:67%;" />

## Minterm & Maxterm

<img src="/min_max_term.png" style="zoom:50%;" />

- **Minterm**: product of literals
- **Maxterm**: sum of literals
- The complement of Minterm is the corresponding Maxterm, vice versa.
- minterm $m_i$ or maxterm $M_i$ is equivalent to the corresponding binary combination.
- They are frequently used to find the logic equation for a known truth table. (Now you should be able to perform conversion from truth table to logic equation mentioned in RC2.)

Find sum-of-minterms and product-of-maxterms from a given truth table:

<img src="/minterm_truth_table.png" style="zoom:67%;" />

## Simplification

### Don’t Care

<img src="/dont_care.png" style="zoom:97%;" />

> Outputs designated as “X” are called ‘don’t care’, which can be either 0 or 1, depending on your need. This concept will be useful for logic optimization later.