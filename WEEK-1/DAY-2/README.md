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







