# Week 0 - Day 1


<details>
	<summary>Day 0 - Getting started with VLSI SOC DESIGN and PLANNING </summary>

# Day 1 - Getting started with VLSI SOC DESIGN and PLANNING

An SoC is a single chip that integrates multiple components of a complete electronic system (CPU, memory, I/O, communication blocks, etc.).
Instead of having separate chips for processor, memory, and peripherals → everything is put on one silicon die.
In VLSI (Very-Large-Scale Integration), SoC design is one of the most advanced applications — because it combines digital, analog, memory, RF, and sometimes sensors into one chip.

<img width="1712" height="217" alt="Screenshot 2025-09-18 233255" src="https://github.com/user-attachments/assets/8e22b9a8-0b96-4523-a3da-cb645ba6ed2c" />

The Specification and High-Level Model (C Model)
O1 —— Specs (C model): This is the starting point.

Specs: These are the detailed requirements for the chip. They define what the chip must do (its function, performance, power consumption, etc.), but not how it will be implemented in hardware.

C model: To validate the specifications, engineers first create a software model of the chip's functionality written in the C programming language. This is a high-level, behavioral model that is fast to simulate and easy to modify. It acts as the "golden reference" — a perfect software representation of what the chip should do.

Using RTL (Verilog): The architect implements the design using a Hardware Description Language (HDL), specifically Verilog, at the Register-Transfer Level (RTL)

Testbench is in C language: This is the most crucial part of the diagram for verification. A testbench is a setup that applies stimuli (test inputs) to a design and checks its outputs against expected results.

The same C testbench that was used to verify the C model (O1) is now used to verify the RTL model (O2).

Soft copy of the Hardware: This refers to the final, verified RTL code (the Verilog files). This is called a "soft copy" because it is the digital design files that fully describe the hardware. This RTL code is then sent through subsequent automated steps in the chip design flow (like Synthesis and Place & Route) to be turned into a physical "hard" chip.

<img width="1012" height="413" alt="Screenshot 2025-09-18 233313" src="https://github.com/user-attachments/assets/9aae355a-bde0-4fe4-b2d1-83efbe6cb606" />


This diagram illustrates the physical implementation phase of chip design. It starts with the verified RTL code (the "soft copy" from the previous diagram) and shows the key components and steps involved in converting that abstract description into a detailed, technology-specific circuit model (a netlist).

The diagram shows that the RTL code is organized into the main components of the System-on-a-Chip (SoC):

Processor: The central processing unit (CPU) core(s).

Peripherals/IPs: The surrounding functional blocks, such as memory controllers, USB interfaces, graphics processors (GPUs), etc.

**The Outputs: The Results of Synthesis -->**
This is the core of the image. The term "Synthesis" is the process of using automated Electronic Design Automation (EDA) tools to translate the high-level RTL code into a list of specific logic gates and their interconnections (a netlist), based on a chosen semiconductor technology library (e.g., a 3nm Samsung process).

**Gate Level Netlist (synth P&R)**

 A detailed netlist for the digital logic that was described in the main RTL. It consists of standard cells (basic logic gates like AND, OR, flip-flops) from the technology library.

(synth P&R) means this netlist is the input for the next major stage: Place and Route (P&R), where the gates are physically arranged on the silicon die and the wires connecting them are drawn.

**Macros (synth RTL)**

 "Macros" typically refer to large, pre-designed blocks with a fixed layout, such as Memory (SRAM, DRAM) or Processor Cores.

(synth RTL) indicates that these macros are synthesized from their own RTL descriptions separately. They are not broken down into standard cells like the main logic. They are generated or imported as complete, black-box units with a fixed physical interface and layout.

**Analog IPs (func RTL)**
 These are blocks that interface with the real, continuous world, such as Phase-Locked Loops (PLLs), Analog-to-Digital Converters (ADCs), or USB PHYs.

(func RTL) is crucial. It means that for the digital part of the chip to be simulated and verified, these analog blocks are represented by a Functional RTL model. 

<img width="1541" height="553" alt="Screenshot 2025-09-18 233515" src="https://github.com/user-attachments/assets/b2fbaf98-2e26-4943-a08d-c04a68d2c499" />

The diagram visualizes the Physical Design process. Its goal is to convert the logical description of the chip (the Gate-Level Netlist) into a precise, physical layout (a GDSII file) that defines exactly where every transistor, gate, and wire will be placed on the silicon die, ensuring it meets timing, power, and area constraints and is free of manufacturing rule violations.

**SoC integration** : The process of connecting all these components—the main digital logic, the macros, and the analog IPs—into a single, complete system. GPIOs (General-Purpose Input/Output) are the physical pins that connect this integrated system to the outside world.

**The Core Process: RTL2GDS**
RTL2GDS: This is the name for the entire automated flow, managed by EDA tools, that turns Register-Transfer Level code into a GDSII file. The key steps within this flow are:

Synthesis: (Shown here again for context, though it's technically the previous step). Converts RTL to the gate-level netlist.

Floorplanning: The first step of physical design. The chip's floor plan is created: the overall size and shape of the chip are defined, and the major blocks (especially the hard macros) are placed. I/O pins and power delivery networks are also planned here.

Placement: The exact location of every standard cell (from the gate-level netlist) is determined on the silicon die. The goal is to minimize the total length of connections while meeting timing requirements.

CTS (Clock Tree Synthesis): A critical step where a dedicated network is built to distribute the clock signal from a single source to all sequential elements (flip-flops) across the chip with minimal skew (delay differences). This ensures all parts of the chip operate in sync.

Routing: The process of adding the metal wires that connect all the placed components (standard cells, macros, I/Os) according to the netlist. This is like wiring a very complex, microscopic city.


**The Libraries and IP Types**
The physical design tools need libraries to know how to build the chip:

Macros and analog IP libraries: These contain the physical and timing information for the hard macros and analog IPs.

Hardened (hard macro - HM): A Hard Macro is a pre-designed, pre-verified block with a fixed, optimized physical layout. 

**The Final Output and Verification**
GDSII: This is the final output. It is a industry-standard database file format that contains the complete geometric information of the entire chip's layout—every polygon, wire, and component. This file is sent to the semiconductor foundry (e.g., TSMC, Samsung) to create the photomasks used in manufacturing.

DRC/LVS checks: The final, critical verification steps before tape-out (sending to the fab).

DRC (Design Rule Check): Ensures the physical layout adheres to all the manufacturing rules of the chosen process technology (e.g., minimum spacing between wires, minimum width of a transistor). It checks if the design is manufacturable.

LVS (Layout vs. Schematic): Checks that the physical layout (GDSII) is logically equivalent to the original circuit diagram (Gate-Level Netlist). It verifies that the layout matches the design.


<img width="1441" height="434" alt="Screenshot 2025-09-18 233740" src="https://github.com/user-attachments/assets/917f5592-01ac-4482-ac33-3971184de86c" />

The image illustrates the principle of Functional Equivalence Checking throughout the entire chip design flow.

The equation **O1 = O2 = O3 = O4** is the ultimate goal of the entire design process. It means that the physical chip you manufacture will behave exactly as the original C code intended.

O1 == O2: Verified by simulation using the C Testbench (as shown in the first diagram).

O2 == O3: Verified by a process called Formal Equivalence Checking. Tools mathematically prove that the synthesized gate-level netlist is functionally identical to the original RTL code, without needing test vectors.

O3 == O4: Verified again by Formal Equivalence Checking after place and route. This step is critical because the physical implementation (layout) can introduce issues like clock skew or unexpected electrical effects that could change the logical behavior. This check ensures that the final layout is still logically equivalent to the netlist it was built from.


