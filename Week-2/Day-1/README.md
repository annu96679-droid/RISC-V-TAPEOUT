# BabySoC Fundamentals

# What is System On Chip (SOC)?

 A system on a chip is “simply” every element of a computer on a single integrated circuit. However, in general, SoCs cannot accommodate enough memory for a full operating system such as Linux or Android and, thus, some external memory must be provided, often on top of the SoC itself.

<img width="1899" height="1011" alt="Screenshot 2025-09-28 104950" src="https://github.com/user-attachments/assets/5f7ff0e8-8fd6-4885-af06-74c946a28982" />


 A System on Chip (SoC) is an integrated circuit (IC) that combines all the essential components of a computer or embedded system onto a single chip. Unlike traditional computer systems where the CPU, memory, peripherals, and interfaces are separate chips, in an SoC, everything is on the same silicon die.

 <img width="1911" height="1054" alt="Screenshot 2025-09-28 105845" src="https://github.com/user-attachments/assets/b36042a9-1cb2-4131-a8e5-88015049261c" />



# Key Components of an SoC

An SoC generally contains the following blocks:

**a) Processor/Core**

* The brain of the SoC.

* Can be single-core or multi-core.

* Can include CPU (RISC-V, ARM, etc.), GPU, or specialized cores like DSP (Digital Signal Processor) for signal processing tasks.

* Responsible for executing instructions and controlling other parts of the system.

**b) Memory**

* RAM (volatile): For storing temporary data during computation.

* ROM/Flash (non-volatile): For boot code and permanent storage.

* May include cache memory for fast access to frequently used data.

**c) Interconnect / Bus**

* Communication backbone connecting CPU, memory, and peripherals.

* Common interconnects: AMBA (AXI, AHB, APB), Wishbone, TileLink.

* Responsible for data transfer between different blocks efficiently.

**d) Peripherals**

* Provide interface to the external world or specialized functions.

Examples:

* UART, SPI, I2C → Serial communication

* GPIO → General purpose input/output pins

* Timers, Counters, PWM → Control logic

**e) I/O Interfaces**

* For connecting to external devices:

* USB, HDMI, Ethernet, PCIe, etc.

* Often specialized hardware blocks are used to handle these protocols.

**f) Accelerators / Co-processors**

* Specialized hardware blocks to speed up computation:

* AI accelerators (for ML tasks)

* Video codecs

* Crypto engines (encryption/decryption)

* These improve performance without overloading the CPU.

# Central Processing Unit ( RISC-V, ARM )

The job of a processor is to manipulate data. To do this, it executes a program, which is simply a list 
of basic instructions. Many such instructions are available on all processors, in the form of simple 
arithmetic and logical operations. The unit that executes these is the arithmetic and logical unit (ALU) 
and the operations are the classical ADDition, SUBtraction, sometimes MULtiplication and DIVision, 
logical AND, OR, EOR (exclusive OR, sometimes named XOR), and NOT, together with SHIFTing and 
ROTation. These latter two can function in a single cycle or on multiple cycles via a barrel shifter.

<img width="719" height="862" alt="Screenshot 2025-09-28 113302" src="https://github.com/user-attachments/assets/9e606d4e-097a-41c1-b97a-f55e2971fb8f" />

# Memory

A processor needs to have memories for both the code and 
the data. A further specific area is used for a stack, a special form of data memory into which the 
processor can PUSH data to store it and POP data to recover it. We will consider this in more detail 
in due course, but we first need to have a model of memory, starting with a high-level model before 
moving to the level of the transistor.

 A memory can be viewed as a large number of separate drawers, each with its own identification 
number, starting from 0 and increasing thereafter. Thus, a position in memory can be seen as a specific 
drawer and inside this a set of boxes represent the bits of information, an empty box representing a 
binary 0, and a full box a binary 1. If all of the drawers are housed in a single cabinet, this represents 
the memory chip. It is selected by a signal, the chip select (CS), which can be seen as the key to locking 
or unlocking the door of the cabinet.

<img width="1072" height="436" alt="image" src="https://github.com/user-attachments/assets/a1894003-9068-4fbf-9057-3b122d86f96e" />

# Interconnect

interconnects are fundamental to the flexibility, 
manageability, and performance of a system

<img width="1072" height="441" alt="image" src="https://github.com/user-attachments/assets/9a706852-7e10-4bc2-a924-51e8d35a9eac" />





 





