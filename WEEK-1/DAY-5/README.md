# Week 1 - Day 5

## SKY130RTL D5SK1 L1 IF CASE Constructs

## IF STATEMENT

if is used to make decisions inside procedural blocks like always or initial. Based on a condition, certain statements are executed.

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

