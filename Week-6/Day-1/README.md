# DAY-1

# Inception of Open source EDA , OpenLANE and SKY130 PDK

## 1.Introduction to QFN-48 Package, chip, pads, core, die and IPs

* The QFN-48 (Quad Flat No-leads) package is a modern, surface-mount technology (SMT) housing for integrated circuits (ICs). The "48" denotes the number of electrical connections, or I/O pads, the package provides. Its key physical characteristic is the absence of protruding leads, unlike its predecessor, the QFP (Quad Flat Package). Instead, it features flat, copper-exposed pads on the bottom periphery of the package, which are soldered directly onto matching pads on the printed circuit board (PCB).
  
* This design offers significant advantages: a much smaller footprint, lower height, and superior thermal and electrical performance. The central, exposed pad on the bottom is a critical feature, designed to be soldered to a large copper pour on the PCB, acting as an efficient heat sink to draw thermal energy away from the silicon chip inside. This makes the QFN package exceptionally popular in space-constrained and thermally sensitive applications like smartphones, tablets, and wearable devices.

<img width="916" height="881" alt="Screenshot 2025-10-26 143804" src="https://github.com/user-attachments/assets/3b4fb943-0121-436f-beb8-dfef24a4a2e1" />
<img width="1035" height="860" alt="Screenshot 2025-10-26 144538" src="https://github.com/user-attachments/assets/98c840cc-df03-42f6-a98c-2e9465a1ea23" />


## Block diagram or pinout description of a microcontroller (MCU) or System-on-a-Chip (SoC)

**1. Dedicated High-Performance Peripheral Buses**

This section describes pins dedicated to high-speed communication, likely located on one side of the chip package.

* Direct I2C: This refers to a dedicated I²C bus controller. "Direct" means the pins for I²C (SDA and SCL) are hardwired to this controller, ensuring reliable, high-performance communication without being affected by other pin configuration issues.

* Direct QSPI: This is a dedicated Quad SPI interface. QSPI is a very fast serial bus often used to connect to external Flash memory. Having it as "direct" is critical for achieving maximum read/write speeds, which is essential for executing code from an external memory chip (XiP - Execute in Place).

* GPIO8-14: A bank of General-Purpose Input/Output pins that are likely located next to these dedicated peripherals.

* PWM4-5: Specific Pulse-Width Modulation channels tied to this pin group, useful for controlling LEDs, motors, or generating analog signals.

<img width="1543" height="863" alt="Screenshot 2025-10-26 144147" src="https://github.com/user-attachments/assets/12182504-57ae-401f-9602-12a4e4f7898c" />

**2. Second Set of Flexible Pins**

This appears to be another bank of pins, perhaps on the opposite side of the chip.

"Direct I2C, direct QSPI, GPIO0-7, PWM0-3"

* This is very similar to the first block, suggesting the chip has two separate, dedicated I2C and QSPI peripherals. This is a powerful feature, allowing you to connect to two different QSPI memory chips or communicate on two separate I²C networks simultaneously without any performance compromise.

* GPIO0-7, PWM0-3: Another bank of general-purpose and PWM pins, numbered distinctly from the first set.

**3.The Multiplexed ("Muxed") IO Bank**
This is the core of the chip's pin flexibility. It shows a pool of peripherals that can be routed to a set of pins via software configuration.

* This list (I2C1, QSPI2, UART1/2, PWM0-5) represents the internal peripheral IPs available to be connected.

* The range "GPIO0-14" represents the physical pins that can be programmed to act as any of these peripherals.

* Implication: You can decide, for example, that you want GPIO5 to be a UART transmit pin and GPIO6 to be a UART receive pin. Alternatively, you could configure GPIO5 as a PWM output instead. This gives tremendous design flexibility but requires careful planning to avoid conflicts.

**4. External Memory and Storage Interfaces**

These blocks are dedicated to connecting critical external components.

* "SDRAM" & "SDRAM chip": This indicates a dedicated memory bus (with data lines, address lines, and control signals) to connect an external Synchronous Dynamic RAM chip. This is used to expand the system's working memory (RAM), which is essential for running complex applications or graphics displays.

* "GSPI1-Flash": Likely stands for a dedicated General SPI or GP-SPI bus connected to the primary external Flash memory chip. This is where the main program code is stored.

**5. Debug, Analog, and Additional Interfaces**

These are specialized interfaces for development, analog signals, and more.

* "JTAG-UART FIDI": This is a debug and programming interface.

* JTAG is used for in-circuit debugging (stepping through code, inspecting registers).

* The "FIDI" part likely refers to FTDI, a company that makes a common USB-to-serial/JTAG bridge chip. This indicates the chip supports programming and serial communication over a USB connection, which is very common on development boards.

* "QSPI3, GPIO14-19" & "ADC (QSPI3), muxed with GPIOs":

* This shows a third QSPI interface (QSPI3) that can be used for another peripheral.

* Crucially, some of these pins are shared with the Analog-to-Digital Converter (ADC). An ADC reads analog voltages (e.g., from a sensor) and converts them to digital numbers. The note "muxed with GPIOs" means you must choose whether a specific pin in this range is used as a digital GPIO or as an analog input.

**6. Power**

* "VCC/GND": These are the pins for supplying power (VCC) and ground (GND) to the chip. A complex chip will have multiple VCC and GND pins to ensure stable, clean power delivery to different internal sections (e.g., digital core, analog circuits, I/O buffers).

## 2. RISC-V SoC design workflow

**workflow**
<img width="1102" height="620" alt="Screenshot 2025-10-26 153109" src="https://github.com/user-attachments/assets/7a367506-e4cf-4ece-8154-469e1177c4df" />


**1. RISC-V Architecture Level (Instruction Execution)**

This part of the image shows a terminal window displaying:

* A simple C program named swap.c that swaps two integers.

* The compilation and disassembly process using RISC-V GNU tools (riscv64-unknown-elf-gcc and objdump).

Here’s what’s happening technically:

* The C code (swap(a, b)) is compiled into RISC-V machine code, a set of instructions that conform to the RISC-V Instruction Set Architecture (ISA).

* The objdump command disassembles the binary back into assembly instructions (like addi, sd, ld, jalr, etc.).

* This represents the architecture layer — the abstract definition of what the CPU understands and executes.

* The RISC-V ISA defines how operations like add, load, store, branch, etc., are encoded and executed.

So, in this step:

* The software (C program) is converted to machine instructions that can be run on a RISC-V processor.

* The disassembly output shows that the instructions are now ready for a hardware implementation to execute them.

➡️ Purpose: To illustrate how a high-level software instruction is mapped to RISC-V assembly — the bridge between software and hardware.

**2.Implementation — Verilog RTL (picorv32 CPU core)**

This portion shows the hardware description (Verilog code) of a RISC-V compatible CPU core — specifically the PicoRV32, which is an open-source, lightweight 32-bit RISC-V processor core.

In this Verilog code:

* There is a module picorv32 with configurable parameters like ENABLE_COUNTERS, BARREL_SHIFTER, etc.
These parameters make the CPU flexible — you can enable/disable features during synthesis.

* The always @(posedge clk) block defines sequential behavior — operations triggered on every rising edge of the clock.
This is the heart of the RTL (Register Transfer Level) model, which defines:

    * How instructions are fetched and decoded.

    * How arithmetic/logic operations are performed.

    * How results are written back to registers or memory.

* The control signals like instr_lui, instr_auipc, instr_jal, etc., indicate which RISC-V instruction is currently being executed.

This Verilog code is the implementation of the RISC-V ISA at the hardware level.

➡️ Purpose: This section bridges architecture to hardware. It defines how RISC-V instructions actually work inside the processor — a hardware implementation written in Verilog HDL.

**3.Layout — Physical Design using Qflow**

This is the final stage of chip design — where the Verilog RTL (logical design) is transformed into an actual physical circuit layout ready for fabrication.

It shows a layout view generated by Qflow, an open-source VLSI design automation toolchain.

Qflow typically involves:

* Synthesis (using Yosys): Converts Verilog RTL to a gate-level netlist.

* Placement (using GrayWolf): Places standard cells from the technology library.

* Routing (using QRouter): Connects the placed cells using metal interconnects.

* Layout Visualization (using Magic): Displays the final layout with transistors, cells, and interconnects.

In the layout image:

* You can see labeled standard cells like AND2X1, DFFPOSX1, AOI21X1, etc.

   * These are logic gates and flip-flops from a standard-cell library.

* The pink regions represent placed cells.

* The blue lines represent routed metal interconnections between cells.

This layout is the final physical representation of the RISC-V processor core — the exact geometry that can be fabricated on a silicon wafer using semiconductor manufacturing processes.

➡️ Purpose: To show the final hardware realization — how Verilog RTL (digital logic) is turned into an actual chip layout at transistor and metal level.

| Stage              | Description                                    | Tool/Language Used                      | Domain               |
| ------------------ | ---------------------------------------------- | --------------------------------------- | -------------------- |
| **Architecture**   | Defines instruction set and execution behavior | RISC-V ISA, GCC toolchain               | Software + ISA       |
| **Implementation** | Implements ISA in hardware                     | Verilog HDL                             | RTL Design           |
| **Layout**         | Converts RTL to chip layout                    | Qflow (Yosys, GrayWolf, QRouter, Magic) | VLSI Physical Design |



## OpenLANE Directory structure

```bash
open the vsdworkshop ubuntu
```
<img width="1918" height="1016" alt="image" src="https://github.com/user-attachments/assets/f64f859d-0ac9-4f3d-a43a-835859ad40da" />

## LAB

```bash
#change the main directory
cd Desktop

#change the directory
cd work/tools/

#open the directory
ls -ltr

#change the directory
cd openlane_working_dir

#open the directory
ls -lrt

#change the directory
cd pdks
```

<img width="1290" height="912" alt="Screenshot 2025-10-27 120742" src="https://github.com/user-attachments/assets/f8a8a29d-a2bd-40e4-966c-9413245c513a" />
<img width="1284" height="559" alt="Screenshot 2025-10-27 120813" src="https://github.com/user-attachments/assets/e395a4b7-a238-4b9e-aa5a-c826937ad91b" />

In this terminal :

**~/Desktop/work/tools/openLane_working_dir/pdks/sky130A/**

* libs.tech/ - Technology libraries - tool-specific configuration files

      * DRC Rules: Design Rule Checking specifications

      * LVS Rules: Layout vs Schematic comparison rules

      * Display Resources: Color schemes, layer definitions

      * Technology Files: Tool-specific setup files

* libs.ref/ - Reference libraries - standard cell libraries, IO cells, etc.

     * Standard Cell Library: Digital logic cells with multiple views (layout, timing, functional)

     * I/O Library: Input/Output pad cells for chip interfaces

     * Primitive Devices: Basic transistors, resistors, capacitors

     * Memory Compilers: For generating RAM/ROM blocks

* source - Metadata file describing the PDK source and version

**Commands to invoke the OpenLANE flow and perform synthesis**

```bash
#Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

#alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
#Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker

## Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

#activates OpenLane’s Tcl package (version 0.9)
package require openlane 0.9 (in the OpenLANE environment)


```
<img width="795" height="907" alt="Screenshot 2025-10-27 140538" src="https://github.com/user-attachments/assets/bcf02d92-1d36-42cc-ab48-d62b10e84dbb" />

