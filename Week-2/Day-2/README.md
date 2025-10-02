# VSDBabySoC functional modelling

## VSDBabySoC:

The VSDBabySoC is a simple SoC (System-on-Chip) design incorporating a RISC-V processor (rvmyth), a PLL (Phase-Locked Loop) module (pll), and a DAC (Digital-to-Analog Converter) module (dac). This project demonstrates integration of these IP cores and aims to simulate and verify the design behavior using pre-synthesis and post-synthesis simulations.

<img width="2048" height="1136" alt="image" src="https://github.com/user-attachments/assets/1055f6b1-d166-4f7e-a776-e746ea926155" />

The image is a mixed-signal BabySoC die/package diagram showing:

* a central digital core block labeled rvmyth (the CPU/SoC digital subsystem),

* a PLL/VCO block (yellow, labeled something like avsdpll_1v8) that uses a crystal pad (XO/XI) and a small current source for analog bias,

* a 10-bit DAC block (avsddac_3v3) on the right (powered/referenced at 3.3 V domain),

* an SPI block on the lower-left (3.3 V domain),

* level shifters (“LS”) between the 1.8 V digital domain and the 3.3 V I/O/analog domain,

* package pins for XO/XI (crystal), two pins labeled adc_low / adc_high (analog pins), and an analog output pin for the DAC,

* color-coded voltage domains: 1.8 V for the core and most logic, 3.3 V for certain peripherals/analog blocks,

* control / bias pins into the PLL (labels such as B_CP, B_VCO, EN_VCO, EN_CP, B[3:0], REF, VCO_IN) and a CLK output from the PLL to the core,

* the digital data bus to the DAC is D[9:0] and an EN line for the DAC.

## Functional modelling:

A functional model (aka behavioral or golden model) is a high-level, executable description of the system’s intended behavior and algorithms — written before you commit to cycle-accurate RTL and physical implementation. It focuses on functional correctness and architecture tradeoffs (what the system does), not on gate-level timing or placement (how it’s laid out).

## Labs (Hands-on Functional Modelling) 

## 1. VSDBabySoC.v (Top level SoC module)

Here we are going to model and simulate the VSDBabySoC using iverilog, then we will show the results using gtkwave tool. Some initial input signals will be fed into vsdbabysoc module that make the pll start generating the proper CLK for the circuit. The clock signal will make the rvmyth to execute instructions in its imem. As a result the register r17 will be filled with some values cycle by cycle. These values are used by dac core to provide the final output signal named OUT. So we have 3 main elements (IP cores) and a wrapper as an SoC and of-course there would be also a testbench module out there.

* First we need to install some packages:

```bash
$ sudo apt install make python python3 python3-pip git iverilog gtkwave docker.io
$ sudo chmod 666 /var/run/docker.sock
$ cd ~
$ pip3 install pyyaml click sandpiper-saas
```

* Clone this repository in an arbitrary directory (we'll choose home directory here):

```bash
$ cd ~
$ git clone https://github.com/manili/VSDBabySoC.git
```

* Make the pre_synth_sim.vcd:

  ```bash
$ cd VSDBabySoC
$ make pre_synth_sim
  ```

* Waveforms
```bash
$ gtkwave output/pre_synth_sim/pre_synth_sim.vcd
```

