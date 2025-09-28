# BabySoC Fundamentals & Functional Modelling 

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

 


