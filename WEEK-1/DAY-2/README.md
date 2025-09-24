# Week 1 - Day 2

## SKY130.lib

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




