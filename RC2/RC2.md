---
typora-root-url: pic2
---

<div style="width:60%;height:150px;text-align:center;border-top:none solid red;border-left:none;border-bottom:none;margin-left:0%;margin-right:20%;display:inline-block">
    <div style="border:4px solid #999999;border-radius:8px;width:95%;height:100%;background-color: rgb(227, 227, 227);">
        <div style="width:100%;height:30%;text-align:center;line-height:60px;font-size:20px;font-family:'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;">VE270 - Fall 2021</div>
        <div style="width:100%;height:18%;text-align:center;line-height:50px;font-size:26px;font-family:'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;"><b>RC2 Logic Gates</div>
        <div style="width:100%;height:57%;text-align:center;font-size:16px;line-height:32px;font-family: 'Courier New', Courier, monospace;font-weight:300;"><br><b>TA: Wu You<br></b></div>


[TOC]

# Truth Table

> Truth table creates the relationship between the inputs and outputs. Once truth table is known, behavior of circuit is known, too.

<img src="/truth_table.jpg" style="zoom:25%;" />

# Transistors and Logic Gate

> In VE270, we only consider CMOS transistors, which can be categorized into nMOS and pMOS, whose behaviors are shown below.

- CMOS: Complementary Metal-Oxide-Semiconductor

<img src="/cMOS.jpg" style="zoom:30%;" />

> Based on transistor behaviors, different transistor circuits are built, which can be wrapped in corresponding **logic gates**. A logic gate is merely a simpler representation for a transistor circuit.

## AND Gate

> Behavior: accepts multiple inputs and outputs 1 iff all inputs are 1 .

<img src="/and_logic.png" style="zoom:90%;" />

<img src="/and_gate.png" style="zoom:100%;" />

## OR Gate

> Behavior: accepts multiple inputs and outputs 0 iff all inputs are 0.

<img src="/or_logic.png" style="zoom:90%;" />

<img src="/or_gate.png" style="zoom:90%;" />



## NOT Gate

> Behavior: accepts a single input and outputs the opposite value

<img src="/not_logic.png" style="zoom:90%;" />

<img src="/not_gate.png" style="zoom:100%;" />

## Other Logic Gates

<img src="/other_gates.png" style="zoom:100%;" align='left' />

- ‘X’ stands for ‘exclusive’
- XOR gate can be used for overflow detection method 2 (see RC1)
- **Only** personal experience: XOR gate is the most commonly encountered one out of above four gates.

# Logic Circuit

![](/logic_circuit.png)

## Main Components

- Logic Gates (Priority: **NOT > AND > OR**)
- Signals

## Tips on Circuit Drawing

![image-20210526133405005](/image-20210526133405005.png)

## Alternatives for Logic Circuit

> You should be familiar with how to convert among different forms of logic circuit. (For detailed procedure, please refer to Lecture Slides or come to my OH.) Once one of them is know, others  should also be known.

### Logic Equation and Truth Table

> In the picture below, you shall be capable of conduct conversions marked in black. The one marked in red will be discussed later. 

<img src="/conversion.jpg" style="zoom:100%;" />

### Timing Diagram

> Shows the response to changes on a signal in voltage levels with time.

<img src="/image-20210526133425005.png" style="zoom:30%;" />

<img src="/image-20210526133450032.png" style="zoom:50%;" />



<img src="/image-20210526133604589.png" style="zoom:70%;" />

- 1: high, 0: low
- dotted lines required when needed
