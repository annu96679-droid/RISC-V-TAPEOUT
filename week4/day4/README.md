# DAY4

## Introduction to noise margin

**1. What is Noise Margin?**

Noise Margin is the amount of noise voltage that can be added to a digital signal without causing an incorrect interpretation of the logic level at the receiver gate.

In simpler terms, it's the "safety margin" that protects against unwanted electrical noise (from crosstalk, power supply fluctuations, electromagnetic interference, etc.) that could accidentally flip a logic 0 to a 1 or vice-versa.

There are two types of noise margins:

* Noise Margin High (NM<sub>H</sub>): The safety margin for logic HIGH levels

* Noise Margin Low (NM<sub>L</sub>): The safety margin for logic LOW levels

**2. Detailed Explanation: The Need for Noise Margin**

Digital circuits don't switch instantaneously at a single voltage. The Voltage Transfer Characteristic (VTC) curve has a transition region. Without noise margins, any small noise could push the input voltage into this uncertain region, causing unpredictable behavior.

Noise margins ensure that:

* A transmitted "1" is received unambiguously as a "1" even with some negative noise

* A transmitted "0" is received unambiguously as a "0" even with some positive noise

  
**3. Key Voltage Levels for Noise Margin Calculation**
To understand noise margins, we need to define four critical voltage points:

For the OUTPUT of a gate:

* V<sub>OH</sub>: Output High Voltage - The minimum voltage that the driving gate will output when sending a logic "1"

* V<sub>OL</sub>: Output Low Voltage - The maximum voltage that the driving gate will output when sending a logic "0"

For the INPUT of a gate:
* V<sub>IH</sub>: Input High Voltage - The minimum input voltage that will be reliably interpreted as a logic "1"

* V<sub>IL</sub>: Input Low Voltage - The maximum input voltage that will be reliably interpreted as a logic "0"

**4. How to Calculate Noise Margin**

The noise margins are calculated as:

* Noise Margin High (NM<sub>H</sub>):
NM_H = V_OH - V_IH

* Noise Margin Low (NM<sub>L</sub>):
NM_L = V_IL - V_OL

* Total Noise Margin:
NM_Total = NM_H + NM_L

## Sky130 Noise margin labs

```bash
#open the file
vim day4_inv_noisemargin_Wp1_Wn036.spice
```
<img width="937" height="796" alt="Screenshot 2025-10-18 101801" src="https://github.com/user-attachments/assets/1aad86bd-d599-4947-8cc7-2a2898c2b4c2" />


```bash
#plot the characteristics
ngspice day4_inv_noisemargin_Wp1_Wn036.spice

plot -vdd#branch
```
<img width="1026" height="800" alt="image" src="https://github.com/user-attachments/assets/8ec79a52-7faa-499c-b149-2b282a90de52" />
<img width="1282" height="802" alt="image" src="https://github.com/user-attachments/assets/0517dac2-03be-44f7-a0fd-6cf72e6f29f8" />
<img width="1417" height="802" alt="image" src="https://github.com/user-attachments/assets/9e70b463-98f3-454a-894d-55e833b1800f" />

**Points for calculate the Noise margin high and Noise margin low**
<img width="1257" height="182" alt="image" src="https://github.com/user-attachments/assets/dfab1654-108d-41ae-9453-be6f39f6719b" />

* (Noise margin high) NM_H = V_OH - V_IH = 0.766071 - 0.124638 = 0.641ps

* (Noise margin low) NM_L = V_IL - V_OL = 1.71594 - 0.969643 = 0.746ps
