# Week 1 - Day 2

## SKY130RTL D2SK1 L1 Introduction to .Lib 

**open the library**

```bash

 gvim ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib
```
Explanation of library:

sky130 → Refers to the SkyWater 130nm PDK (Process Design Kit).

fd → Stands for “Fully Depleted”, often used to indicate the standard cell family type.

sc → Means “Standard Cell”. This is a library of basic logic cells like AND, OR, NAND, NOR, flip-flops, etc.

hd → High Density, meaning the cells are optimized for smaller area rather than maximum speed.

tt → Typical-Typical process corner. This is a characterization condition where both NMOS and PMOS transistors are at their nominal (typical) parameters.

025C → Temperature of 25°C at which the library is characterized.

1v80 → Operating voltage of 1.8V.

.lib → The file extension. This is a Synopsys Liberty format file, which contains timing, power, and functional data for all cells in the library.

<img width="1257" height="495" alt="Screenshot 2025-09-24 161440" src="https://github.com/user-attachments/assets/568d3183-8b41-4fb9-a8a3-c9628f89d3c7" />


**Showing an AND gate with OR gate**

<img width="1261" height="800" alt="Screenshot 2025-09-24 161529" src="https://github.com/user-attachments/assets/80852737-2cde-46a5-978c-a3dd48683727" />

```bash
cell ("sky130_fd_sc_hd_a21110_2")

```

***sky130_fd_sc_hd***: SkyWater 130nm Foundry, Standard Cell, High Density

***a21110***: Cell function - 4-input AND-OR gate (2-input AND + 3-input AND into OR)

***_2***: Drive strength variant.

```bash
leakage_power () {
    value : 0.0021893000;    // Leakage power value (in watts)
    when : "IA1S1A2&1B1&1C1&D1";  // Condition when this leakage occurs
}
```

| Value (Watts) | Condition | Notes |
|---------------|-----------|-------|
| 0.0021893 | `IA1S1A2&1B1&1C1&D1` | B=HIGH, C=HIGH, D=HIGH |
| 0.0093488 | `IA1S1A2&1B1&1C1&D1` | Same condition, different leakage path |
| 0.0009247 | `IA1S1A2&1B1&C1&D1` | B=HIGH, C=LOW, D=HIGH |
| 0.0013982 | `IA1S1A2&1B1&C1&D1` | Same condition, different path |
| 0.0009175 | `IA1S1A2&8I&1C1&D1` | Different internal condition |
| 0.0013030 | `IA1S1A2&8I&1C1&D1` | Same condition, different path |
| 0.0008951 | `IA1S1A2&8I&C1&D1` | C=LOW variant |
| 0.0009198 | `IA1S1A2&8I&C1&D1` | Same condition, different path |


## SKY130RTL D2SK2 L1 Hier synthesis flat synthesis 

Hierarchical Synthesis and Flat Synthesis are two different approaches to digital circuit synthesis in VLSI design. They differ in how they handle the hierarchy and structure of the design during the optimization process.

**Hierarchical Synthesis:**

***Definition***

Hierarchical synthesis preserves the modular structure of the design. It synthesizes each submodule independently and then integrates them at the top level.

***How it Works***
Each module is synthesized separately

Submodules are treated as black boxes with defined interfaces

Hierarchy boundaries are maintained throughout the synthesis process

Top-level module connects the synthesized submodules.

```bash

heirarchical approach

module multiplier_4bit(input [3:0] a, b, output [7:0] product);
    // 4-bit multiplier implementation
endmodule

module adder_8bit(input [7:0] a, b, output [7:0] sum);
    // 8-bit adder implementation
endmodule

module multiplier_8bit(input [7:0] a, b, output [15:0] result);
    wire [7:0] prod_high, prod_low;
    wire [7:0] sum_intermediate;
    
    multiplier_4bit mult_low(.a(a[3:0]), .b(b[3:0]), .product(prod_low));
    multiplier_4bit mult_high(.a(a[7:4]), .b(b[7:4]), .product(prod_high));
    adder_8bit adder(.a(prod_high), .b(prod_low), .sum(sum_intermediate));
    // Additional logic...
endmodule
```

**Flat Synthesis:**
***Definition***
Flat synthesis flattens the entire design hierarchy into one large module and optimizes it as a single entity.

***How it Works***
All hierarchy boundaries are removed

The entire design is treated as one massive combinational/sequential circuit

Global optimization across module boundaries

No preservation of original module structure.



**Example - multiple_module**

```bash

#To open the multiple_modules
gvim multiple_modules.v
```
<img width="1284" height="808" alt="Screenshot 2025-09-24 095935" src="https://github.com/user-attachments/assets/110c3d81-a8d3-4750-8f97-b637bfc9460e" />

**For hier synthesis:**

```bash

#open the yosys
yosys

#Read the liberty file
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#Read the verilog file
read_verilog multiple_modules.v

#for synthesis
synth -top multiple-modules

#run the ABC tool
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#see the hierarchy
show multiple_modules
```

<img width="1283" height="805" alt="Screenshot 2025-09-24 100645" src="https://github.com/user-attachments/assets/4613d933-e428-4a7a-afc5-faa387617c56" />

```bash
#Export the design with all modules and hierarchy intact
write_verilog -noattr multiple_modules_hier.v

#show the netlist
!gvim multiple_modules_hier.v
```
<img width="1282" height="800" alt="Screenshot 2025-09-24 101118" src="https://github.com/user-attachments/assets/0b22ee30-9e76-46ee-8fc2-3b31c6693dd9" />
<img width="1287" height="809" alt="Screenshot 2025-09-24 101132" src="https://github.com/user-attachments/assets/968b28f2-966e-400b-8604-7d0e7996214e" />

**For flat synthesis:**

```bash

#open the yosys
yosys

#Read the liberty file
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#Read the verilog file
read_verilog multiple_modules.v

#for synthesis
synth -top multiple-modules

#run the ABC tool
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#For flat
flattened

#see the hierarchy
show multiple_modules
```

<img width="1280" height="800" alt="Screenshot 2025-09-24 102234" src="https://github.com/user-attachments/assets/72aa1ce7-d38c-4493-8ff6-5ac445820529" />

```bash
#Export the design with all modules and flattend intact
write_verilog -noattr multiple_modules_hier.v

#show the netlist
!gvim multiple_modules_hier.v
```
<img width="1284" height="805" alt="Screenshot 2025-09-24 101356" src="https://github.com/user-attachments/assets/679e44e4-c3af-4476-bee3-75950b51cc55" />
<img width="1289" height="795" alt="Screenshot 2025-09-24 101410" src="https://github.com/user-attachments/assets/f7016a54-baa7-4144-86e3-51818cbfa128" />

## SKY130RTL D2SK3 L1 Why Flops and Flop coding styles

**Introduction to Flip-Flops (Flops)**
Flip-flops are fundamental building blocks of digital circuits that store state information. They are essential for:

Sequential logic (vs combinational logic), State retention across clock cycles, Synchronization in digital systems, Timing control in pipelined design.

**Why We Need Flip-Flops:**

***State Storage and Memory***

```bash
verilog
// Without flip-flop - purely combinational (no memory)
assign output = input_a & input_b;

// With flip-flop - stores state
always @(posedge clk) begin
    stored_value <= input_a & input_b;
end
```

***Synchronization***
Clock domain synchronization, Metastability prevention, Reliable data transfer.

# **Different coding styles**

**1. Flop with asynchronous reset:**

```bash
#Open the code
gvim dff_asyncres.v
```

<img width="1283" height="797" alt="Screenshot 2025-09-24 150530" src="https://github.com/user-attachments/assets/d2b0c973-4152-4ead-a500-6c8d9fb5c3b3" />


```bash
#Compile Verilog files using Icarus Verilog
iverilog dff_asyncres.v tb_dff_asyncres.v

#Run the compiled simulation
./a.out

#View waveform results in GTKWave
gtkwave dumpfile.vcd

```

<img width="1282" height="802" alt="Screenshot 2025-09-24 151020" src="https://github.com/user-attachments/assets/52158766-ded0-4a9a-a036-3bd0dfe9b3ac" />

**For synthesis**

```bash

#open the yosys
yosys

#Read the liberty file
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#Read the verilog file
read_verilog  dff_asyncres.v

#for synthesis
synth -top  dff_asyncres

dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#run the ABC tool
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#see the hierarchy
show 
```

<img width="1288" height="814" alt="Screenshot 2025-09-24 151519" src="https://github.com/user-attachments/assets/771677c4-baa0-4dc3-8422-1caf856ea174" />


**2. Flop with asynchronous set:**

```bash
#Open the code
gvim dff_async_set.v
```
<img width="1291" height="804" alt="Screenshot 2025-09-24 150632" src="https://github.com/user-attachments/assets/aec7fd52-c2b5-4011-ae89-4b4c85df8ae9" />

```bash
#Compile Verilog files using Icarus Verilog
iverilog dff_async_set.v tb_dff_async_set.v

#Run the compiled simulation
./a.out

#View waveform results in GTKWave
gtkwave dumpfile.vcd
```
<img width="1289" height="808" alt="Screenshot 2025-09-24 151737" src="https://github.com/user-attachments/assets/af5c0f11-4595-4e7c-8791-36c591eace89" />


**For synthesis**

```bash

#open the yosys
yosys

#Read the liberty file
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#Read the verilog file
read_verilog  dff_async_set.v

#for synthesis
synth -top  dff_async_set

dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#run the ABC tool
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#see the hierarchy
show 
```

<img width="1285" height="810" alt="Screenshot 2025-09-24 152140" src="https://github.com/user-attachments/assets/2ec66a6b-9864-4b52-8cca-48f0b72227bf" />

**3. Flop with asynchronous and synchronous reset:**

```bash
#Open the code
gvim dff_async_set.v
```
<img width="1282" height="808" alt="Screenshot 2025-09-24 150716" src="https://github.com/user-attachments/assets/79ff8f3d-7e7b-42e6-9052-e88dacb3a1b0" />

```bash
#Compile Verilog files using Icarus Verilog
iverilog dff_asyncres_syncres.v tb_dff_async_synres.v

#Run the compiled simulation
./a.out

#View waveform results in GTKWave
gtkwave dumpfile.vcd
```
<img width="1280" height="794" alt="Screenshot 2025-09-24 152337" src="https://github.com/user-attachments/assets/186587d8-106d-4033-8c85-819127c7c079" />


**For synthesis**

```bash

#open the yosys
yosys

#Read the liberty file
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#Read the verilog file
read_verilog dff_asyncres_syncres.v

#for synthesis
synth -top  dff_asyncres_syncres

dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#run the ABC tool
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#see the hierarchy
show 
```

<img width="1288" height="800" alt="Screenshot 2025-09-24 152531" src="https://github.com/user-attachments/assets/26c2936a-b192-49cc-aef4-b13cf2b4b7b1" />


**4. Flop with synchronous reset:**

```bash
#Open the code
gvim dff_syncres.v
```

<img width="1289" height="805" alt="Screenshot 2025-09-24 150738" src="https://github.com/user-attachments/assets/5ba1a988-ada5-4ac1-aa18-9d8b3057b5d8" />


```bash
#Compile Verilog files using Icarus Verilog
iverilog dff_syncres.v tb_dff_syncres.v

#Run the compiled simulation
./a.out

#View waveform results in GTKWave
gtkwave dumpfile.vcd

```

<img width="1290" height="804" alt="Screenshot 2025-09-24 152708" src="https://github.com/user-attachments/assets/2dc11128-7706-4592-b2b6-2a19c44bbf19" />


**For synthesis**

```bash

#open the yosys
yosys

#Read the liberty file
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#Read the verilog file
read_verilog  dff_syncres.v

#for synthesis
synth -top  dff_syncres

dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#run the ABC tool
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#see the hierarchy
show 
```

<img width="1284" height="808" alt="Screenshot 2025-09-24 152928" src="https://github.com/user-attachments/assets/242746f2-5be3-4f4e-b908-5783a48ae438" />

# SKY130RTL D2SK3 L5 Interesting optimisations

**1. mult_2, mult_8**
```bash
#Open the module
gvim mult_2.v
gvim mult_8.v
```
<img width="1284" height="810" alt="Screenshot 2025-09-24 153618" src="https://github.com/user-attachments/assets/9db762d7-ac08-4677-9d75-d7edf8854540" />

**For synthesis of mult_2**

```bash

#open the yosys
yosys

#Read the liberty file
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#Read the verilog file
read_verilog mult_2.v

#for synthesis
synth -top  mult2

#run the ABC tool
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib
```


<img width="1285" height="802" alt="Screenshot 2025-09-24 160517" src="https://github.com/user-attachments/assets/484b34fd-8aee-40a2-aef0-115823a0d37a" />


Don't call ABC as there is nothing to map: If the compiler sees assign y = a * b; and no gate-level expansion has been done yet, ABC cannot map it to a library because it’s still a high-level expression, not a gate-level netlist.

```bash

#show the heiracrhcy
show
```
<img width="1289" height="802" alt="Screenshot 2025-09-24 160536" src="https://github.com/user-attachments/assets/dc0bdf9c-2bf0-40a4-959a-7ae7db00325b" />

**For synthesis of mult_8**

```bash

#open the yosys
yosys

#Read the liberty file
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#Read the verilog file
read_verilog mult_8.v

#for synthesis
synth -top  mult8

#run the ABC tool
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib
```


<img width="1283" height="807" alt="Screenshot 2025-09-24 160724" src="https://github.com/user-attachments/assets/fba3b795-b3ba-4e97-af8e-e199edd03e90" />



Don't call ABC as there is nothing to map: If the compiler sees assign y = a * b; and no gate-level expansion has been done yet, ABC cannot map it to a library because it’s still a high-level expression, not a gate-level netlist.

```bash

#show the heiracrhcy
show
```
<img width="1284" height="803" alt="Screenshot 2025-09-24 160750" src="https://github.com/user-attachments/assets/1d975d4e-fc31-41f0-a947-570896e9efbc" />


















































