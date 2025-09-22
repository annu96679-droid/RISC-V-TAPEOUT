# Week 0 - Day 2


# Day 2 - TOOLS INSTALLATION


**UBUNTU**

I have set up Ubuntu as my development environment. This will be used for compiling, running, and experimenting with various tools related to my projects.

Installed Ubuntu on [VirtualBox ].

 
<div align="center">

| **Specification**      | **Details**            |
|-----------------------|-----------------------|
| **Operating System**    | Ubuntu 20.04+         |
| **RAM**                 | 6GB                   |
| **Storage**              | 50GB HDD              |
| **vCPUs**              | 4                     |

</div> 

<img width="1917" height="1016" alt="image" src="https://github.com/user-attachments/assets/abe92ec1-9636-47fa-9e67-89d99d780df0" />



<img width="1282" height="903" alt="image" src="https://github.com/user-attachments/assets/51176deb-0192-4855-b1f1-6ec3b5aef2ba" />


**YOSYS SETUP**

YOSYS is an open-source framework for RTL synthesis widely used in VLSI and FPGA flows.

*INSTALLATION COMMANDS*

```bash
**Update packages**
sudo apt-get update

**Clone Yosys source**
git clone https://github.com/YosysHQ/yosys.git
cd yosys

**Install required dependencies**
sudo apt-get install -y build-essential clang bison flex \
  libreadline-dev gawk tcl-dev libffi-dev git \
  graphviz xdot pkg-config python3 libboost-system-dev \
  libboost-python-dev libboost-filesystem-dev zlib1g-dev

**Configure and build**
make config-gcc
make 

**Install**
sudo make install
```

<img width="911" height="936" alt="image" src="https://github.com/user-attachments/assets/999921ad-9a95-4c8a-8abb-d812c8a008f1" />


**IVERILOG SETUP**

Icarus Verilog is an open-source Verilog simulation and synthesis tool widely used in digital design and FPGA development.

**Icarus Verilog Installation**

```bash
# Update package lists
sudo apt-get update

# Install Icarus Verilog
sudo apt-get install -y iverilog
```

Test with a Simple Verilog Program

```bash
module hello;
  initial begin
    $display("Hello, Verilog!");
    $finish;
  end
endmodule
```
<img width="1292" height="957" alt="image" src="https://github.com/user-attachments/assets/3f386e72-3365-4628-b8ce-ef0789abe5a6" />

**GTKWAVE INSTALLATION**

GTKWave is an open-source **waveform viewer** used in digital design.  
When you simulate Verilog code using tools like **Icarus Verilog**, the simulation generates a **VCD (Value Change Dump)** file.  
GTKWave lets you **visualize signal transitions** over time, which is very useful for debugging and verifying digital circuits.

*INSTALLATION COMMANDS*


```bash
sudo apt-get update
sudo apt install gtkwave
```

<img width="1158" height="1023" alt="image" src="https://github.com/user-attachments/assets/1d9392ee-cbb7-4e3b-98c7-2857d2bd63e7" />

**Purpose of the setup**


The tools (Yosys, Icarus Verilog, GTKWave) cover the main steps:

**Yosys → synthesis**

**Icarus Verilog → simulation**

**GTKWave → waveform viewing & debugging**


How to use it together (workflow overview)

Write Verilog code → simulate it with Icarus Verilog → debug with GTKWave → synthesize with Yosys.

This shows the big picture and connects the tools logically.
