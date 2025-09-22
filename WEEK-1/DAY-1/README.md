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
cd vedflow/
ls

```

some images of package installation:

<img width="1149" height="815" alt="Screenshot 2025-09-22 113007" src="https://github.com/user-attachments/assets/e0fd2ed8-4113-452f-bc51-8280839f580e" />

<img width="1144" height="820" alt="Screenshot 2025-09-22 115035" src="https://github.com/user-attachments/assets/29058aef-5f26-40af-adc5-cae3ae521750" />


<img width="1145" height="806" alt="Screenshot 2025-09-22 115044" src="https://github.com/user-attachments/assets/c51245e3-bdc9-43d2-844a-f9b178fb3586" />













