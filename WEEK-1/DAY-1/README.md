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

