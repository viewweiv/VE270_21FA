---
typora-root-url: pic5
---

<div style="width:60%;height:150px;text-align:center;border-top:none solid red;border-left:none;border-bottom:none;margin-left:0%;margin-right:20%;display:inline-block">
    <div style="border:4px solid #999999;border-radius:8px;width:95%;height:100%;background-color: rgb(227, 227, 227);">
        <div style="width:100%;height:30%;text-align:center;line-height:60px;font-size:20px;font-family:'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;">VE270 - Fall 2021</div>
        <div style="width:100%;height:18%;text-align:center;line-height:50px;font-size:26px;font-family:'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;"><b>RC5 Combinational Circuit</div>
        <div style="width:100%;height:57%;text-align:center;font-size:16px;line-height:32px;font-family: 'Courier New', Courier, monospace;font-weight:300;"><br><b>TA: Wu You<br></b></div>

[TOC]

# General Design Process

> So far, we’ve used CMOS transistors to build logic gate (AND gate, Or gate, NOT gate ...). Now, we are able to increase the level of abstraction again and use logic gates to build combinational building block (MUX, HA, FA...).
>
> Design process of combinational building blocks and circuits follows the steps below.

1. Capture the function by creating a truth table or equation(s).
2. If using a truth table to capture the function, create an equation for each output by
   `OR` ing all the minterms for that output. Then, simplify the equation(s).
3. Implement the equation(s) as a logic circuit.

> Examples of above steps will be given in the following sections.

# Combinational Building Blocks

## Multiplexor (MUX)

> Select one of its N **data inputs** to its one **output**, based on its **select input(s)**.

**Design Process**

1. Truth table

   <img src="/mux_table.png" style="zoom:100%; float: left;" />

2. Equations

   <img src="/mux_equation.png" style="zoom:80%; float: left;" />

3. Circuit

   <img src="/mux_circuit.png" style="zoom:80%; float: left;" />

4. Final Building Block

   <img src="/mux_block.png" style="zoom:100%; float: left;" />

- $N$ data inputs $\rightarrow$ $log_2(N)$ select input(s)
- Remember to write down block/port labels.
  - $2 \space bit \space 4\cross1 \space mux$ (block label)
  - $i_0, i_1, i_2, i_3...$ (data input port labels)
  - $s_0, s_1...$ (select input port labels)
  - $d$ (output port label)
- Remember to state the correspondence between select inputs and
  date inputs if needed. If not specified, it is assumed that its correspondence follows the default one of 0 (00), 1 (01), 2 (10)... from top to bottom.
- Data inputs can be buses (more than one bit). Specify the width of input and output data ports, which should all be the same.

<img src="/bus_mux.png" style="zoom:100%;" />

## Adders

> Addition is a basic and important operation in computer systems. Recall that when it comes to 2’s complement, subtraction shares the same logic with addition. Thus, as long as adders are designed, subtraction operation can be performed with exactly the same building block.

### Half Adder (HA)

> Addition of **two 1 bit**  numbers A, B.  Output **1 bit Sum** and **1 bit Carry**.

**Design Process**

1. Truth table

   <img src="/HA_table.png" style="zoom:80%; float: left;" />

2. Equations

   <img src="/HA_equation.png" style="zoom:80%; float: left;" />

   * Note that Sum can also be expressed as $Sum = A \newcommand*\xor{\oplus} B$.

3. Circuit

   <img src="/HA_circuit.png" style="zoom:80%; float: left;" />

4. Final Building Block

   <img src="/HA_block.png" style="zoom:100%; float: left;" />

- Remember to write down output port labels (sum and carry).

### Full Adder (FA)

> Addition of **three 1 bit** numbers A, B, C. Output **1 bit Sum** and **1 bit Carry**.

**Design Process**

Its design process is nothing different from MUX and HA. Reach me if you have any difficulties finishing it yourself.

- Note that XOR gate is useful in the design of FA.
- HA can also be used directly to design FA.

### Carry-Ripple Adder

> Addition of **two n bit** binary numbers A, B and **one 1 bit** cin, which is added to the LSBs of A and B.  Output **n bit Sum** and **1 bit Cout**.

Recall how human do addition:

<img src="/human_addition.png" style="zoom:100%;" />

Come up with the idea of connecting FAs to build an n-bit carry-ripple adder.

<img src="/adder.png" style="zoom:80%;" />

- Overflow can be an optional output.

### Subtractor

> Since `A-B=A+(-B)`, where `-B` is the additive inverse of `B`. In 2’s complement number system, the additive inverse of a number can be obtained by inverting every bit of it and adding 1 to the result. Thus, if we invert B and set `cin` to 1, the n-bit adder can be used as an n-bit subtractor.

<img src="/subtractor.png" style="zoom:80%;" />

## Arithmetic-Logic Unit (ALU)

> A component that can select and perform one of various arithmetic (add, subtract, increment, ...) and logic (AND, OR, ...) operations based on **input control signals**.
>
> ALU is resembles MUX, except that ALU selects among operations while MUX selects among data.

<img src="/ALU.png" style="zoom:100%;" />

## Encoder

> Convert $2^{n}$-bit one high binary inputs to n-bit binary outputs.

<img src="/encoder.png" style="zoom:100%;" />

- Encoder is less popular than decoder.
- Normally, one and only one bit of input data is 1 and others are all 0. 
- All the inputs together can be understood as “a basis for a vector space” in linear algebra. (Ignore this item if it doesn’t make sense to you).

## Decoder

> Convert **n bit** binary inputs to **$2^{n}$ bit** one high binary outputs. 
>
> when its **1 bit** enable input, e, is high. Otherwise, outputs all 0.

<img src="/decoder.png" style="zoom:100%;" />

- Decoder with an extra control input, e, works as regular decoder when e=1 and outputs all 0 when e=0 regardless of what input it is fed with. (Decoder in Multisim has this feature. Remember to set e to 1 when working on your labs.)
- Decoder is also called Minterm Generator (review RC3 for the definition of minterm). With this property, we can design more human-readable circuits.

<img src="/minterm_gen.png" style="zoom:80%;" />

- Note that port 7 is unused, which is acceptable.

## Buffer

> A buffer is a one directional transmission logic device. Implemented with two inverters.

<img src="/buffer.png" style="zoom:80%;" />

- Amplify the driving capability of a signal in order to compensate for loss in transmission.

  <img src="/fan_out.png" style="zoom:80%;" />

- Insert delay to avoid timing issues.

### Tri-State Buffer

> A tri-state buffer can have three different output values, controlled by a **1 bit** enable signal, B.

<img src="/tri_state.png" style="zoom:80%;" />

- `Z`: high impedance with zero voltage.

- Tri-state buffers and a decoder together can build a MUX.

  <img src="/decoder_buffer_mux.png" style="zoom:80%;" />

- For more information about buffer, you can refer to https://www.electronics-tutorials.ws/logic/logic_9.html.
