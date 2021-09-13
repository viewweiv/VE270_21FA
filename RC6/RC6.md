---
typora-root-url: https://github.com/viewweiv/VE270_21FA_RC/tree/main/RC6/pic6
---

<div style="width:60%;height:150px;text-align:center;border-top:none solid red;border-left:none;border-bottom:none;margin-left:0%;margin-right:20%;display:inline-block">
    <div style="border:4px solid #999999;border-radius:8px;width:95%;height:100%;background-color: rgb(227, 227, 227);">
        <div style="width:100%;height:30%;text-align:center;line-height:60px;font-size:20px;font-family:'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;">VE270 - Fall 2021</div>
        <div style="width:100%;height:18%;text-align:center;line-height:50px;font-size:26px;font-family:'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;"><b>RC6 Latch & Flip Flop</div>
        <div style="width:100%;height:57%;text-align:center;font-size:16px;line-height:32px;font-family: 'Courier New', Courier, monospace;font-weight:300;"><br><b>TA: Wu You<br></b></div>

[TOC]

# Combinational vs. Sequential Circuit

| Combinational Circuit                      | Sequential Circuit                                           |
| ------------------------------------------ | ------------------------------------------------------------ |
| Output never comes back.                   | Output is fed back to the same circuit as input.             |
| Output depends only on **present** inputs. | Output depends on **present** inputs as well as I/O **history**. |
| Does not have memory.                      | Stores and remembers previous behaviors.                     |
| Truth table                                | Characteristic table                                         |
| MUX, HA, FA, ALU, ...                      | Latch, FF, ...                                               |

# Important Signals

## Control Signal

> After designing a circuit (function), in order to instantiate it with various control signals (input variables).

**Active High:** This control signal functions correctly when it is 1.

**Active Low:** This control signal functions correctly when it is 0.

<img src="/active_low.png" style="zoom:90%;" />

## Clock Signal

> In sequential circuits design, it is common that the circuit change simultaneously to the rhythm of a global signal, clock signal, so that the circuit behaviors can be synchronized.

<img src="/clock.png" style="zoom:90%;" />

**Synchronous**: clock signal dominating (Control signal influence the output **only** at the
active edge of the clock signal).

**Asynchronous**: control signal is dominating (Control signal influences the output even when clock signal is not at its active edge).

# Sequential Building Blocks

> In this chapter, two basic and essential sequential building blocks will be introduced: Latch and Flip Flop.

## Latch

### SR Latch

> S: set to high; R: reset to low.
>
> When S (set) = 1, $Q^+$= 1;
>
> When R (reset) = 1, $Q^+$= 0;
>
> When S = 0 and R = 0, $Q^+$ holds its previous state.
>
> When S = 1 and R = 1, $Q^+$may oscillate and eventually unpredictably settles to 0 or 1.

- Characteristic table and equation:

<img src="/SR_table.png" style="zoom:100%;" />

- Gate Level Implementation:

<img src="/SR_circuit.png" style="zoom:100%;" />

- Problem: output will oscillate when S=1 and R=1.

  Solution: Gated D Latch.

### Gated SR Latch

> Adds protection for unwanted situation.
>
> G: gate
>
> When G = 0, latch is locked, $Q^+$ holds its previous state.
>
> When G = 1, latch works as an SR latch.

- Characteristic table:

<img src="/Gated_SR_table.png" style="zoom:130%;" />

- Gate Level Implementation:

  <img src="/Gated_SR_circuit.png" style="zoom:100%;" />

- Problem: redundant control signals

### Gated D Latch

> Avoid unstable state by making S and R mutually exclusive to each other.
>
> D: S (set) in SR latch;
>
> D’: R (reset) in SR latch.
>
> When G = 1, $Q^+$ copies D;
>
> Otherwise, $Q^+$ follows previous value.

- Characteristic table:

<img src="/gated_D_table.png" style="zoom:100%;" />

- Gate Level Implementation:

  <img src="/gated_D_circuit.png" style="zoom:100%;" />

  - Recall that gated D latch resembles gated SR latch in its implementation. The only difference is that it feeds a control signal D to S port in gated SR latch, and feeds D’ to the R port.

- Timing Diagram Example:

  <img src="/gated_D_timing_diagram.png" style="zoom:80%;" />

  - Note that Q changes when G is not changing: asynchronous

## Flip Flop

### D Flip Flop (DFF)

<img src="/DFF.png" style="zoom:80%;" />

> *One of the most important building blocks in this course. You will see it again and again in the upcoming courses.
>
> At the active edge of clock signal, $Q^+$ copies the value of D. (Assume D would never change at the active edge of clock signal).
>
> Otherwise, $Q^+$ holds its previous state.

- Characteristic table and equation (of rising edge triggered DFF):

<img src="/DFF_table.png" style="zoom:100%;" />

- Timing Diagram Example:

  <img src="/DFF_diagram.png" style="zoom:80%;" />

### JK Flip Flop

<img src="/JK_FF.png" style="zoom:100%;" />

> Improve SR Latch by taking into account the situation of S = 1 and R = 1.
>
> J: jump (S in SR Latch) ; K: kill (R in SR Latch).
>
> When J (jump) = 1 and K (kill) = 0, $Q^+$= 1;
>
> When J (jump) = 0 and K (kill) = 1, $Q^+$= 0;
>
> When J = 0 and K = 0, $Q^+$ holds its previous state;
>
> When J = 1 and K = 1, $Q^+$ toggles (changes its output to the logical complement of its current value).

- Characteristic table:

  <img src="/JK_FF_table.png" style="zoom:100%;" />

### T Flip Flop

> T: toggle.
>
> When T = 0, $Q^+$ holds its previous state;
>
> When T = 1, $Q^+$ toggles.

- Characteristic table (of rising edge triggered TFF):

  <img src="/TFF_table.png" style="zoom:100%;" />

- Implementation (Review of RC5: Design Process of Building Blocks)

  <img src="/TFF_impl.png" style="zoom:100%;" />

### Register

<img src="/reg.png" style="zoom:100%;" />

> So far, we’ve only talked about building blocks operating on **1 bit** data. Register is used to store **multi bit** data. It is implemented by connecting multiple flip-flops that share a clock signal.

<img src="/reg_circuit.png" style="zoom:100%;" />

## Latch vs. Flip Flop

| Latch                                      | Flip Flop                                        |
| ------------------------------------------ | ------------------------------------------------ |
| level-sensitive: duty cycle length matters | edge-sensitive: duty cycle length doesn’t matter |
| asynchronous to clock signal               | synchronous to clock signal                      |

