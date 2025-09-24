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

**Hierarchical Synthesis**

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

**Flat Synthesis**
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

**For hier synthesis**

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

**For flat synthesis**

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





















