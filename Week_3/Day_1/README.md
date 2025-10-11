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
<img width="1295" height="803" alt="Screenshot 2025-10-06 165257" src="https://github.com/user-attachments/assets/c935169a-047b-4c9a-b704-7cac74776e1e" />

```bash
#Load the Liberty Files for Synthesis
read_verilog /home/anuj/VSDBabySoC/src/module/vsdbabysoc.v
read_verilog -I /home/anuj/rvmyth /home/anuj/VSDBabySoC/output/complied_tlv/rvmyth.v
read_verilog -I /home/anuj/rvmyth /home/anuj/VSDBabySoc/src/module/clk_gate.v
```
<img width="1287" height="802" alt="Screenshot 2025-10-06 165708" src="https://github.com/user-attachments/assets/4adb36ee-4a13-437c-aaf9-e8b22c1da8a3" />
<img width="1280" height="801" alt="Screenshot 2025-10-06 165715" src="https://github.com/user-attachments/assets/e24b2564-f6c0-43cb-9bde-091bdf2ac2a2" />

```bash
#Run Synthesis Targeting vsdbabysoc
synth -top vsdbabysoc
```
<img width="1239" height="800" alt="Screenshot 2025-10-06 174935" src="https://github.com/user-attachments/assets/23d9321a-3ffc-40d5-840b-1bda7da80172" />
<img width="1285" height="807" alt="Screenshot 2025-10-06 175022" src="https://github.com/user-attachments/assets/960c61d1-154f-49a3-b1df-e3c0e41dcf4f" />
<img width="1191" height="565" alt="Screenshot 2025-10-06 175042" src="https://github.com/user-attachments/assets/27c305bc-2e26-4591-bcfe-ee20a938fc91" />

<img width="1283" height="389" alt="Screenshot 2025-10-06 175053" src="https://github.com/user-attachments/assets/869234c5-01d5-421f-85ed-b782601bb507" />
<img width="1280" height="805" alt="Screenshot 2025-10-06 175104" src="https://github.com/user-attachments/assets/ffe79da8-37c2-4e1e-9ae3-dcfc4aa0ee70" />


