<div style="width:60%;height:150px;text-align:center;border-top:none solid red;border-left:none;border-bottom:none;margin-left:0%;margin-right:20%;display:inline-block">
    <div style="border:4px solid #999999;border-radius:8px;width:95%;height:100%;background-color: rgb(227, 227, 227);">
        <div style="width:100%;height:30%;text-align:center;line-height:60px;font-size:20px;font-family:'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;">VE270 - Fall 2021</div>
        <div style="width:100%;height:18%;text-align:center;line-height:50px;font-size:26px;font-family:'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;"><b>RC7 Introduction to Verilog</div>
        <div style="width:100%;height:57%;text-align:center;font-size:16px;line-height:32px;font-family: 'Courier New', Courier, monospace;font-weight:300;"><br><b>TA: Wu You<br></b></div>

[TOC]

## Verilog HDL

### Overview

What is Verilog?

- basically a HDL (Hardware Description Language) that **describes digital circuits with C-like syntax**
- ==\*== [^3] it can define digital circuits at the RTL (Register-Transfer Level) of abstraction
- by using other tools (ASIC / FPGA toolchains), Verilog program can be translated to a gate-level netlist

### Module, I/O Port, Instantiation, Hierarchy [^1]

What are Verilog modules?

- Modules are the building blocks of Verilog designs. They are a means of abstraction and encapsulation for your design.

- A module consists of a port declaration and Verilog code to implement the desired functionality.

- Modules should be created in a Verilog file `(.v)`  where the filename matches the module name. For example:

- ```verilog
  module full_adder (
  	input x, y,
      input cin,
      output s,
      output cout
  );
      // do something
  endmodule
  ```

- which is corresponding to:

<img src="RC4 (week-8).assets/image-20210624020230407.png" alt="image-20210624020230407" style="zoom: 35%;" />

What are I/O Ports in Verilog modules?

- Ports allow communication between a module and its environment
- Each port has a name and a type (`input`, `output`, ...)
- Ports for a module are declared in the port declaration

Verilog Module Instantiation:

- After finishing defining your module, you can instantiate the module inside other modules.
- Instantiation syntax: `<module_name> <instance_name> (.port0(wire), .port1(wire), ...)`
- Note: `.port0()` can be omitted, but such a syntax feature is recommended

Verilog Module Hierarchy:

- Every Verilog design has a top-level module which sits at the highest level of the design hierarchy

- The top-level module defines the I/O for the entire digital system

- All the modules in your design reside inside the top-level module. For example:

- ```verilog
  module top_level_module(
  // I/O ports omitted
  );
      // intermediate wires omitted
      full_adder adder1(.x(in0), .y(in1), .cin(cin), .s(LED0), .cout(LED1));
      // other logics omitted
  endmodule
  ```

- which is corresponding to:

<img src="RC4 (week-8).assets/image-20210624021132067.png" alt="image-20210624021132067" style="zoom:40%;" />

### Structure Verilog vs. Behavioral Verilog

- structural descriptions: 
  - connections of components
  - nearly one-to-one correspondence with the schematic diagram
  - `and`, `or`, `xor`, `not`, `nand`, `nor`, `xnor` exist in Verilog as gate primitives
  - typically not used in design
- behavioral descriptions:
  - use high-level constructs to describe the circuit function
  - similar to conventional programming
- comment
  - structural Verilog is a pain to write, but you will never have to write it that way!

Example (**just for fun*)

```verilog
// Structural Verilog Full Adder
module _full_adder(input a, input b, input cin, output s, output cout);
    xor (s,a,b,cin);
    wire xor_a_b, cin_and, and_a_b;
    xor (xor_a_b, a, b);
    and (cin_and, cin, xor_a_b);
    and (and_a_b, a, b);
    or (cout, and_a_b, cin_and);
endmodule

// Behavior Verilog Full Adder
module full_adder(input a, input b, input cin, output s, output cout);
    assign s = x ^ y ^ cin;
    assign count = (a && b) || (cin && (a ^ b));
endmodule
```

### data type, `wire` vs. `reg` [^2]

definition:

- `wire` elements are simple wires (a.k.a. busses of arbitrary width) in Verilog design.
- `reg` elements can be used to store information ('state') like registers.

**legal** usage of `wire` element:

```verilog
wire	A, B, C, D, E;	// declaration of simple 1-bit-wide wires
wire 	[8:0] Wide; 	// declaration of a 9-bit-wide wire

assign A = B & C; 		// wire assignment

reg I;
always @(B or C) begin
	I = B | C;			// using wires on the right-hand-side of an always@ assignment
end

myModule uut(
    .In (D),
    .Out (E)
);						// using a wire as the input / output of a module
```

**legal** usage of `reg` element:

```verilog
wire	A, B;
reg 	I, J, K;		// declaration of simple 1-bit-wide reg elements
reg		[8:0] Wide;		// declaration of a 9-bit-wide reg

always @(A or B) begin
	I = A | B;			// using a reg as the left-hand-side of an always@ assignment
end

initial begin 			// using reg in an initial block
	J = 1'b1;
    #1
    J = 1'b0;
end

always @(posedge Clock) begin
    K <= I; 			// using a reg to create a positive-edge-triggered register
end    
```

Note: when connecting multi-bit signals from one to another, the bit-widths of them must match!

**major difference**

| metric                                              | `wire`                                                       | `reg`                                                        |
| :-------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| general usage                                       | Combinational logic                                          | Combinational logic and Sequential logic                     |
| **Outside** a module instance: I/O port restriction | `wire` elements are used to connect `input` and `output` ports of a module instantiation together with some other element in your design | (1) `reg` can be connected to the `input` port of a module instantiation<br>(2) `reg` **cannot** be connected to the `output` port of a module instantiation |
| **Inside** a module design: I/O port restriction    | `wire` element are used as `input`s and `output`s within an actual module declaration | (1) `reg` can be used as `output`s within an actual module declaration<br>(2) `reg` cannot be used as `input`s within an actual module declaration |
| in an `assign` statement:                           | `wire` is the only legal type on the left-hand side of an `assign` statement | `reg` **cannot** be used on the left-hand side of an `assign` statement |
| in an `always@` block:                              | `wire` **cannot** be used as the left-hand side of an `=` / `<=` operator in an `always@` block | `reg` is **the only legal type** on the left-hand side of an `always@` block `=` / `<=` operator |
| in an `initial` block:                              | `wire` cannot appear on the left-hand side of `=` operator in an `initial` block | `reg` is **the only legal type** on the left-hand side of `=` operator in an `initial` block |
| MISC                                                | (1) `wire` elements must be driven by something (*otherwise the value will display as Z*), and cannot store a value without being driven<br>(2) `wire` elements are a stateless way of connecting two pieces in Verilog | (1) `reg` can be used to create registers when used in conjunction with `always@(posedge Clock)` blocks |

when are `wire` and `reg` be interchangeable?

1. Both can appear on the right-hand side of `assign` statements 
2. Both can appear on the right-hand side of `=` / `<=` operators in `always@` block
3. Both can be connected to the `input` ports of module instantiations
4. Both can be used as output pin within an actual module declaration

### assignment, blocking vs. non-blocking

assignment rules

- when connecting multi-bit inputs or outputs in an assignment, the bit-widths need to match!
- wires can be assigned to logic equations, other wires, or operations performed on wires by `assign`
  - see more in the previous section
- `=` for blocking assignment (in combinational logic)
- `<=` for non-blocking assignment (in sequential logic)

### operators

| Operator Type     | Symbol      | Operation Performed         |
| ----------------- | ----------- | --------------------------- |
| Arithmetic        | `+`         | Add                         |
|                   | `-`         | Subtract                    |
|                   | ``*``       | Multiply                    |
|                   | `/`         | Divide                      |
|                   | `%`         | Modulus                     |
| Logical           | `!`         | Logical negation            |
|                   | `&&`        | Logical and                 |
|                   | `||`        | Logical or                  |
| Relational        | `>`         | Greater than                |
|                   | `<`         | Less than                   |
|                   | `>=`        | Greater than or equal       |
|                   | `<=`        | Less than or equal          |
| Equality          | `==`        | Equality                    |
|                   | `!=`        | Inequality                  |
| Bitwise operation | `~`         | Bitwise negation            |
|                   | `&`         | Bitwise AND                 |
|                   | `|`         | Bitwise OR                  |
|                   | `^`         | Bitwise XOR                 |
| Shift             | `<<`        | Shift left logical          |
|                   | `>>`        | Shift right logical         |
|                   | `<<<`       | Arithmetic left shift ==*== |
|                   | `>>>`       | Arithmetic left shift       |
| Concatenation     | `{}`        | Join bits                   |
| Replication       | `{{}}`      | Duplicate bits              |
| Indexing/Slicing  | `[MSB:LSB]` | Select bits                 |

Examples:

```verilog
wire [7:0] d; //8-bit
wire [31:0] e, f; //32-bit

assign f = {d, e[23:0]}; // Concatenation + Slicing
assign f = { 32{d[5]} }; // Replication + Indexing
```

Note: one thing omitted is the set of reduction operators, see more in [click-me!](https://class.ece.uw.edu/cadta/verilog/reduction.html#:~:text=Verilog%20has%20six%20reduction%20operators,ANDed%20together%20to%20produce%20Y1.)

### Ternary operator

Ternary operator, a.k.a. Conditional Operator:

`<condition> ? <true_operation> : <false_operation>`

![image-20210618025453001](RC4 (week-8).assets/image-20210618025453001.png)

- can be used either in wire assignment or reg assignment

- **extremely useful** for conditional assignment between wire variables

- can be cascaded:

  - ```verilog
    // An example for cascading ternary operators
    assign q_next =
        a==3'b001 ? 4'b0000 :
        a==3'b010 || a==3'b10 ? 4'b1111 :
        q_val;
    ```

  - evaluation order: from left to right (in this example, from up to down :-) )

  - cascading conditions are not necessary to be mutual exclusion with each other

  - can be imagined as the cascade of three 2-to-1 MUX! [^5]

### Literal (Constant), Tricky Grammar Feature

Verilog literals

- Syntax: `[bit width]'[radix][literal]`
- radix can be `d` (decimal), `h` (hexadecimal), `o` (octal), `b` (binary)
- it is critical to always match bit widths for operators and module connections, do not ignore these warnings from the tools
- extra warning: `{16{1'b1}}` is **totally different** from `16'b1`!!!!!

Verilog literals example:

```verilog
2'd1 // 2-bit literal (decimal 1)
16'hAD14 // 16-bit literal (hexadecimal 0xAD14)
8'b01011010 // 8-bit literal (binary 0b01011010)
```

### `always@` [^4]

**Combinational logic blocks using `always@(*)`**

- the value inside the parentheses is called **the sensitivity list**
- using `*` will tell the compiler to compute the sensitivity list automatically

```verilog
reg x, y;
always @(*) begin
    x = ~y;
end
```

**if-else statements**

```verilog
input x, y, w;
reg [1:0] z;
always @(*) begin
    if (x) begin
        z[0] = w;
        z[1] = y;
    end else begin
        z[0] = y;
        z[1] = x;
    end
end
```

**case statements**

```verilog
input [1:0] x;
reg [1:0] y;
always @(*) begin
    case(x)
        0: y = 2'd0;
        1: y = 2'd3;
        2: y = 2'd2;
        default: y = 2'd2;
    endcase
end
```

**synchronous logic block**

Synchronous logic blocks are generated using special identiers in the sensitivity list. Here we only want to update on the positive edge of the clock, so we use posedge. This will generate a synchronous circuit that increments x every clock cycle.

```verilog
input clk;
reg [1:0] x;
always @(posedge clk) begin
    x <= x + 1;
end
```

## Verilog Testbench

- `initial` block: primarily used for test benches; contain sequences of code to be executed at the beginning of the simulation
- `#`: delay operator, used to step through time events
- ``timescale 1ns/1ps` 
  - the first value defines the time unit (1ns), i.e., a delay of `#1` will result in a 1-ns step
  - the second value defines the simulation resolution (1ps), i.e., a minimum delay of `#0.001` will be supported

```verilog
`define clock_period 5
reg [31:0] a, b;

reg clk = 1'b0; // give it an initial value
always #(`clock_period/2) clk = ~clk; // toggle clock every half cycle

module dut mymodule(.clk(clk), .in0(a), .in1(b), .out(c));

initial begin
	clk = 1'b0;
	a = 32'h01234567;
	b = 32'h00000000;
	#1 a = 32'h10101010; // wait 1 time unit
	#1 b = 32'h01010101;
	#1 a = 32'h00000000;
	b = 32'h11111111; // a and b change together
	@(posedge clk); // jump to next rising edge
	@(posedge clk); // jump to next rising edge
	a = 32'hFEDCBA98;
	#10;
	// observe some output here.
    
end
```

more see blackboard 0.0

## Unintentional Hazard

### Unintentional Latch Synthesis

see the example below:

```verilog
input [1:0] x;
reg [1:0] y;
always @(*) begin
    if (x == 2'b10)
        y = 2'd3;
    else if (x == 2'b11) begin
        y = 2'd2;
    end
end
```

- every signal should have a default value.
- assigning a value to a reg only under given conditions will result in latch synthesis.

revised:

```verilog
input [1:0] x;
reg [1:0] y;
always @(*) begin
    y = 2'b00;
    if (x == 2'b10)
        y = 2'd3;
    else if (x == 2'b11) begin
        y = 2'd2;
    end
end
```

## ==\*== Verilog Implementation Tricks

### Non-Synthesizable Control Statements

- `for` and `while` loops **cannot** be mapped to hardware. They are valid Verilog feature which can be simulated, but cannot always be mapped to hardware.
  - ==*== `generate` statement is an appropriate alternative. (explained later)

### Non-Synthesizable Initial Block

Initial blocks are not synthesizable.

### Implementation Tricks

**ambiguous clock in event control** [^8]

> [Synth 8-91] ambiguous clock in event control ...
> [Synth 8-285] failed synthesizing module 'counter'

During the synthesis process, the synthesis tool maps to devices available on the FPGA. For example, when the sensitivity list is always @(posedge clk), we map to available flip-flops.

There are variants of the flip-flop that can do asynchronous preset/clear, which we can map to using always @(posedge clk or <posedge/negedge> rst). 

However, if based on your code, it is not clear which signal the tool needs to connect to which input pin on the flip-flop, the tool will report this error and let you know that the clock is ambiguous. 

```verilog
// error code
module counter( input clk, enable, output reg [3:0] q);
    always @ ( negedge clk, negedge enable) begin
        q <= q + 1;
    end
endmodule

// another error code
module counter( input clk, output reg [3:0] q);
    always @ ( negedge clk, posedge clk) begin
    	q <= q + 1;
    end
endmodule
```

```verilog
// fixed
module counter( input clk, enable, clear, output reg [3:0] q);
    always @ ( negedge clk, negedge enable) begin
        if (!enable) begin
            q <= q;
        end else begin
            q <= q + 1;
        end
    end
endmodule
```

extra notes: try to simplify if-else branches or fill incomplete else loops

**Multiple Driver Nets**

> [DRC 23-20] Rule violation (MDRV-1) Multiple Driver Nets - Net y_XXX has multiple drivers: y_XXX/Q, y_XXX/Q.

This message is informing you that there is a signal (in the error example above, signal y is the source of the error) that is being assigned / updated / driven in two different always blocks.

```verilog
// error code
always @ (posedge CLK)
    y = y + 1;


always @ (posedge CLK2)
    y = y + 3;
```

In the example code above, there are two conflicting instructions presented. The first instruction would be to increment y by 1, while the second instruction would be to increment y by 3. The compiler would not be able to know which instruction should be executed. Thus, the Multiple Driver Nets error would occur.

Solution:

1. Examine the error to first identify the signal (for example signal y) with multiple conflicting drivers.
   - In cases of large and complex designs, it may be easier to use the elaborated netlist (RTL Analysis → Open Elaborated Design) to identify the signal.
2. Within the always blocks in your code, search for lines of code where y is the target of assignment (y <= ... or y = ...) to identify the multiple drivers.
3. Modify the code and remove this issue.

## ==\*== Verilog Advanced Feature

### localparam Declaration, Parameterization

Private module parameters are defined using the localparam directive. These are useful for constants that are needed only by a single module.

```verilog
localparam coeff = 5;
reg [31:0] x;
reg [31:0] y;
always @(*) begin
    x = coeff*y;
end
```

Verilog modules may include parameters in the module denition. This is useful to change bus sizes or with generate statements. Here we dene and instantiate a programmable-width adder with a default width of 32.

```verilog
module adder #(parameter width=32)
	(input [width-1:0] a,
	input [width-1:0] b,
	output [width:0] s
);
	s = a + b;
endmodule

module top();
	localparam adder1width = 64;
	localparam adder2width = 32;
	reg [adder1width-1:0] a,b;
	reg [adder2width-1:0] c,d;
	wire [adder1width:0] out1;
	wire [adder2width:0] out2;
	adder #(.width(adder1width)) adder64 (.a(a), .b(b), .s(out1));
	adder #(.width(adder2width)) adder32 (.a(c), .b(d), .s(out2));
endmodule
```

### for-generate loops

Generate loops are used to iteratively instantiate modules. This is useful with parameters (next slide) or when instantiating large numbers of the same module.

```verilog
wire [3:0] a, b;
genvar i;
core first_one (1'b0, a[i], b[i]);
// programmatically wire later instances
generate
	for (i = 1; i < 4 ; i = i + 1) begin:nameofthisloop
		core generated_core (a[i], a[i-1], b[i]);
	end
endgenerate
```

### Multidimensional Nets

- It is often useful to have a net organized as a two-dimensional net to make indexing easier; this is particularly useful in any memory structure.
- Syntax: `reg [M:0] <netname> [N:0]`
  - it creates a net called <netname> and describes it as an array of (N+1) elements, where each element is a (M+1) bit number

```verilog
// A memory structure that has eight 32-bit elements
reg [31:0] fifo_ram [7:0];
fifo_ram[2] // The full 3rd 32-bit element
fifo_ram[5][7:0] // The lowest byte of the 6th 32-bit element
```

### what's more

macro, include, headguard, ... if interested, try to search by yourself!

## ==\*== Verilog Environment Refinement

.[^6]

### syntax highlight, snippet, linter

- example: visual studio code
- extension: `Verilog-HDL/SystemVerilog/Bluespec SystemVerilog`
- extension: `verilog-formatter`
- tool: ctags [link-to](https://github.com/universal-ctags/ctags)
- operation: set the environment path for vivado bin folder

### batch mode

You can launch Vivado in command-line mode without opening the GUI with the flag -mode tcl. This opened an interactive TCL console, and then you can enter Tcl commands to interact with Vivado.
Or you can use the Flag -mode batch -source my_script.tcl to run vivado in batch mode. You can see a lot of scripts in the script directory.

- tool: Makefile
- tool: git, .gitignore
- scripts
- source / simulation files
- constraint files
- file hierarchy

Note: if interested, try to explore the example code repo on Canvas.

## Lab Code Review

.[^7]

### Lab5 Digital Clock

1. `always @(clk)` faster clock
2. ambiguous clock
3. dependency issues in multiple always blocks

## Reference, Credit and Notes

[^1]:J. Wright and V. Iyer, “A Verilog Primer An Overview of Verilog for Digital Design and Simulation,.” Accessed: Jun. 17, 2021. [Online]. Available: https://inst.eecs.berkeley.edu/~eecs151/fa20/files/verilog/Verilog_Primer_Slides.pdf.
[^2]: C. Fletcher, “Verilog: wire vs. reg,” 2008. Accessed: Jun. 17, 2021. [Online]. Available: https://inst.eecs.berkeley.edu/~eecs151/fa20/files/verilog/wire_vs_reg.pdf. ↩
[^3]: Note: contents marked with an ==\*== are optional, i.e., not required by this course VE270.
[^4]: C. Fletcher, “Verilog: always @ Blocks,” 2008. Accessed: Jun. 17, 2021. [Online]. Available: https://inst.eecs.berkeley.edu/~eecs151/fa20/files/verilog/always_at_blocks.pdf.

[^5]: Special thanks for VE373-SU21 TA Wei Xiangdong.
[^6]:  Special thanks for VE427-SU21 TA Yuan Yichao.

[^7]: Special thanks for VE270-SU21 TA Shi Xiaotian & Chen Yuang
[^8]: “[Synth 8-91] ambiguous clock in event control - EE2026 Design Project - Wiki.nus,” *Nus.edu.sg*, 2016. https://wiki.nus.edu.sg/display/EE2020DP/%5BSynth+8-91%5D+ambiguous+clock+in+event+control (accessed Jul. 02, 2021).

