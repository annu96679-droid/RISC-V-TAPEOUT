# Week 1 - Day 4

# Introduction to Gate Level Simulation

**What is gatte level simualtion**
Gate-Level Simulation (GLS) is the process of verifying the behavior of a digital design after it has been synthesized into a netlist of logic gates.

At the RTL (Register Transfer Level), you write Verilog/VHDL code and simulate it functionally.

But synthesis tools (like Synopsys DC, Cadence Genus, Yosys, etc.) transform that RTL into a gate-level netlist, which consists of standard cells (AND, OR, NAND, flip-flops, multiplexers, etc.) from a technology library.

**Why do we need GLS?**

When we design digital circuits, the RTL simulation (behavioral/functional simulation) is our first checkpoint. It ensures that the high-level Verilog or VHDL code behaves as intended. However, synthesis tools transform this RTL into a gate-level netlist that uses logic gates and flip-flops from a technology library.
This transformation may introduce subtle changes, and RTL simulation alone cannot guarantee that the synthesized hardware will behave correctly in silicon. That’s where Gate-Level Simulation (GLS) comes in.

***1. Validation of RTL vs Synthesized Netlist***

RTL code is written in a high-level, abstract way. The synthesis tool translates it into a gate-level representation.

Certain RTL constructs (like inferred latches, case statements without default, blocking vs non-blocking assignments, etc.) may lead to unexpected hardware structures.

GLS ensures that the gate-level netlist behaves the same way as the RTL simulation under real testbench scenarios.

***2. Reset and Initialization Verification***

In RTL simulation, uninitialized signals may default to 0 or X, depending on the simulator. Designers often assume registers start in a known state.

In real silicon, flip-flops power up in unknown states until explicitly reset.

GLS models this correctly: all flip-flops start as X (unknown) unless a reset signal is applied.

***Timing-Aware Verification***

RTL simulation assumes zero delay for all operations. This hides many timing-related problems.

At the gate level, each logic gate has a propagation delay, and interconnects add more delay.

GLS can be run with SDF (Standard Delay Format) annotation, which introduces actual cell and wire delays.
