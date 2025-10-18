# DAY 3

## Switching Threshold

The Switching Threshold (often denoted as V<sub>M</sub>) of a CMOS inverter is the specific value of the input voltage (V<sub>in</sub>) at which the output voltage (V<sub>out</sub>) is exactly equal to the input voltage.

In simpler terms, it's the point on the voltage transfer characteristic (VTC) curve where V<sub>in</sub> = V<sub>out</sub>. This is a metastable point where both the NMOS and PMOS transistors in the inverter are turned on and operating in the saturation region, creating a path of significant current from V<sub>DD</sub> to GND.

**Why is it important?**

* Noise Margins: It determines the balance between the high and low noise margins (N<sub>MH</sub> and N<sub>ML</sub>). An ideal inverter has V<sub>M</sub> = V<sub>DD</sub>/2.

* Switching Speed: It influences the propagation delay of the gate.

* Robustness: A V<sub>M</sub> that is not centered can make the circuit more susceptible to noise on one logic level than the other.

**Detailed Explanation: The Physics of V<sub>M</sub>**

A CMOS inverter consists of an NMOS and a PMOS transistor.

When V<sub>in</sub> is low (0 V), the NMOS is off, the PMOS is on, and V<sub>out</sub> is high (V<sub>DD</sub>).

When V<<sub>in</sub> is high (V<sub>DD</sub>), the PMOS is off, the NMOS is on, and V<sub>out</sub> is low (0 V).

The transition between these two states is not instantaneous. The Switching Threshold (V<sub>M</sub>) is the precise input voltage in the middle of this rapid transition where the inverter is, for a brief moment, in an analog amplifier mode. At this point:

Both transistors are conducting significant current.

Both transistors are typically in the saturation region (for a symmetric inverter).

The circuit draws a peak current from the power supply, known as the short-circuit current.

## Labs Sky130 SPICE simulation for CMOS

```bash
#open the file
vim day3_inv_vtc_Wp084_Wn036.spice
```
<img width="1046" height="805" alt="image" src="https://github.com/user-attachments/assets/b6f78036-3db3-4b06-b56a-76741f6b49e5" />

```bash
#plot the characteristics
ngspice day3_inv_vtc_Wp084_Wn036.spice

plot -vdd#branch
```

<img width="1252" height="805" alt="image" src="https://github.com/user-attachments/assets/57159edf-ad8e-4963-99e7-d298e6f9e5ff" />
<img width="1315" height="813" alt="image" src="https://github.com/user-attachments/assets/67c6f90c-cf75-4a61-9cbf-42820a21b969" />

Hence the switching threshold : 8.77mv [Vin = Vout]

## Rise delay and Fall delay

**What are Rise Time and Fall Time?**

* Rise Time (t<sub>r</sub>) is the time it takes for a signal to transition from a low logic level to a high logic level. Specifically, it's measured from 10% to 90% of the full voltage swing (from V<sub>DD</sub>).

* Fall Time (t<sub>f</sub>) is the time it takes for a signal to transition from a high logic level to a low logic level. Specifically, it's measured from 90% to 10% of the full voltage swing.

These parameters describe how "steep" or "slow" the edges of your digital signals are. Fast rise/fall times are crucial for high-speed digital circuits.

**Detailed Explanation:**

The Physics Behind t<sub>r</sub> and t<sub>f</sub>
The fundamental reason why signals don't transition instantaneously is the parasitic capacitance at the output node of a gate. This capacitance (C<sub>L</sub>) must be charged or discharged through the transistors.

**For Rise Time (t<sub>r</sub>):**

* When the input to a CMOS inverter goes LOW, the PMOS transistor turns ON.

* The PMOS acts like a variable resistor, creating a current path from V<sub>DD</sub> to the output node.

* This current charges the load capacitor C<sub>L</sub>.

* The voltage across a capacitor cannot change instantaneously - it follows an exponential (or RC) charging curve.

* The time it takes to charge from 10% to 90% of V<sub>DD</sub> is the rise time.

**For Fall Time (t<sub>f</sub>):**

* When the input goes HIGH, the NMOS transistor turns ON.

* The NMOS acts like a variable resistor, creating a current path from the output node to GND.

* This current discharges the load capacitor C<sub>L</sub>.

* The voltage discharges following an exponential (or RC) curve.

* The time it takes to discharge from 90% to 10% of V<sub>DD</sub> is the fall time.

## Labs Sky130 SPICE simulation for CMOS (transient analysis)

```bash
#open the file
vim day3_inv_trans_Wp084_Wn036.spice
```
<img width="1146" height="801" alt="image" src="https://github.com/user-attachments/assets/c1774116-1d53-490c-88c7-72e5b30c38c6" />

**Explanation of Pulse**

PULSE (0V 1.8V 0 0.1ns 0.1ns 2ns 4ns)

* 0V to 0.8V 0: pulse from 0 to 1.8 with a shift of 0.

* 0.1ns and 0.1ns : Rise time and Fall time

* 2ns : Pulse width

* 4ns : total time periods

```bash
#plot the characteristics
ngspice day3_inv_trans_Wp084_Wn036.spice

plot out vs in
```
<img width="1150" height="795" alt="image" src="https://github.com/user-attachments/assets/18251e1d-4568-45c7-b5ea-9497a202ebfb" />
<img width="1155" height="796" alt="image" src="https://github.com/user-attachments/assets/af5bdc30-dd91-4995-b297-2ee83348451b" />

**Rise delay**

<img width="1152" height="805" alt="image" src="https://github.com/user-attachments/assets/3d1ad83f-435e-4ec0-b66e-95d0cddbfebc" />

* on the x axes, the values are : 2.144 and 2.519

* then the rise delay will be = 2.519 - 2.144 = 0.375

**Fall delay**

<img width="965" height="117" alt="image" src="https://github.com/user-attachments/assets/76720243-c0e1-467b-8eb9-4dd59ef4404f" />
<img width="976" height="806" alt="image" src="https://github.com/user-attachments/assets/7a11ba2b-9937-4c90-bb33-4510765bbbad" />

* on the x axis , the values are : 8.055 and 8.304
* the the fall delay will be = 8.304 and 8.055 = 0.249
