# DAY5

## Power supply variations (Gain)

**Power Supply Variation in CMOS**

**1. What is Power Supply Variation?**
   
Power Supply Variation refers to the fluctuations or changes in the supply voltage (V<sub>DD</sub>) provided to a CMOS circuit. These variations can be:

* Static: Long-term variations due to manufacturing tolerances, temperature changes, or IR drops

* Dynamic: Short-term variations due to switching activity, power supply noise, or current surges.

**2. Detailed Explanation: Effects of Power Supply Variation**

Power supply variations affect CMOS circuits in several critical ways:

A. Performance Effects (Speed)
* Delay Variation: Propagation delay is strongly dependent on V<sub>DD</sub>

* Lower V<sub>DD</sub> → Higher delay (circuit slows down)

* Higher V<sub>DD</sub> → Lower delay (circuit speeds up)

* The relationship follows: t_d ∝ V_DD / (V_DD - V_th)^α where α ≈ 1.3-2

B. Power Consumption

* Dynamic Power: P_dyn = α × C_L × V_DD² × f

* Quadratic dependence on V<sub>DD</sub> - small variations cause significant power changes

* Static Power: P_static = I_leakage × V_DD

* Increases with higher V<sub>DD</sub> due to increased leakage currents

C. Noise Margins and Signal Integrity

* Lower V<sub>DD</sub> reduces noise margins

* Higher V<sub>DD</sub> improves noise margins but increases power and stress on transistors

D. Functionality

* Minimum Operating Voltage: Below a certain V<sub>DD</sub>, circuits fail to function correctly

* Reliability Issues: Higher V<sub>DD</sub> can cause hot carrier injection and oxide breakdown

**Gain in CMOS**

**1. What is Gain in CMOS?**
* In CMOS circuits, Gain refers to the voltage gain of a transistor or amplifier stage, defined as the ratio of output voltage change to input voltage change:

A_v = ΔV_out / ΔV_in

* For a CMOS inverter, this is the slope of the Voltage Transfer Characteristic (VTC) curve in the transition region.

**2. Detailed Explanation: Gain and Its Effects** 

A. Small-Signal Voltage Gain

* For a single transistor in saturation:
A_v = -g_m × R_out
Where:

g_m = Transconductance = ∂I_D / ∂V_GS

R_out = Output resistance

* For a CMOS inverter (both transistors in saturation):
A_v = -(g_mn + g_mp) × (r_on || r_op)

B. Effects of Gain in Digital Circuits

Regeneration and Noise Immunity:

* High gain provides sharp switching transitions

* Improves noise margins by making the VTC curve steeper

* Ensures digital signals are quickly restored to valid logic levels

Switching Characteristics:

* Higher gain → Faster transition through the uncertain region

* Reduces the time spent in intermediate voltage levels

## Sky130 Supply Variation Labs

```bash
#open the file
vim day3_inv_suppyvariation_Wp1_Wn036.spice
```
<img width="1097" height="806" alt="image" src="https://github.com/user-attachments/assets/8be3355a-c428-45a7-a813-b16b4fc11f2d" />

```bash
#plot the characteristics
ngspice day3_inv_suppyvariation_Wp1_Wn036.spice

plot out vs in
```
<img width="1259" height="796" alt="image" src="https://github.com/user-attachments/assets/b5946d93-3550-4301-b5cd-4beb1f6f8abf" />
<img width="1257" height="800" alt="image" src="https://github.com/user-attachments/assets/5b6f2d71-a723-48c1-aaa2-af063bd6c73c" />

**GAIN**
Here are the some points extracted from the graph from which we will find the gain
<img width="1246" height="798" alt="image" src="https://github.com/user-attachments/assets/3394699d-1742-496f-9246-178f838398c4" />

* Gain for supply voltage (1.8) = 7.75

* Gain for supply voltage (1.6) = 9.03

* Gain for supply voltage (1.4) = 10.25

* Gain for supply voltage (1.2) = 10.29

* Gain for supply voltage (1.0) = 10.33

* Gain for supply voltage (0.8) = 10.42

**Observations**

* Gain increases as VDD is reduced.

* At 1.8 V gain = 7.75, then it rises monotonically to 10.42 at 0.8 V.

* Most of the gain change happens between high VDD (1.8 V) and mid VDD (1.4–1.2 V); below ~1.4 V the gain largely plateaus with small changes.

* 1.4 → 1.2 → 1.0 are ~10.25 → 10.29 → 10.33 (very small variation), then a modest rise to 10.42 at 0.8 V.

* Relative change is significant at the top end but small across mid-to-low VDD.

* Gain at 1.8 V is ~28% lower than at 0.8 V.

* No obvious catastrophic collapse of gain at low VDD (down to 0.8 V) — circuit is still amplifying.

* But absolute gain change is modest in the 1.4→0.8 V range, indicating some operating-region stability there.

## Sky130 Device Variation Labs

```bash
#open the file
vim day3_inv_devicevariation_Wp7_Wn042.spice
```
<img width="937" height="802" alt="image" src="https://github.com/user-attachments/assets/33b21e84-667d-4102-9ee4-acf862d0c583" />

```bash
#plot the characteristics
ngspice day3_inv_devicevariation_Wp7_Wn042.spice

plot out vs in
```
<img width="936" height="796" alt="image" src="https://github.com/user-attachments/assets/7d392854-c95e-4696-90c7-9d70fac75c55" />
<img width="1217" height="798" alt="image" src="https://github.com/user-attachments/assets/73fd926f-a0e1-4fee-a928-ef8be532f84d" />
<img width="1205" height="130" alt="image" src="https://github.com/user-attachments/assets/d4ec497d-8d9e-462e-aea7-28b836dacbb7" />

Hence we got the switching threshold voltage = 0.98.


USB project:

Inferred memory devices in process
	in routine usb_phy line 108 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|     rst_cnt_reg     | Flip-flop |   5   |  Y  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_phy line 119 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|     usb_rst_reg     | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================
Presto compilation completed successfully. (usb_phy)
Loading db file '/home/.linuxserver/synopsys/syn/R-2020.09-SP5-5/libraries/syn/dw_foundation.sldb'
Elaborated 1 design.
Current design is now 'usb_phy'.
Information: Building the design 'usb_tx_phy'. (HDL-193)
Warning:  /home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv:355: Case statement is not a full case. (ELAB-909)

Statistics for case statements in always block at line 142 in file
	'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'
===============================================
|           Line           |  full/ parallel  |
===============================================
|           145            |    user/user     |
===============================================

Statistics for case statements in always block at line 346 in file
	'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'
===============================================
|           Line           |  full/ parallel  |
===============================================
|           355            |    user/user     |
===============================================

Inferred memory devices in process
	in routine usb_tx_phy line 77 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|    TxReady_o_reg    | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_tx_phy line 82 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|     ld_data_reg     | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_tx_phy line 92 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|      tx_ip_reg      | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_tx_phy line 103 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|   tx_ip_sync_reg    | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_tx_phy line 116 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|    data_done_reg    | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_tx_phy line 132 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|     bit_cnt_reg     | Flip-flop |   3   |  Y  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_tx_phy line 142 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|    sd_raw_o_reg     | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_tx_phy line 156 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|    sft_done_reg     | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_tx_phy line 159 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|   sft_done_r_reg    | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_tx_phy line 165 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|    hold_reg_reg     | Flip-flop |   8   |  Y  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_tx_phy line 170 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|   hold_reg_d_reg    | Flip-flop |   8   |  Y  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_tx_phy line 180 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|     one_cnt_reg     | Flip-flop |   3   |  Y  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_tx_phy line 197 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|     sd_bs_o_reg     | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_tx_phy line 211 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|    sd_nrzi_o_reg    | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_tx_phy line 227 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|   append_eop_reg    | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_tx_phy line 238 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'.
================================================================================
|    Register Name     |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
================================================================================
| append_eop_sync1_reg | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
================================================================================

Inferred memory devices in process
	in routine usb_tx_phy line 247 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'.
================================================================================
|    Register Name     |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
================================================================================
| append_eop_sync2_reg | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
================================================================================

Inferred memory devices in process
	in routine usb_tx_phy line 256 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'.
================================================================================
|    Register Name     |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
================================================================================
| append_eop_sync3_reg | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
================================================================================

Inferred memory devices in process
	in routine usb_tx_phy line 266 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'.
================================================================================
|    Register Name     |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
================================================================================
| append_eop_sync4_reg | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
================================================================================

Inferred memory devices in process
	in routine usb_tx_phy line 282 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|     txoe_r1_reg     | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_tx_phy line 291 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|     txoe_r2_reg     | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_tx_phy line 300 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|      txoe_reg       | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_tx_phy line 314 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|      txdp_reg       | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_tx_phy line 325 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|      txdn_reg       | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_tx_phy line 341 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_tx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|      state_reg      | Flip-flop |   3   |  Y  | N  | N  | N  | N  | N  | N  |
===============================================================================
Presto compilation completed successfully. (usb_tx_phy)
Information: Building the design 'usb_rx_phy'. (HDL-193)

Statistics for case statements in always block at line 137 in file
	'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'
===============================================
|           Line           |  full/ parallel  |
===============================================
|           140            |    user/user     |
===============================================

Statistics for case statements in always block at line 189 in file
	'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'
===============================================
|           Line           |  full/ parallel  |
===============================================
|           195            |    user/user     |
===============================================

Inferred memory devices in process
	in routine usb_rx_phy line 70 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|      rx_en_reg      | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 71 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|    sync_err_reg     | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 83 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|     rxd_s0_reg      | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 84 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|     rxd_s1_reg      | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 85 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|      rxd_s_reg      | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 90 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|     rxdp_s0_reg     | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 91 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|     rxdp_s1_reg     | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 92 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|    rxdp_s_r_reg     | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 93 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|     rxdp_s_reg      | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 95 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|     rxdn_s0_reg     | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 96 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|     rxdn_s1_reg     | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 97 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|    rxdn_s_r_reg     | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 98 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|     rxdn_s_reg      | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 104 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|      se0_s_reg      | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 123 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|      rxd_r_reg      | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 132 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|   dpll_state_reg    | Flip-flop |   2   |  Y  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 162 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|    fs_ce_r1_reg     | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 163 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|    fs_ce_r2_reg     | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 164 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|      fs_ce_reg      | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 184 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|    fs_state_reg     | Flip-flop |   3   |  Y  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 276 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|    rx_active_reg    | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 284 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|   rx_valid_r_reg    | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 294 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|      sd_r_reg       | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 300 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|     sd_nrzi_reg     | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 316 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|     one_cnt_reg     | Flip-flop |   3   |  Y  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 330 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|  bit_stuff_err_reg  | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 337 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|    shift_en_reg     | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 340 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|    hold_reg_reg     | Flip-flop |   8   |  Y  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 352 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|     bit_cnt_reg     | Flip-flop |   3   |  Y  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 363 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|    rx_valid1_reg    | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 371 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|    rx_valid_reg     | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 373 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|      se0_r_reg      | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================

Inferred memory devices in process
	in routine usb_rx_phy line 375 in file
		'/home/user4/Desktop/usb_phy/rtls/usb_rx_phy.sv'.
===============================================================================
|    Register Name    |   Type    | Width | Bus | MB | AR | AS | SR | SS | ST |
===============================================================================
|    byte_err_reg     | Flip-flop |   1   |  N  | N  | N  | N  | N  | N  | N  |
===============================================================================
Presto compilation completed successfully. (usb_rx_phy)

