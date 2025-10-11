# POST SYNTHESIS GLS
## Synthesize RISC-V and compare output with functional simulations 

## Gate Level Simulation of VSDBabySoC

Gate-Level Simulation is used to verify the functionality of a design after the synthesis process. Unlike behavioral or RTL (Register Transfer Level) simulations, which are performed at a higher level of abstraction, GLS works on the netlist generated post-synthesis. This netlist includes the actual gates and connections used to implement the design.

## Importance of Gate-Level Simulation (GLS)

GLS verifies the logical correctness, timing constraints, and reset behavior of the synthesized design. While RTL simulation checks only functional behavior based on ideal conditions, gate-level simulation includes propagation delays, setup and hold times, and real hardware timing information extracted from the synthesis results. This makes it crucial for detecting issues that might not appear during RTL simulation, such as:

* Uninitialized signals or X-propagation caused by missing resets or synthesis optimizations.

* Race conditions or glitches introduced due to real gate-level delays.

* Clock domain or timing violations that could lead to setup/hold time failures.

* Differences between pre-synthesis and post-synthesis behavior, ensuring that synthesis did not change the intended logic.

## Here is the step-by-step execution plan for running the commands manually:

```bash
#Open the yosys
yosys

#Load the Top-Level Design and Supporting Modules
read_liberty -lib /home/anuj/VSDBabySoC/src/lib/avsdpll.lib
read_liberty -lib /home/anuj/VSDBabySoC/src/lib/avsddac.lib
read_liberty -lib /home/anuj/VSDBabySoC/src/lib/sky130_fd_sc_hd_tt+025C_1v80.lib

```

