# Week 1 - Day 1 : Introduction to Verilog RTL Design and Synthesis

## Introduction to open source simulator iverilog
### 2-SKY130RTL D1SK1 L1 Introduction to iverilog design test bench

<img width="1644" height="778" alt="Screenshot 2025-09-21 221446" src="https://github.com/user-attachments/assets/3764f211-c655-472b-a054-98ae8887068d" />

This image is a fundamental block diagram of a Test Bench used in digital design verification, particularly with Hardware Description Languages (HDLs) like VHDL and Verilog.

**What is a Test Bench?**
A test bench is a virtual environment used to test and verify the correctness of a digital logic design (like a chip or a circuit) before it is physically manufactured. It's essentially a harness that surrounds your design, applying controlled inputs and checking if the outputs are as expected.

**Breakdown of each components:**

***1. Design (or DUT - Design Under Test)***
This is the central component, the actual digital circuit you have designed and want to test. It could be anything from a simple AND gate to a complex microprocessor.

What it does: It has inputs (like clk, reset, data_in) and based on its internal logic, it produces outputs (like data_out, ready, status).

Its Role in the Test Bench: It is a passive component. It simply reacts to the stimuli provided to it. It doesn't know it's being tested.

***2. Stimulus Generator (Test Vector Generator)***
This block is the "brain" of the test bench. It's responsible for creating all the input signals that will be fed into the Design.

What it does: It generates sequences of signals that mimic how the real world would interact with your design. This includes:

Generating a clock signal (clk).

Asserting and de-asserting a reset signal at specific times.

Providing valid and invalid data inputs (data_in) to test all possible scenarios, including edge cases.

Its Role: It is an active component. It drives the DUT.

***3. Stimulus Observer (Response Monitor)***
This block watches the outputs of the Design and often its internal states (if possible).

What it does: It continuously monitors the outputs of the DUT. Its job is to capture the design's response to the stimuli. It might simply record the values to a log file, or it might do much more...

Its Role: It is a passive component. It watches and records but does not interfere with the DUT's operation.

(Often, the Observer is combined with a "Checker" or "Scoreboard")
A basic observer just records data. A more advanced test bench will have a Checker that compares the observed outputs to the expected outputs. If they don't match, it will flag an error (e.g., print "ERROR: Test Failed at time 105ns"). This automation is crucial for testing thousands of scenarios.

***4. Primary Inputs & Outputs***
These are the connection points.

Primary Inputs to Design: These are the wires connecting the Stimulus Generator to the DUT. They carry the test signals (clk, reset, data_in).

Primary Outputs from Design: These are the wires connecting the DUT's outputs to the Stimulus Observer. They carry the results (data_out, ready).

## iverilog based simulation flow

<img width="1822" height="832" alt="Screenshot 2025-09-21 221500" src="https://github.com/user-attachments/assets/372e2923-bd0b-499e-bc17-1945488ca4ba" />

This image describes a specific toolchain and workflow used for simulating and verifying digital circuits.

***1. DESIGN & TEST BENCH***
This is the starting point. You create two separate files.

Design (The Circuit): A hardware description language (Verilog/VHDL) file that describes the digital circuit you want to test. This is the "Device Under Test" (DUT).

Test Bench (The Tester): Another HDL file that acts as the harness for your design. It instantiates the DUT, applies input stimuli (test vectors), and monitors its outputs. It also contains the commands to dump the signal changes to a .vcd file.

***2. IVERILOG (The Compiler & Simulator)***
Icarus Verilog is an open-source Verilog simulation and synthesis tool. It plays two key roles:

Compiler: It reads your Design and Test Bench code, checks them for syntax errors, and translates them into an intermediate format.

Simulator: It then executes this intermediate format. As it runs, it simulates the passage of time and models how the signals in your design change based on the stimuli from the test bench.

***3. VALUE CHANGE DUMP (.vcd file) (The Result Log)***
The VCD file is a standardized ASCII text format. During the simulation, the test bench instructs the simulator to record every change (a "value change") of the specified signals (e.g., clk, reset, a, b, y) along with the simulation time when the change occurred.

It's essentially a detailed log file of everything that happened during the test.

It is not human-readable in a practical sense, which is why we need the next tool.

***4. GTKWAVE (The Waveform Viewer)***
GTKWave is an open-source waveform viewer. It takes the .vcd file and displays its contents as a professional-looking timing diagram (waveform). This visual representation allows you to easily see:

The relationship between your input stimuli and output responses.

Propagation delays (if modeled).

Whether the circuit behaved as expected at every point in time. It is the primary tool for debugging

## LAB USING IVERILOG AND GTKWAVE



1.System Update & Package Installation : 

```bash
# Update package list
sudo apt-get update

# Install Git
sudo apt-get install git -y

# Check current folders in Desktop
ls

# Move to /home directory
cd /home
ls

# Move to your 'vsd' user folder
cd vsd
ls

# Create VLSI folder
mkdir VLSI

# Move into VLSI folder
cd VLSI
ls

# Create vsdflow folder
mkdir vsdflow

# Move into vsdflow folder
cd vsdflow

# cloning the git
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git

# Move into git clone folder
cd sky130RTLDesignAndSynthesisWorkshop
ls

# Move into my_lib folder
cd my_lib
ls

# Returning to sky130RTLDesignAndSynthesisWorkshop folder
cd ..

# Move into verilog_files folder
cd verilog_files/
ls

# Return to VLSI folder
cd ..
cd ..

# Move to vsdflow folder
cd vsdflow/
ls

```

some images of package installation:

<img width="1149" height="815" alt="Screenshot 2025-09-22 113007" src="https://github.com/user-attachments/assets/e0fd2ed8-4113-452f-bc51-8280839f580e" />

<img width="1144" height="820" alt="Screenshot 2025-09-22 115035" src="https://github.com/user-attachments/assets/29058aef-5f26-40af-adc5-cae3ae521750" />


<img width="1145" height="806" alt="Screenshot 2025-09-22 115044" src="https://github.com/user-attachments/assets/c51245e3-bdc9-43d2-844a-f9b178fb3586" />

## Module, Testbench and Simulation using IVERILOG and GTKWAVE

**from verilog_files/**

```bash
# Move into the verilog_files folder
cd verilog_files/

# download gvim tool to have the modules and testbench
sudo apt-get install vim-gtk3

# Give command to edit [ex - good_mux]
gvim tb_good_mux.v good_mux.v

# check current folder in destop
ls

# Command to generate dumpfile
./a.out

# Simulation(waveform)
gtkwave tb_good_mux.vcd
```
**Module (good_mux)**

```bash

module good_mux (input i0 , input i1 , input sel , output reg y);     //Module Declaration
    always @ (*)       //Always Block
    begin               
        if(sel)        //combinational logic
            y = i1;  // When sel=1, output i1
        else
            y = i0;  // When sel=0, output i0
    end
endmodule

```

<img width="1184" height="806" alt="Screenshot 2025-09-22 121326" src="https://github.com/user-attachments/assets/36f59b78-0373-429f-b915-e789f1be2c14" />

**Testbench (tb_good_mux)

```bash
module tb_good_mux;
   reg i0, i1, sel;
   wire y;

//Port mapping: Connects testbench signals to DUT ports
   good_mux uut (
       .sel(sel),
       .i0(i0),
       .i1(i1),   // corrected from .i1(i1)
       .y(y)
   );

   initial begin
       $dumpfile("tb_good_mux.vcd");
       $dumpvars(0, tb_good_mux);
       // Initialize inputs
       sel = 0;
       i0 = 0;
       i1 = 0;
       // Add stimuli to change i0 and i1
       #100;
       i0 = 1;
       #100;
       i1 = 1;
       #100;
       i0 = 0;
       #100;
       $finish;
   end

   always #50 sel = ~sel;
endmodule

```

<img width="994" height="561" alt="Screenshot 2025-09-22 121107" src="https://github.com/user-attachments/assets/7abe1d1c-1a05-4494-84ca-10a3566ea315" />

**Simluation (Waveform)**

Simulation is the process of testing your Verilog code before putting it on hardware (like FPGA).

A waveform is the graphical view of signals during simulation.


<img width="1152" height="809" alt="Screenshot 2025-09-22 120017" src="https://github.com/user-attachments/assets/085ce407-9567-419b-8681-68c00fbdf83c" />

**Multiplexer Operation Verification:**
When sel = 0:

Output y should equal i0

The waveform shows y matches i0 during these periods

When sel = 1:

Output y should equal i1

The waveform shows y matches i1 during these periods

## Introduction to YOSYS and Logical synthesis 


Yosys reads the RTL and library, performs parsing, elaboration and multiple synthesis/optimization passes, and performs technology mapping to real cells.

Synthesis is the process of converting RTL code (written in Verilog/VHDL/SystemVerilog) into a gate-level netlist using logic gates (NAND, NOR, flip-flops, etc.) from a standard cell library.

<img width="1151" height="619" alt="Screenshot 2025-09-23 011222" src="https://github.com/user-attachments/assets/8a01721b-1b59-4515-8bf9-5de3808af7f3" />

The image is divided into two main sections, separated by a horizontal line. The top section is labeled "Yosys setup" and describes the fundamental input/output operations. The bottom section is labeled "DESIGN" and places the Yosys process within the broader context of a typical digital design flow.

**Section 1: Yosys Setup**
This section explains the core commands to run a simple synthesis job in Yosys.

read_verilog: This represents the first step. Yosys is instructed to read a source file written in Verilog (a Hardware Description Language - HDL). This Verilog file describes the desired digital circuit (e.g., a processor, a filter, a state machine) at a Register-Transfer Level (RTL) of abstraction. It uses constructs like always blocks, if statements, and assign statements.

write_verilog: This represents the final step in this basic flow. After processing the RTL, Yosys writes out a new Verilog file. However, this output is not the same as the input. It is a structural netlist. This means the high-level RTL code has been transformed into a list of basic logic gates (AND, OR, NOT, flip-flops, etc.) and the connections (wires) between them. This netlist is a much lower-level representation of the circuit, ready for the next stages of physical design.

In summary, the top half shows: RTL Verilog Input -> (Yosys Synthesis Process) -> Gate-level Netlist Output.

**Section 2: DESIGN**

***.lib / read_liberty:*** This is a crucial part of realistic synthesis. The .lib (Liberty) file is a database that contains detailed timing and power information about the specific standard cell library you are using (e.g., a 7nm or 28nm cell library from a foundry like TSMC or GlobalFoundries).

The command read_liberty is used in Yosys to load this .lib file.

Why is this important? It allows Yosys to perform technology mapping. Instead of just generating a netlist of generic gates, Yosys can map the design to the actual, physical cells (like a specific type of AND gate with known delay and area) available in the target technology. This enables optimization for speed (timing) or area.

yosys: This label points to the central box in the diagram, which represents the Yosys synthesis engine itself. This is where all the magic happens: the RTL code is elaborated, optimized, and mapped to the target cell library.

***netlist file:*** This is the final output of the process, generated by the write_verilog command (or similar commands like write_json). This file is the deliverable from the synthesis step and is used as the input for the next stages in the chip design flow, such as Place and Route (P&R). The P&R tools will take this netlist and decide the physical placement of each gate on the silicon die and route the wires between them.

***After this there is a process of verification of synthesis***

**Synthesis (ILLUSTRATION)**

<img width="888" height="652" alt="Screenshot 2025-09-23 011616" src="https://github.com/user-attachments/assets/0271d495-a759-42a2-a9e9-3d66fc1bab17" />

It shows how a behavioral description of a circuit (written in Verilog) is automatically transformed by a synthesis tool into an interconnected list of standard logic gates (a netlist), which can then be physically manufactured.

**Section 1: The Input (Register-Transfer Level - RTL Code)**

Module declaration
Functionality
The flip-flop with reset

the code describes a circuit that selects one of two inputs (A or B) and stores that selected value in a flip-flop on each clock cycle, with the ability to reset the stored value to zero.

**Section 2: The Output (Synthesized Gate-Level Netlist)**

***The Multiplexer (MUX):*** The tool has implemented the assign int = sel ? A : B statement using basic gates:

It uses two AND gates, one OR gate, and one NOT (inverter) gate.

***The Flip-Flop (FF):*** The always block has been mapped to a standard cell called a D-Type Flip-Flop (DFF) with asynchronous reset.


## Generating netlist and synthesis using YOSYS and Sky130 PDKs

**following commands are:**

```bash

yosys -p "

**Load the standard cell library**

read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

**Read the verilog RTL files**

read_verilog good_mux.v

**Synthesis with good_mux as top module**

synth -top good_mux

**Show the synthesized circuit graph**

show

**Write the full netlist with attributes**

 write_verilog good_mux_netlist.v

**Write netlist without extra attributes

write_verilog -noattr good_mux.v

"

**Open the synthesized netlist in gvim**

gvim good_mux_netlist.v

```

**Enter into the YOSYS**

<img width="1146" height="803" alt="Screenshot 2025-09-23 120228" src="https://github.com/user-attachments/assets/dd5f9164-de90-48a5-94db-5be1415af516" />

**Synthesis part**


<img width="523" height="152" alt="Screenshot 2025-09-23 120251" src="https://github.com/user-attachments/assets/530b5549-8813-45f8-928d-71e492a7d2a3" />


***3.22.1.2. Re-integrating ABC results.***

What it is: This is a status message from the main Electronic Design Automation (EDA) tool you are using (like Cadence Innovus, Synopsys DC, or an open-source tool like Yosys+Odin).

What it means: The tool has called the ABC program to perform a specific task (e.g., optimize a piece of logic). ABC has finished its work and generated a result. Now, the main tool is taking that result and merging it back into the overall design database. This is a critical step in the automated design process.

***2. ABC RESULTS: MUX cells: 1***

What it is: This is a report from ABC showing what basic logic cells (components) it used to build the circuit.

What it means: ABC determined that the entire function it was asked to implement can be perfectly built using just one Multiplexer (MUX) cell. A multiplexer is a fundamental component that selects one of several input signals and forwards it to a single output line. This is a sign of significant optimization, as more complex logic can often be reduced to a MUX.

***3. ABC RESULTS: internal signals: 0***

What it is: A count of the internal wiring needed to connect the cells ABC used.

What it means: Since there is only one cell (the MUX), there are no internal signals needed to connect multiple cells together. All connections are directly to the inputs and output.

***4. ABC RESULTS: input signals: 3***

What it is: The number of input ports to the circuit block that ABC synthesized.

What it means: This multiplexer requires 3 input signals. A typical 2-to-1 MUX has 3 inputs: two data lines (let's call them A and B) and one select line (S) to choose between them.

***5. ABC RESULTS: output signals: 1***

What it is: The number of output ports from the circuit block.

What it means: There is 1 output signal. This is the standard for a multiplexer; it outputs the selected data value (either A or B).

***6. Removing temp directory.***

What it is: A housekeeping message from the main tool.

What it means: The temporary files and folders created specifically for the ABC run are being deleted to clean up the workspace, as they are no longer needed now that the results have been re-integrated.

**Generative Netlist**

<img width="1260" height="809" alt="Screenshot 2025-09-23 121725" src="https://github.com/user-attachments/assets/6cd38f1c-96ba-4ef5-97bb-88404ff8978a" />

***From Logic Synthesis to Gate-Level Implementation***
This two-step process shows the complete transformation of a digital circuit from a high-level description to a physical implementation:

Step 1: Logic Synthesis (ABC Results)
Tool: ABC logic synthesis tool

Result: Optimized the circuit to a single 2-to-1 multiplexer

Key Metrics:

3 input signals

1 output signal

0 internal signals (highly optimized)

1 MUX cell

Status: Successfully re-integrated into design database

Step 2: Technology Mapping (Yosys Netlist)
Tool: Yosys synthesis with SkyWater 130nm PDK

Implementation: Shows the actual gate-level structure:

Input stage: 3 buffer cells for signal conditioning

Core logic: sky130_fd_sc_hd_mux2_1 standard cell (pre-optimized multiplexer)

Output stage: 1 buffer for driving capability

Module name: good_mux (indicating successful optimization)


| Step | Tool | Key Metrics | Implementation Details | Status / Module Name |
|------|------|-------------|------------------------|---------------------|
| **1. Logic Synthesis** | ABC logic synthesis tool | • 3 input signals<br>• 1 output signal<br>• 0 internal signals<br>• 1 MUX cell | Optimized circuit to single 2-to-1 multiplexer | Successfully re-integrated into design database |
| **2. Technology Mapping** | Yosys synthesis with SkyWater 130nm PDK | • Input stage: 3 buffer cells<br>• Core logic: `sky130_fd_sc_hd_mux2_1` standard cell<br>• Output stage: 1 buffer cell | Shows actual gate-level structure using pre-optimized multiplexer | Module: `good_mux` |






  


































































