# Week 1 - Day 3

## Combinational and Sequential optimizations

## Introduction to optimizations

Optimization is the scientific discipline of choosing the best element from a set of available alternatives. It's about making something as effective or functional as possible, given a set of constraints.

In more formal, mathematical terms, optimization involves:

Maximizing or Minimizing a specific quantity called the objective function (e.g., profit, cost, efficiency, error).

By systematically choosing the values of decision variables (e.g., how much product to make, which route to take).

Subject to a set of constraints (e.g., limited resources, physical laws, budget limits).

**Logic optimization operates at different levels of abstraction and uses a variety of techniques:**

***1. Two-Level Logic Optimization (Sum-of-Products - SOP)***
This is the classic and most well-studied form of logic optimization. The goal is to minimize a Boolean function expressed as a Sum-of-Products (e.g., F = AB + AC + BC).

a) Karnaugh Maps (K-maps)

What it is: A graphical method for simplifying Boolean expressions with up to 4 or 5 variables. It's a grid where each cell represents a minterm (a single product term) from the truth table.

How it works: You plot the 1s from the truth table onto the K-map. The optimization involves grouping adjacent 1s in the largest possible powers of two (1, 2, 4, 8, etc.). Each group corresponds to a simplified product term.

Limitation: Becomes impractical for more than 5-6 variables due to geometric complexity.

b) Quine-McCluskey Algorithm

What it is: A tabular, algorithmic method that is functionally equivalent to K-maps but can be automated and handles a larger number of variables.

How it works:

Generate Prime Implicants: It starts by listing all minterms (product terms where the function is 1). It then systematically compares and combines minterms that differ in only one variable, creating larger "cubes." Terms that cannot be combined further are called Prime Implicants—these are the largest possible product terms that still imply the function.

Select a Minimum Cover: A table (Prime Implicant Chart) is created with prime implicants as rows and original minterms as columns. The goal is to find the smallest set of prime implicants that "cover" all the minterms where the function is 1. This is a classic set cover problem.

***2. Multi-Level Logic Optimization****
Two-level logic (SOP) is often not area-efficient for complex functions. Real-world circuits are multi-level, meaning signals pass through several layers of gates. Multi-level optimization is more complex but can yield far better results.

Techniques include:

Factoring: Finding common sub-expressions. For example, the expression F = AB + AC + AD can be factored into F = A(B + C + D), reducing the number of gates needed.

Decomposition: Breaking down a complex function into a network of smaller, simpler sub-functions.

Extraction: Identifying and re-using common sub-expressions across multiple functions within a larger circuit.

Substitution: Expressing one function in terms of another already present in the circuit.

Tools for Multi-Level Optimization: This level of complexity requires sophisticated software tools. The Espresso logic minimizer is a famous and highly effective tool for two-level minimization that uses heuristics to quickly find near-optimal solutions. For multi-level optimization, systems like SIS (Sequential Interactive System) and modern commercial EDA tools (from Synopsys, Cadence, etc.) use a combination of algorithms.

***3. Technology Mapping***
This is the final, critical step where the optimized Boolean logic is mapped onto the actual gates available in a specific technology library (e.g., a standard cell library for an ASIC or the basic logic elements of an FPGA).

The Problem: Your optimized function might be F = (A AND B) OR C. However, your cell library might not have a 3-input OR gate. It might only have 2-input NAND gates.

The Solution: Technology mapping transforms the generic, optimized logic into a netlist (a list of connections) using only the gates available in the library. This involves representing the logic using a universal gate (like a NAND) and then "covering" this representation with the available library cells, often using graph-based algorithms.

## Combinational Logic optimization 

Combinational Logic Optimization is the process of transforming a combinational logic circuit into an equivalent circuit that minimizes one or more design metrics while preserving the exact input-output behavior.

**Fundamental Techniques and Algorithms**

***1. Two-Level Logic Optimization***

A. Karnaugh Maps (K-maps)
Concept: Graphical method for simplifying Boolean functions with 2-6 variables.

Procedure:

Create a grid with input variable combinations

Plot 1s for minterms where function outputs 1

Group adjacent 1s in largest possible powers of two (1, 2, 4, 8...)

Each group corresponds to a simplified product term.

***B. Boolean logic optimization***

<img width="2385" height="1215" alt="boolean_optimization" src="https://github.com/user-attachments/assets/ef0ecfb2-4041-4aff-9980-bdd940ac782b" />


Boolean logic optimization transforms a Boolean expression into an equivalent form that:

Uses fewer logic gates

Reduces propagation delay

Lowers power consumption

Maintains identical logical functionality

## Sequential Logic optimization

Sequential-logic optimization = making the registered parts of your design meet timing, area, and power goals while preserving functionality.
Goals (often competing): increase fmax / reduce clock period, reduce area, reduce power, improve reliability (hold/meta), and reduce latency (or accept more latency to improve throughput).

**1. Sequential Constant Propagation**

Sequential Constant Propagation extends the classic constant propagation optimization to sequential circuits by propagating constant values through registers over multiple clock cycles.

***How It Works***
Basic Principle:
Identify flip-flops that eventually settle to constant values

Propagate these constants through the sequential logic

Simplify the circuit by replacing constant-controlled logic.

**2. State Optimization**

State optimization in sequential circuits means simplifying the finite state machine (FSM) so that it uses fewer states or better state encodings without changing how the circuit behaves. In many designs, some states are redundant because they always produce the same outputs and lead to the same future states as others, so these can be merged into one. This process is called state reduction and it makes the circuit smaller and simpler. Even when the number of states is fixed, the way we assign binary codes to the states affects speed, area, and power.

**3. Retiming**

***Retiming Formulation***

<img width="2385" height="1215" alt="retiming_diagram" src="https://github.com/user-attachments/assets/a9a793de-2231-4676-9145-8815c0b1d5ab" />

A circuit is represented as a directed graph G(V, E) where:

Vertices (V): Combinational logic blocks with delay d(v)

Edges (E): Connections with w(e) registers

Retiming: A function r: V → Z that assigns an integer lag to each vertex

New register count: wᵣ(e) = w(e) + r(v) - r(u) for edge u→v

Constraints:
Non-negativity: wᵣ(e) ≥ 0 for all edges

Clock period: For all paths u→v with delay D, wᵣ(path) ≥ 1 if D > T (target clock period).


## LAB: COMBINATIONAL LOGIC OPTIMIZATION

**1. opt_check.v**
```bash
#Open the code
gvim opt_check.v
```
<img width="1261" height="811" alt="Screenshot 2025-09-25 123908" src="https://github.com/user-attachments/assets/18159fa2-b189-4095-a0c6-eee56bfd5b42" />

**Synthesis & optimization**
```bash

#open the yosys
yosys

#Read the liberty file
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#Read the verilog file
read_verilog opt_check.v

#for synthesis
synth -top opt_check
```
<img width="1201" height="305" alt="Screenshot 2025-09-25 124846" src="https://github.com/user-attachments/assets/1ece6bf3-760b-417b-8d18-2c5370882ba1" />

## 🔎 Yosys `opt_check` Statistics

| Parameter                  | Value | Explanation                                                                 |
|-----------------------------|-------|-----------------------------------------------------------------------------|
| **Number of wires**        | 3     | Total signal connections in the design (inputs, outputs, internal nets).    |
| **Number of wire bits**    | 3     | Total width of all wires (e.g., 3 single-bit wires, or 1×3 bus).            |
| **Number of public wires** | 3     | Public = visible wires (top-level ports or preserved after optimization).   |
| **Number of public wire bits** | 3 | Total bit-width of public wires = 3 bits.                                   |
| **Number of memories**     | 0     | No memory arrays (RAM/ROM) in the design.                                   |
| **Number of memory bits**  | 0     | Since no memories, there are no memory bits.                                |
| **Number of processes**    | 0     | No behavioral blocks (`always` with if/else or case); flattened to gates.   |
| **Number of cells**        | 1     | Only one logic cell (gate) remains after optimization.                      |
| **$_AND_**                 | 1     | The remaining cell is a 1-bit AND gate (`$_AND_` is Yosys’s primitive).     |

✅ Final simplified logic:  
\[
Y = A &  B
\]


```bash

#for optimization
opt_clean -purge

#run the ABC tool
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#see the hierarchy
show 
```
<img width="1282" height="806" alt="Screenshot 2025-09-25 124818" src="https://github.com/user-attachments/assets/d8759919-95ab-46ff-b516-beb6d6b0ece3" />
## 🔎 Yosys Netlist Visualization (`opt_check`)

| Symbol / Block                          | Meaning                                                                 |
|-----------------------------------------|-------------------------------------------------------------------------|
| **a, b**                                | Primary inputs (declared in Verilog).                                   |
| **BUF (input side)**                    | Buffer cells automatically inserted by synthesis for signal fanout.     |
| **sky130_fd_sc_hd__and2_0**             | 2-input AND gate from the SkyWater130 standard cell library.            |
| **Inputs A, B**                         | Gate inputs connected to signals `a` and `b`.                           |
| **Output X**                            | Gate output net, internal signal before driving `y`.                    |
| **BUF (output side)**                   | Output buffer cell before driving the primary output.                   |
| **y**                                   | Primary output of the design.                                           |
| **$84**                                 | Internal unique instance name for the AND gate generated by Yosys.      |
| **opt_check (label)**                   | Indicates this netlist was dumped after the `opt_check` optimization.   |

**2.opt_check2.v**

```bash
#Open the code
gvim opt_check2.v
```
<img width="1277" height="812" alt="Screenshot 2025-09-25 124921" src="https://github.com/user-attachments/assets/172f3e93-855e-47b1-b565-f9245a93e63b" />


**Synthesis & optimization**
```bash

#open the yosys
yosys

#Read the liberty file
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#Read the verilog file
read_verilog opt_check2.v

#for synthesis
synth -top opt_check2
```
<img width="1214" height="268" alt="Screenshot 2025-09-25 125029" src="https://github.com/user-attachments/assets/30f9c7f7-d02a-4eab-8c5c-9d97662f3bb4" />

## 🔎 Yosys `opt_check2` Statistics

| Parameter                  | Value | Explanation                                                                 |
|-----------------------------|-------|-----------------------------------------------------------------------------|
| **Number of wires**        | 3     | Total number of signal connections in the design.                           |
| **Number of wire bits**    | 3     | Total width of all wires = 3 bits (e.g., 3 single-bit wires).               |
| **Number of public wires** | 3     | All wires are public → visible at the top level (inputs/outputs).           |
| **Number of public wire bits** | 3 | Total bit-width of public wires.                                            |
| **Number of memories**     | 0     | No memory blocks (like RAM/ROM).                                            |
| **Number of memory bits**  | 0     | Since no memories exist, memory bits = 0.                                   |
| **Number of processes**    | 0     | No behavioral processes (`always`, `if`, `case`); fully mapped to gates.    |
| **Number of cells**        | 1     | Only one logic cell (primitive gate) remains after optimization.            |
| **$_OR_**                  | 1     | The remaining cell is a 1-bit OR gate (`$_OR_` is Yosys’s internal cell).   |

```bash

#for optimization
opt_clean -purge

#run the ABC tool
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#see the hierarchy
show 
```
<img width="1285" height="803" alt="Screenshot 2025-09-25 125304" src="https://github.com/user-attachments/assets/954e86ff-469e-4911-95ef-74833243df45" />


| Component | Type | Description | Inputs | Output | Function |
|-----------|------|-------------|--------|---------|----------|
| `a` | Input Signal | Primary input signal A | - | - | Raw input signal A |
| `BUF` (first) | Buffer | Input buffer for signal A | `a` | `A` | Isolates and strengthens input A |
| `b` | Input Signal | Primary input signal B | - | - | Raw input signal B |
| `BUF` (second) | Buffer | Input buffer for signal B | `b` | `B` | Isolates and strengthens input B |
| `A` | Buffered Signal | Output from first buffer | `a` (via BUF) | - | Clean, buffered version of input A |
| `B` | Buffered Signal | Output from second buffer | `b` (via BUF) | - | Clean, buffered version of input B |
| `$84` | Instance Name | OR gate reference | - | - | Unique component identifier |
| `sky130_fd_sc_hd_or2_0` | OR Gate | 2-input OR gate | `A`, `B` | `X` | Logical OR: X = A + B |
| `X` | Intermediate Signal | OR gate output | `A`, `B` | - | Result of A OR B operation |
| `BUF` (third) | Buffer | Output buffer | `X` | `opt_check2` | Drives final output |
| `opt_check2` | Output Signal | Final output | `X` (via BUF) | - | Buffered OR operation result |


**3. opt_check3.v**
```bash
#Open the code
gvim opt_check3.v
```
<img width="1278" height="807" alt="Screenshot 2025-09-25 125340" src="https://github.com/user-attachments/assets/b198a6f5-2c54-4d97-ae04-932cfb2ff54a" />

**Synthesis & optimization**
```bash

#open the yosys
yosys

#Read the liberty file
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#Read the verilog file
read_verilog opt_check3.v

#for synthesis
synth -top opt_check3
```
<img width="1222" height="293" alt="Screenshot 2025-09-25 125424" src="https://github.com/user-attachments/assets/1fe57380-877e-4676-9468-b9705f912092" />


| Metric | Value | Explanation |
|--------|-------|-------------|
| **Number of wires** | 5 | Total distinct electrical nets/nodes |
| **Number of wire bits** | 5 | Total signal connections (no buses) |
| **Number of public wires** | 4 | I/O accessible wires at module boundaries |
| **Number of public wire bits** | 4 | Individual I/O signals (all single-bit) |
| **Number of memories** | 0 | No RAM/ROM memory blocks |
| **Number of memory bits** | 0 | Zero memory storage elements |
| **Number of processes** | 0 | No sequential/clocked elements |
| **Number of cells** | 2 | Total logic gates in design |
| **`$_ANDNOT_`** | 1 | AND-NOT gate (A AND NOT B) |
| **`$_NAND_`** | 1 | NAND gate (NOT AND) |



```bash

#for optimization
opt_clean -purge

#run the ABC tool
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#see the hierarchy
show 
```
<img width="1287" height="801" alt="Screenshot 2025-09-25 125508" src="https://github.com/user-attachments/assets/ba61388b-a70d-427f-8c66-5d5c94cf8db2" />

**4.opt_check4.v**

```bash
#Open the code
gvim opt_check4.v
```
<img width="1275" height="802" alt="Screenshot 2025-09-25 125557" src="https://github.com/user-attachments/assets/92aac4c3-6d8a-44a3-a1db-fdb5163171b5" />


**Synthesis & optimization**
```bash

#open the yosys
yosys

#Read the liberty file
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#Read the verilog file
read_verilog opt_check4.v

#for synthesis
synth -top opt_check4
```
<img width="1206" height="312" alt="Screenshot 2025-09-25 125650" src="https://github.com/user-attachments/assets/96f1d6c3-ada3-431e-866f-d9645710f003" />


```bash

#for optimization
opt_clean -purge

#run the ABC tool
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#see the hierarchy
show 
```
<img width="1288" height="798" alt="Screenshot 2025-09-25 125742" src="https://github.com/user-attachments/assets/c4991a95-1565-4ac6-8124-9f8744ca7138" />


**5. multiple_module_opt.v**

```bash
#Open the code
gvim multiple_module_opt.v
```

<img width="1284" height="807" alt="Screenshot 2025-09-25 125910" src="https://github.com/user-attachments/assets/344085f6-04f1-4a70-a4bd-04540be5f60c" />


**Synthesis & optimization**
```bash

#open the yosys
yosys

#Read the liberty file
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#Read the verilog file
read_verilog multiple_module_opt.v

#for synthesis
synth -top multiple_mopdule_opt
```

<img width="1218" height="610" alt="Screenshot 2025-09-25 130021" src="https://github.com/user-attachments/assets/1f80aee0-80a4-4435-8b5a-30392ca1e3ef" />

<img width="1217" height="359" alt="Screenshot 2025-09-25 130029" src="https://github.com/user-attachments/assets/195cd56d-e6ec-49b2-b7a0-31cf78a5e9ef" />


```bash
#for flatten
flatten

#for optimization
opt_clean -purge

#run the ABC tool
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#see the hierarchy
show 
```

<img width="1285" height="806" alt="Screenshot 2025-09-25 131234" src="https://github.com/user-attachments/assets/f9db36d5-80c8-428a-96c8-28c55ecf678d" />

**5. multiple_module_opt2.v**

```bash
#Open the code
gvim multiple_module_opt2.v
```

<img width="1283" height="803" alt="Screenshot 2025-09-25 130308" src="https://github.com/user-attachments/assets/ef546297-09db-46a9-87fa-bc395b689161" />


**Synthesis & optimization**
```bash

#open the yosys
yosys

#Read the liberty file
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#Read the verilog file
read_verilog multiple_module_opt2.v

#for synthesis
synth -top multiple_module_opt2
```

<img width="1286" height="669" alt="Screenshot 2025-09-25 130459" src="https://github.com/user-attachments/assets/a2c5a107-1eee-4020-88d6-fb2501d4f7e6" />
<img width="1273" height="352" alt="Screenshot 2025-09-25 130507" src="https://github.com/user-attachments/assets/9d318143-9259-43ff-ac2e-ef4e479e1ee0" />


```bash
#for flatten
flatten

#for optimization
opt_clean -purge

#run the ABC tool
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#see the hierarchy
show 
```

<img width="1270" height="809" alt="Screenshot 2025-09-25 131037" src="https://github.com/user-attachments/assets/8ebb3310-94ed-4fdd-9d34-fd0635be43dd" />

















