# DAY 3

## Switching Threshold

The Switching Threshold (often denoted as V<sub>M</sub>) of a CMOS inverter is the specific value of the input voltage (V<sub>in</sub>) at which the output voltage (V<sub>out</sub>) is exactly equal to the input voltage.

In simpler terms, it's the point on the voltage transfer characteristic (VTC) curve where V<sub>in</sub> = V<sub>out</sub>. This is a metastable point where both the NMOS and PMOS transistors in the inverter are turned on and operating in the saturation region, creating a path of significant current from V<sub>DD</sub> to GND.

**Why is it important?**

* Noise Margins: It determines the balance between the high and low noise margins (N<sub>MH</sub> and N<sub>ML</sub>). An ideal inverter has V<sub>M</sub> = V<sub>DD</sub>/2.

* Switching Speed: It influences the propagation delay of the gate.

* Robustness: A V<sub>M</sub> that is not centered can make the circuit more susceptible to noise on one logic level than the other.
