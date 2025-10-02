# VSDBabySoC functional modelling

# VSDBabySoC:

The VSDBabySoC is a simple SoC (System-on-Chip) design incorporating a RISC-V processor (rvmyth), a PLL (Phase-Locked Loop) module (pll), and a DAC (Digital-to-Analog Converter) module (dac). This project demonstrates integration of these IP cores and aims to simulate and verify the design behavior using pre-synthesis and post-synthesis simulations.

<img width="2048" height="1136" alt="image" src="https://github.com/user-attachments/assets/1055f6b1-d166-4f7e-a776-e746ea926155" />

* The image is a mixed-signal BabySoC die/package diagram showing:

* a central digital core block labeled rvmyth (the CPU/SoC digital subsystem),

* a PLL/VCO block (yellow, labeled something like avsdpll_1v8) that uses a crystal pad (XO/XI) and a small current source for analog bias,

* a 10-bit DAC block (avsddac_3v3) on the right (powered/referenced at 3.3 V domain),

* an SPI block on the lower-left (3.3 V domain),

* level shifters (“LS”) between the 1.8 V digital domain and the 3.3 V I/O/analog domain,

* package pins for XO/XI (crystal), two pins labeled adc_low / adc_high (analog pins), and an analog output pin for the DAC,

* color-coded voltage domains: 1.8 V for the core and most logic, 3.3 V for certain peripherals/analog blocks,

* control / bias pins into the PLL (labels such as B_CP, B_VCO, EN_VCO, EN_CP, B[3:0], REF, VCO_IN) and a CLK output from the PLL to the core,

* the digital data bus to the DAC is D[9:0] and an EN line for the DAC.
