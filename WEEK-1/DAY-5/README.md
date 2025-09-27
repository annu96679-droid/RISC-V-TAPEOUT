# Week 1 - Day 5

## SKY130RTL D5SK1 IF Constructs

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
**2.Counter**

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


## SKY130RTL D5SK1 CASE Constructs

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

# SKY130RTL D5SK2 L1 Lab Incomplete IF

# 1. incomp_if

```bash
#Open the code
gvim incomp_if
```

<img width="1150" height="808" alt="Screenshot 2025-09-27 143009" src="https://github.com/user-attachments/assets/e4873b0e-cdf1-4161-97d9-349b8f1f2a56" />


```bash
#Compile Verilog files using Icarus Verilog
iverilog incomp_if.v tb_incomp_if.v

#Run the compiled simulation
./a.out

#View waveform results in GTKWave
gtkwave dumpfile.vcd

```
<img width="1259" height="812" alt="Screenshot 2025-09-27 143204" src="https://github.com/user-attachments/assets/ee7548c8-3243-4842-b643-835cd064658b" />



**For synthesis**

```bash

#open the yosys
yosys

#Read the liberty file
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#Read the verilog file
read_verilog  incomp_if.v

#for synthesis
synth -top incomp_if
```
<img width="1092" height="309" alt="Screenshot 2025-09-27 143319" src="https://github.com/user-attachments/assets/eb2bad17-52e4-44f9-8b30-cf4a6b83654e" />


```bash

#run the ABC tool
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#see the hierarchy
show 
```
<img width="1092" height="309" alt="Screenshot 2025-09-27 143319" src="https://github.com/user-attachments/assets/c802fc5c-67b3-42a5-b0db-8c35a7be6b0a" />

# 2. incomp_if2

```bash
#Open the code
gvim incomp_if2
```

<img width="1262" height="804" alt="Screenshot 2025-09-27 143453" src="https://github.com/user-attachments/assets/f0eb0cde-a835-42d0-9508-c4746ed4591d" />


```bash
#Compile Verilog files using Icarus Verilog
iverilog incomp_if2.v tb_incomp_if2.v

#Run the compiled simulation
./a.out

#View waveform results in GTKWave
gtkwave dumpfile.vcd

```
<img width="1256" height="802" alt="Screenshot 2025-09-27 143605" src="https://github.com/user-attachments/assets/f8f41c18-2bc2-40ed-a027-e5225a1f22e4" />


**For synthesis**

```bash

#open the yosys
yosys

#Read the liberty file
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#Read the verilog file
read_verilog  incomp_if2.v

#for synthesis
synth -top incomp_if2
```
<img width="1191" height="318" alt="Screenshot 2025-09-27 143714" src="https://github.com/user-attachments/assets/869782a4-54d6-470d-b3bb-df2207a0b6dd" />


```bash

#run the ABC tool
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#see the hierarchy
show 
```
<img width="1267" height="813" alt="Screenshot 2025-09-27 143751" src="https://github.com/user-attachments/assets/1f36975c-07f9-4ee9-a960-085bab234f26" />

# SKY130RTL D5SK3 L1 Lab incomplete overlapping Case:


# 1. incomp_case

```bash
#Open the code
gvim incomp_if2
```

<img width="1256" height="812" alt="Screenshot 2025-09-27 143833" src="https://github.com/user-attachments/assets/82f4a763-cada-4b97-8bd7-bb8f444a3ca8" />

```bash
#Compile Verilog files using Icarus Verilog
iverilog incomp_if2.v tb_incomp_if2.v

#Run the compiled simulation
./a.out

#View waveform results in GTKWave
gtkwave dumpfile.vcd

```
<img width="1264" height="804" alt="Screenshot 2025-09-27 144006" src="https://github.com/user-attachments/assets/aceaf64f-ff45-4988-8705-0adf9997350c" />



**For synthesis**

```bash

#open the yosys
yosys

#Read the liberty file
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#Read the verilog file
read_verilog  incomp_if2.v

#for synthesis
synth -top incomp_if2
```
<img width="1106" height="393" alt="Screenshot 2025-09-27 144059" src="https://github.com/user-attachments/assets/186c0494-ffcc-424c-9ba7-c20a678e099f" />


```bash

#run the ABC tool
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#see the hierarchy
show 
```
<img width="1259" height="806" alt="Screenshot 2025-09-27 144125" src="https://github.com/user-attachments/assets/a67578be-2d5a-48b9-9a82-aed924452080" />

# 2. comp_case

```bash
#Open the code
gvim comp_case2
```

<img width="1254" height="813" alt="Screenshot 2025-09-27 144405" src="https://github.com/user-attachments/assets/ca434b6e-c920-4893-bdf8-0e38e03636d8" />


```bash
#Compile Verilog files using Icarus Verilog
iverilog comp_case.v tb_comp_case.v

#Run the compiled simulation
./a.out

#View waveform results in GTKWave
gtkwave dumpfile.vcd

```
<img width="1258" height="805" alt="Screenshot 2025-09-27 144544" src="https://github.com/user-attachments/assets/02c79a51-74f9-490e-81a1-e76f91d89c39" />

**For synthesis**

```bash

#open the yosys
yosys

#Read the liberty file
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#Read the verilog file
read_verilog  comp_case.v

#for synthesis
synth -top comp_if2
```
<img width="1220" height="398" alt="Screenshot 2025-09-27 144629" src="https://github.com/user-attachments/assets/7660169b-31fe-4e25-8345-8fdb75119476" />


```bash

#run the ABC tool
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#see the hierarchy
show 
```
<img width="1253" height="792" alt="Screenshot 2025-09-27 144650" src="https://github.com/user-attachments/assets/3887739d-b984-49db-a68b-ecc13f84ee1e" />



# 3. partial case assign

```bash
#Open the code
gvim partial_case_assign.v
```

<img width="1282" height="800" alt="Screenshot 2025-09-27 145851" src="https://github.com/user-attachments/assets/f9a9a20f-c418-4bde-b8df-d1036694a911" />


**For synthesis**

```bash

#open the yosys
yosys

#Read the liberty file
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#Read the verilog file
read_verilog  partial_case_assign.v

#for synthesis
synth -top partial_case_assign
```
<img width="1134" height="432" alt="Screenshot 2025-09-27 150050" src="https://github.com/user-attachments/assets/7d0bb175-0100-40b4-8fbf-cf01ad7fc294" />



```bash

#run the ABC tool
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#see the hierarchy
show 
```
<img width="1287" height="800" alt="Screenshot 2025-09-27 150110" src="https://github.com/user-attachments/assets/b0d985ca-ba0a-49e2-b83e-4df62ea5de0e" />

# 4. bad_case
```bash
#Open the code
gvim bad_case.v
```
<img width="1290" height="813" alt="Screenshot 2025-09-27 150140" src="https://github.com/user-attachments/assets/9d9beae6-0360-4096-894b-6d38960a0941" />



```bash
#Compile Verilog files using Icarus Verilog
iverilog bad_case.v tb_bad_case.v

#Run the compiled simulation
./a.out

#View waveform results in GTKWave
gtkwave dumpfile.vcd

```
<img width="1285" height="804" alt="Screenshot 2025-09-27 150308" src="https://github.com/user-attachments/assets/86a8156e-9460-41e8-a994-5136dbd00388" />



**For synthesis**

```bash

#open the yosys
yosys

#Read the liberty file
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#Read the verilog file
read_verilog bad_case.v

#for synthesis
synth -top  bad_case
```
<img width="1039" height="385" alt="Screenshot 2025-09-27 154541" src="https://github.com/user-attachments/assets/f524a785-236b-41eb-a83b-f2bfae8e21d9" />




```bash

#run the ABC tool
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#export a clean gate-level Verilog netlist
write_verilog -noattr bad_case_net.v

#see the hierarchy
show 
```
<img width="1289" height="806" alt="Screenshot 2025-09-27 154642" src="https://github.com/user-attachments/assets/fa40caf5-8813-4c37-a9a6-00a6ddbacf6a" />



**simulation after Gate Level Synthesis**
```bash

#compiles the standard-cell library, gate-level netlist, and testbench into a simulation executable
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130-fd-sc-hd.v bad_case_net.v tb_bad_case.v

#Run the compile simulation
./a.out

#View waveform results in GTKWave
gtkwave dumpfile.vcd
```

<img width="1283" height="807" alt="Screenshot 2025-09-27 155957" src="https://github.com/user-attachments/assets/5fe43c0b-1985-44c6-9545-498a445a28e3" />

# SKY130RTL D5SK5 For and For Generate 

# for loop:

It is used for repetition during simulation or hardware generation

```bash
//syntax
for (init; condition; step) begin
    // statements
end
```
**Ex : 4:1 MUX Using case Statement**

```bash
module mux4_case(
    input [1:0] sel,
    input [3:0] d,    // d[0], d[1], d[2], d[3]
    output reg y
);

always @(*) begin
    case (sel)
        2'b00: y = d[0];
        2'b01: y = d[1];
        2'b10: y = d[2];
        2'b11: y = d[3];
        default: y = 0;   // safety (not strictly needed)
    endcase
end

endmodule
```

```bash

//By using for loop
module mux4_for(
    input [1:0] sel,
    input [3:0] d,    // d[0], d[1], d[2], d[3]
    output reg y
);

integer i;
always @(*) begin
    y = 0;  // default assignment
    for (i = 0; i < 4; i = i + 1) begin
        if (sel == i)
            y = d[i];
    end
end

endmodule
```
# SKY130RTL D5SK5 For Generate :

* In Verilog, the generate block is used when you want to replicate hardware structures multiple times.

* A for loop inside generate tells the synthesizer: “Make multiple copies of this hardware, once for each loop iteration.”

```bash

//syntax
genvar i;
generate
    for (i = start; i < end; i = i + 1) begin : label
        // repeated hardware here
    end
endgenerate
```

**Ex : 4-bit Ripple carry adder**

```bash
 // Normally instantiation

module full_adder (
    input  a, b, cin,
    output sum, cout
);
    assign {cout, sum} = a + b + cin;
endmodule

module ripple_adder4_normal(
    input  [3:0] a, b,
    input  cin,
    output [3:0] sum,
    output cout
);

wire c1, c2, c3; // internal carries

// Instantiate full adders manually
full_adder fa0(a[0], b[0], cin,  sum[0], c1);
full_adder fa1(a[1], b[1], c1,   sum[1], c2);
full_adder fa2(a[2], b[2], c2,   sum[2], c3);
full_adder fa3(a[3], b[3], c3,   sum[3], cout);

endmodule
```

```bash

// instantiation by using the for generate

module ripple_adder4_generate(
    input  [3:0] a, b,
    input  cin,
    output [3:0] sum,
    output cout
);

wire [3:0] carry;  // internal carries

genvar i;
generate
    for (i = 0; i < 4; i = i + 1) begin : fa_loop
        if (i == 0)
            full_adder fa(a[i], b[i], cin, sum[i], carry[i]);
        else if (i == 3)
            full_adder fa(a[i], b[i], carry[i-1], sum[i], cout);
        else
            full_adder fa(a[i], b[i], carry[i-1], sum[i], carry[i]);
    end
endgenerate

endmodule
```

# SKY130RTL D5SK5 Lab For and For Generate

# 1. mux_generate

```bash
#Open the code
gvim mux_generate.v
```
<img width="1286" height="809" alt="Screenshot 2025-09-27 160217" src="https://github.com/user-attachments/assets/a0d844b8-5d8b-4c47-90c4-da8700492dcc" />


```bash
#Compile Verilog files using Icarus Verilog
iverilog mux_generate.v tb_mux_generate.v

#Run the compiled simulation
./a.out

#View waveform results in GTKWave
gtkwave dumpfile.vcd

```
<img width="1280" height="804" alt="Screenshot 2025-09-27 161432" src="https://github.com/user-attachments/assets/70e2a3b2-deb3-4eb7-acf2-ed7a5a355dd4" />


**For synthesis**

```bash

#open the yosys
yosys

#Read the liberty file
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#Read the verilog file
read_verilog mux_generate.v

#for synthesis
synth -top  mux_generate
```
<img width="1165" height="372" alt="Screenshot 2025-09-27 161621" src="https://github.com/user-attachments/assets/a0ac5c98-0e64-402f-ada1-f5becba46c95" />


```bash

#run the ABC tool
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#export a clean gate-level Verilog netlist
write_verilog -noattr mux_generate_net.v

#see the hierarchy
show 
```
<img width="1284" height="811" alt="Screenshot 2025-09-27 161725" src="https://github.com/user-attachments/assets/5c92dd00-47eb-4efa-8322-f3e95393f7b1" />


**simulation after Gate Level Synthesis**
```bash

#compiles the standard-cell library, gate-level netlist, and testbench into a simulation executable
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130-fd-sc-hd.v mux_generate_net.v tb_mux_generate.v

#Run the compile simulation
./a.out

#View waveform results in GTKWave
gtkwave dumpfile.vcd
```

<img width="1280" height="803" alt="Screenshot 2025-09-27 162018" src="https://github.com/user-attachments/assets/0ff1c4db-9fe7-4f8d-8962-2afa70eb675d" />


# 2. demux_generate

```bash
#Open the code
gvim demux_generate.v
```
<img width="1275" height="807" alt="Screenshot 2025-09-27 162124" src="https://github.com/user-attachments/assets/f4f11ab2-e3ee-47a8-a3fa-8725e5197091" />


```bash
#Compile Verilog files using Icarus Verilog
iverilog demux_generate.v tb_demux_generate.v

#Run the compiled simulation
./a.out

#View waveform results in GTKWave
gtkwave dumpfile.vcd

```
<img width="1274" height="804" alt="Screenshot 2025-09-27 162302" src="https://github.com/user-attachments/assets/02e9a36b-8e5e-4721-9cef-fced46ab769d" />


**For synthesis**

```bash

#open the yosys
yosys

#Read the liberty file
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#Read the verilog file
read_verilog demux_generate.v

#for synthesis
synth -top  demux_generate
```
<img width="1155" height="395" alt="Screenshot 2025-09-27 162357" src="https://github.com/user-attachments/assets/4d2cdc99-6162-498a-bd98-f8cef2336783" />

```bash

#run the ABC tool
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#export a clean gate-level Verilog netlist
write_verilog -noattr demux_generate_net.v

#see the hierarchy
show 
```
<img width="390" height="671" alt="Screenshot 2025-09-27 162648" src="https://github.com/user-attachments/assets/36320d59-763c-43b0-a144-352b97c51cb4" />
<img width="1282" height="797" alt="Screenshot 2025-09-27 162543" src="https://github.com/user-attachments/assets/54a9e705-c0d7-4e73-b1c8-01997faf0069" />


**simulation after Gate Level Synthesis**
```bash

#compiles the standard-cell library, gate-level netlist, and testbench into a simulation executable
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130-fd-sc-hd.v demux_generate_net.v tb_demux_generate.v

#Run the compile simulation
./a.out

#View waveform results in GTKWave
gtkwave dumpfile.vcd
```

<img width="1282" height="810" alt="Screenshot 2025-09-27 163053" src="https://github.com/user-attachments/assets/9b44e8d5-1fdb-4ac1-8634-7b94963a1441" />


# 4. RIPPLE CARRY ADDER:

```bash
#Open the code
gvim fa.v rca.v
```
<img width="1257" height="815" alt="Screenshot 2025-09-27 183940" src="https://github.com/user-attachments/assets/4ffb3da3-c177-4016-873d-ec2561f829c3" />


```bash
#Compile Verilog files using Icarus Verilog
iverilog fa.v rca.v tb_rca.v

#Run the compiled simulation
./a.out

#View waveform results in GTKWave
gtkwave dumpfile.vcd

```
<img width="1262" height="808" alt="Screenshot 2025-09-27 184123" src="https://github.com/user-attachments/assets/99c14c79-c540-45c1-8352-8f3dcf835600" />


**For synthesis**

```bash

#open the yosys
yosys

#Read the liberty file
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#Read the verilog file
read_verilog fa.v rca.v

#for synthesis
synth -top rca
```


<img width="979" height="373" alt="Screenshot 2025-09-27 184354" src="https://github.com/user-attachments/assets/4e636dcc-76b5-440b-a121-fc22cf8c00fe" />


<img width="1264" height="739" alt="Screenshot 2025-09-27 184657" src="https://github.com/user-attachments/assets/9de59f11-035c-4f05-8961-d3a67b771a31" />


```bash

#run the ABC tool
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#export a clean gate-level Verilog netlist
write_verilog -noattr rca_net.v

#see the hierarchy
show 
```

<img width="1263" height="804" alt="Screenshot 2025-09-27 184840" src="https://github.com/user-attachments/assets/c9c2109d-4302-4e9b-b9bf-8d4dcd6233de" />
















