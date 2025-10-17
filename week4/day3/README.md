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
