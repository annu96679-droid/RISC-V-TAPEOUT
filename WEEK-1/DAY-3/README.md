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


```bash

#for optimization
opt_clean -purge

#run the ABC tool
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#see the hierarchy
show 
```
<img width="1282" height="806" alt="Screenshot 2025-09-25 124818" src="https://github.com/user-attachments/assets/d8759919-95ab-46ff-b516-beb6d6b0ece3" />







