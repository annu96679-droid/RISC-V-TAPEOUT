# DAY4

## Introduction to noise margin

**What is Noise Margin?**

Noise Margin is the amount of noise voltage that can be added to a digital signal without causing an incorrect interpretation of the logic level at the receiver gate.

In simpler terms, it's the "safety margin" that protects against unwanted electrical noise (from crosstalk, power supply fluctuations, electromagnetic interference, etc.) that could accidentally flip a logic 0 to a 1 or vice-versa.

There are two types of noise margins:

* Noise Margin High (NM<sub>H</sub>): The safety margin for logic HIGH levels

* Noise Margin Low (NM<sub>L</sub>): The safety margin for logic LOW levels

**Detailed Explanation: The Need for Noise Margin**

Digital circuits don't switch instantaneously at a single voltage. The Voltage Transfer Characteristic (VTC) curve has a transition region. Without noise margins, any small noise could push the input voltage into this uncertain region, causing unpredictable behavior.

Noise margins ensure that:

* A transmitted "1" is received unambiguously as a "1" even with some negative noise

* A transmitted "0" is received unambiguously as a "0" even with some positive noise

  
**Key Voltage Levels for Noise Margin Calculation**
To understand noise margins, we need to define four critical voltage points:

For the OUTPUT of a gate:

* V<sub>OH</sub>: Output High Voltage - The minimum voltage that the driving gate will output when sending a logic "1"

* V<sub>OL</sub>: Output Low Voltage - The maximum voltage that the driving gate will output when sending a logic "0"

For the INPUT of a gate:
* V<sub>IH</sub>: Input High Voltage - The minimum input voltage that will be reliably interpreted as a logic "1"

* V<sub>IL</sub>: Input Low Voltage - The maximum input voltage that will be reliably interpreted as a logic "0"
