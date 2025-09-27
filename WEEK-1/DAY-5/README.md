# Week 1 - Day 5

## SKY130RTL D5SK1 L1 IF Constructs

## IF STATEMENT

If is used to make decisions inside procedural blocks like always or initial. Based on a condition, certain statements are executed.

```bash
if (condition) begin
    // statements executed if condition is true
end
else begin
    // statements executed if condition is false
end
```
**Important Notes in Verilog**

* If can be used inside always blocks only.

* Do not use if directly in module port declarations or outside procedural blocks.

* You can chain multiple conditions using else if.

**NESTED IF ELESE**
```bash
  if (condition1) begin
    // Code for condition1 true
end else if (condition2) begin
    // Code for condition2 true
end else begin
    // Code if no conditions are true
end
```

## Danger/caution with IF statement (Inferred latches)

In Verilog, a latch is a memory element that stores a value until it is explicitly changed.

* Latches are level-sensitive (not edge-triggered like flip-flops).

* Inferred latches happen when Verilog synthesizes a latch because some branches of an if statement do not assign a value to a reg

**Example**

**1. MUX**

**Wrong code (with latch):**
```bash
module mux_wrong(
    input sel,
    input a,
    input b,
    output reg y
);

always @(*) begin
    if (sel == 0)
        y = a;       // y assigned only if sel==0
    // missing else for sel==1
end

endmodule

❌ Problem:

When sel == 1, y is not assigned, so Verilog infers a latch to store the previous value of y
```

**Correct Code (No Latch):**

```bash
module mux_wrong(
    input sel,
    input a,
    input b,
    output reg y
);

always @(*) begin
    if (sel == 0)
        y = a;       // y assigned only if sel==0
    // missing else for sel==1
end

endmodule
```
**2.**

**counter**

**wrong code (inferred code)**

```bash
module counter_wrong(
    input clk,
    input reset,
    output reg [3:0] count
);

always @(posedge clk) begin
    if (reset)
        count = 0;        // count assigned only if reset is high
    // missing else branch
end

endmodule

❌ Problem:

* When reset == 0, count is not assigned inside the always block.

* Synthesizer infers a latch to remember the previous value of count.

* For sequential logic, we typically want a flip-flop, not a latch.
```
**correct code(no latch)**

```bash

module counter_right(
    input clk,
    input reset,
    output reg [3:0] count
);

always @(posedge clk) begin
    if (reset)
        count <= 0;       // reset
    else
        count <= count + 1; // increment
end

endmodule
```


## SKY130RTL D5SK1 L1 CASE Constructs

* The case statement is a multi-way decision construct, similar to switch in C/C++.

* It is used when you want to select one action among many based on the value of a variable.

* Often used in state machines, decoders, multiplexers, etc.

```bash
**syntax**
case (expression)
    value1: begin
        // statements if expression == value1
    end
    value2: begin
        // statements if expression == value2
    end
    ...
    default: begin
        // statements if none of the above match
    end
endcase
```

# Danger caution with CASE statement(inferred latches)

# 1. Incomplete CASE:

A case statement in Verilog is like a multi-branch if-else.
If not all possible input values are covered (and no default is given), then some outputs may not get assigned, leading to inferred latches in combinational logic.

**Example: 2-to-1 MUX using case**

** wrong code(with latch)**

```bash
module mux_case_wrong(
    input [1:0] sel,
    input a, b, c,
    output reg y
);

always @(*) begin
    case(sel)
        2'b00: y = a;
        2'b01: y = b;
        // Missing cases for 2'b10 and 2'b11
    endcase
end

endmodule

❌ Problem:

* If sel = 2'b10 or sel = 2'b11, y is not assigned.

* Synthesis tool infers a latch to "remember" old y.

* This is usually not intended in combinational logi
```

**right code(no latch)**

```bash

//covering all cases
module mux_case_right(
    input [1:0] sel,
    input a, b, c,
    output reg y
);

always @(*) begin
    case(sel)
        2'b00: y = a;
        2'b01: y = b;
        2'b10: y = c;
        2'b11: y = 0;   // explicitly handle all cases
    endcase
end

endmodule

// by using default statement
module mux_case_right2(
    input [1:0] sel,
    input a, b, c,
    output reg y
);

always @(*) begin
    case(sel)
        2'b00: y = a;
        2'b01: y = b;
        2'b10: y = c;
        default: y = 0;  // catch-all
    endcase
end

endmodule

```

# 2.Case caveat (partial assignments in case)

* Inside a case statement, if not all outputs are assigned in every case branch, the synthesizer will infer latches for the unassigned signals.

* This is because the unassigned signals must "hold their old value" when not updated.


**Wrong Example (Partial Assignment → Inferred Latch)**

```bash
module alu_partial_wrong(
    input [1:0] sel,
    input [3:0] a, b,
    output reg [3:0] y,
    output reg carry
);

always @(*) begin
    case (sel)
        2'b00: y = a + b;     // only y assigned, carry missing
        2'b01: y = a - b;     // only y assigned, carry missing
        2'b10: carry = a[0];  // only carry assigned, y missing
        default: ;            // nothing assigned
    endcase
end

endmodule

❌ Problems here:

* In sel = 00 and 01, carry is not assigned → inferred latch.

* In sel = 10, y is not assigned → inferred latch.

* In default, nothing is assigned → both y and carry latch.
```

**Correct Example (Complete Assignments)**


```bash
module alu_partial_right(
    input [1:0] sel,
    input [3:0] a, b,
    output reg [3:0] y,
    output reg carry
);

always @(*) begin
    case (sel)
        2'b00: begin 
            y = a + b; 
            carry = 0; 
        end
        2'b01: begin 
            y = a - b; 
            carry = 0; 
        end
        2'b10: begin 
            y = 0; 
            carry = a[0]; 
        end
        default: begin 
            y = 0; 
            carry = 0; 
        end
    endcase
end

endmodule

```





















