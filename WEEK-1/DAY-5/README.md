# Week 1 - Day 5

## SKY130RTL D5SK1 L1 IF CASE Constructs

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
```bash







