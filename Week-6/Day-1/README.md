# DAY-1

# Inception of Open source EDA , OpenLANE and SKY130 PDK

## How to talk to computers

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

**2. Second Set of Flexible Pins***
This appears to be another bank of pins, perhaps on the opposite side of the chip.

"Direct I2C, direct QSPI, GPIO0-7, PWM0-3"

* This is very similar to the first block, suggesting the chip has two separate, dedicated I2C and QSPI peripherals. This is a powerful feature, allowing you to connect to two different QSPI memory chips or communicate on two separate I²C networks simultaneously without any performance compromise.

* GPIO0-7, PWM0-3: Another bank of general-purpose and PWM pins, numbered distinctly from the first set.

**3.The Multiplexed ("Muxed") IO Bank**
This is the core of the chip's pin flexibility. It shows a pool of peripherals that can be routed to a set of pins via software configuration.

* This list (I2C1, QSPI2, UART1/2, PWM0-5) represents the internal peripheral IPs available to be connected.

* The range "GPIO0-14" represents the physical pins that can be programmed to act as any of these peripherals.

* Implication: You can decide, for example, that you want GPIO5 to be a UART transmit pin and GPIO6 to be a UART receive pin. Alternatively, you could configure GPIO5 as a PWM output instead. This gives tremendous design flexibility but requires careful planning to avoid conflicts.

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




