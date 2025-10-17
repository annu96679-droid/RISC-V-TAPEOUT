# LAB1 
## BASIC SPICE STEUP:

```bash
cd

#clone the github
git clone https://github.com/kunalg123/sky130CircuitDesignWorkshop/

#open the directory
cd sky130CircuitDesignWorkshop

#check the files and directories in the sky130CircuitDesignWorkshop
ls

#open the directory
cd sky130CircuitDesignWorkshop

#open the directory
cd nfet

#check the files and directories in the sky130CircuitDesignWorkshop, sky130CircuitDesignWorkshop, nfet
ls
```
<img width="1280" height="805" alt="image" src="https://github.com/user-attachments/assets/050f0677-615c-4947-b938-d3919d23267d" />

```bash
#open the file
less sky130_fd_pr__nfet_01v8_tt.pm3.spice
// it contains all the different model parameter
```
<img width="1274" height="804" alt="Screenshot 2025-10-17 222652" src="https://github.com/user-attachments/assets/088c63c8-cf83-41e8-9896-b569ca934a57" />

```bash
#open the file
less sky130_fd_pr__nfet_01v8_tt.corner.spice
//it contains W/L values0
```
<img width="1287" height="802" alt="image" src="https://github.com/user-attachments/assets/c61883d5-d946-4125-a1f3-fc9b67406682" />

```bash
#open the directory
cd models/

#open the file
less sky130.lib.spice
// It contains all the library files for nfet and pfet
```

<img width="1015" height="797" alt="image" src="https://github.com/user-attachments/assets/a0499450-cb7b-4a3b-be2a-6f93e25c3516" />

```bash
#open the file
less spice
```
<img width="1007" height="800" alt="image" src="https://github.com/user-attachments/assets/61403439-3f03-492b-821b-885efaa4add4" />

## DAY1 : LAB SESSION
```bash
#open the file in the current directory
vim day1_nfet_idvds_L2_W5.spice

```
<img width="998" height="800" alt="image" src="https://github.com/user-attachments/assets/c364251e-505f-4b04-beac-1ec51799e7d9" />

```bash
#Plot the Ids vs Vds
ngspice day1_nfet_idvds_L2_W5.spice
```

<img width="1010" height="802" alt="image" src="https://github.com/user-attachments/assets/848b0947-3cfe-43e6-b4fd-cec741b14a64" />

```bash
plot -vdd#branch
```
<img width="1259" height="798" alt="Screenshot 2025-10-17 224700" src="https://github.com/user-attachments/assets/d513d477-6d00-4662-8466-c8bea6e7a682" />

* About the Graph 

This is a set of I–V characteristics (current–voltage curves) of an NMOS transistor.
It plots the drain current versus drain-to-source voltage, for multiple values of gate-to-source voltage.

Shape of the curves (region explanation)

Each curve has two main regions — let’s look at what happens as you move left → right:

(a) Linear / Triode Region

Found at low Vds (0 – ~0.3 V).

The transistor behaves like a resistor — Id increases linearly with Vds.

(b) Saturation Region

As Vds increases further, the curve flattens out (becomes nearly horizontal).

The transistor enters saturation — current becomes almost constant (controlled by Vgs only).

**Purpose:**
To understand how the drain current varies with drain-to-source voltage  for different gate voltages.

Why we do it:

To identify three regions of operation: cutoff, triode (linear), and saturation.

To extract output characteristics of the NMOS.

To determine saturation voltage (Vds, sat) and observe channel length modulation.

Essential for analog design (e.g., knowing when a MOSFET acts like a current source).

| **Parameter**                        | **Meaning**                                                                            |
| ------------------------------------ | -------------------------------------------------------------------------------------- |
| **Vgs (sweep parameter)** | Controls how strongly the NMOS conducts. Higher V<sub>GS</sub> → higher I<sub>D</sub>. |
| **V<sub>DS</sub> (x-axis)**          | Voltage applied across drain and source — determines the operating region.             |
| **I<sub>D</sub> (y-axis)**           | Drain current — output of the transistor model.                                        |
| **Flattening of curves**             | Shows transistor entering the **saturation region**.                                   |
| **Steeper slope at start**           | Indicates **stronger conduction** due to larger V<sub>GS</sub>.                        |


