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

Script.tcl

################ project 1st : USB ###################
#
#
#
#Design_compiler tools

############### setting the variables for paths ###################

set path_ref { /home/user4/Desktop/projects/SAED_32nm/NDM/}
set my_ref_libs {$path_ref/saed32_1p9m_tech.ndm/ \
                 $path_ref/saed32_hvt.ndm/  \
                 $path_ref/saed32_lvt.ndm/   \
                 $path_ref/saed32_rvt.ndm  \
                 $path_ref/saed32_sram_lp.ndm}

set ref_lib {/home/user4/Desktop/projects/SAED32_EDK/stdcell_hvt/milkyway/saed32nm_hvt_1p9m/
             /home/user4/Desktop/projects/SAED32_EDK/stdcell_lvt/milkyway/saed32nm_lvt_1p9m/
              /home/user4/Desktop/projects/SAED32_EDK/stdcell_rvt/milkyway/saed32nm_rvt_1p9m/}

set tech_libs {/home/user4/Desktop/projects/SAED_32nm/tech/saed32nm_1p9m.tf}

############### setting the synthetic_library############

set synthetic_library dw_foundation.sldb
set my_lib "usb_phy.mw"
#file delete -force $my_lib
######################## creating the library #######################

create_mw_lib usb_phy.mw -technology $tech_libs  -mw_reference_library $ref_lib -open

############### setting the paths for target and link, reference library#############
#
 set path_hvt {/home/user4/Desktop/projects/SAED32_EDK/stdcell_hvt/db_ccs/}
 set path_lvt {/home/user4/Desktop/projects/SAED32_EDK/stdcell_lvt/db_ccs/}
 set path_rvt {/home/user4/Desktop/projects/SAED32_EDK/stdcell_rvt/db_ccs/}
  
######################## setting Target_libary ##################   
#
set target_library "$path_hvt/saed32hvt_dlvl_ff0p95v125c_i1p16v.db \
                    $path_hvt/saed32hvt_dlvl_ff0p95vn40c_i1p16v.db \
                    $path_hvt/saed32hvt_ulvl_ff1p16v125c_i0p95v.db \
                    $path_hvt/saed32hvt_ulvl_ff1p16vn40c_i0p95v.db \
                    $path_hvt/saed32hvt_dlvl_ff0p95v125c_i1p16v.db \
                    $path_hvt/saed32hvt_dlvl_ff0p95vn40c_i1p16v.db \
                    $path_hvt/saed32hvt_ulvl_ff1p16v125c_i0p95v.db \
                    $path_hvt/saed32hvt_ulvl_ff1p16vn40c_i0p95v.db \
                      $path_lvt/saed32lvt_ff0p95v125c.db \
                      $path_lvt/saed32lvt_ff0p95vn40c.db \
                      $path_lvt/saed32lvt_ss0p95v125c.db \
                      $path_hvt/saed32hvt_ss0p95v125c.db \
                      $path_hvt/saed32hvt_ff1p16v125c.db \
                      $path_hvt/saed32hvt_ff1p16vn40c.db "
                        

##################### setting the link_library ##################
#

 set link_library    " $path_hvt/saed32hvt_dlvl_ff0p95v125c_i1p16v.db \
                    $path_hvt/saed32hvt_dlvl_ff0p95vn40c_i1p16v.db \
                    $path_hvt/saed32hvt_ulvl_ff1p16v125c_i0p95v.db \
                    $path_hvt/saed32hvt_ulvl_ff1p16vn40c_i0p95v.db \
                    $path_hvt/saed32hvt_dlvl_ff0p95v125c_i1p16v.db \
                    $path_hvt/saed32hvt_dlvl_ff0p95vn40c_i1p16v.db \
                    $path_hvt/saed32hvt_ulvl_ff1p16v125c_i0p95v.db \
                    $path_hvt/saed32hvt_ulvl_ff1p16vn40c_i0p95v.db \
                      $path_lvt/saed32lvt_ff0p95v125c.db \
                      $path_lvt/saed32lvt_ff0p95vn40c.db \
                      $path_lvt/saed32lvt_ss0p95v125c.db \
                      $path_hvt/saed32hvt_ss0p95v125c.db \
                      $path_hvt/saed32hvt_ff1p16v125c.db \
                      $path_hvt/saed32hvt_ff1p16vn40c.db "

#################### reference_library ############################
#
#
set reference_library  "$path_hvt/saed32hvt_dlvl_ff0p95v125c_i1p16v.db \
                    $path_hvt/saed32hvt_dlvl_ff0p95vn40c_i1p16v.db \
                    $path_hvt/saed32hvt_ulvl_ff1p16v125c_i0p95v.db \
                    $path_hvt/saed32hvt_ulvl_ff1p16vn40c_i0p95v.db \
                    $path_hvt/saed32hvt_dlvl_ff0p95v125c_i1p16v.db \
                    $path_hvt/saed32hvt_dlvl_ff0p95vn40c_i1p16v.db \
                    $path_hvt/saed32hvt_ulvl_ff1p16v125c_i0p95v.db \
                    $path_hvt/saed32hvt_ulvl_ff1p16vn40c_i0p95v.db \
                      $path_lvt/saed32lvt_ff0p95v125c.db \
                      $path_lvt/saed32lvt_ff0p95vn40c.db \
                      $path_lvt/saed32lvt_ss0p95v125c.db \
                      $path_hvt/saed32hvt_ss0p95v125c.db \
                      $path_hvt/saed32hvt_ff1p16v125c.db \
                      $path_hvt/saed32hvt_ff1p16vn40c.db  "

current_mw_lib
report_mw_lib
------------------
run.tcl

source -echo /home/user4/Desktop/usb_phy/synth/scripts/setup.tcl
set rtl_path "/home/user4/Desktop/usb_phy/rtls"
analyze -format sverilog [glob $rtl_path/*.sv]
elaborate usb_phy
current_design
remove_upf
load_upf /home/user4/Desktop/usb_phy/synth/constraints/usb_phy.upf
remove_sdc
read_sdc /home/user4/Desktop/usb_phy/synth/constraints/usb_phy.sdc -echo

set_voltage 1.16 -object_list VDD
set_voltage 0.95 -object_list VDDL
set_voltage 0.0 -object_list VSS
set_svf usb_phy.svf

create_operating_conditions -process 1.0 -voltage 0.95 -temperature 125.0 -library saed32lvt_ff0p95v125c -name ff0p95v125c
set_operating_conditions -max ff0p95v125c

set_app_var auto_insert_level_shifters_on_clocks all

compile_ultra -no_autoungroup -no_boundary_optimization

report_level_shifter -domain [get_power_domains * -hierarchical]
report_level_shifter

write_file -format verilog -hierarchy -output usb_phy_gln.v
write_icc2_files -output ../outputs/icc2_files
write_parasitics -format reduced  -output usb_phy_parasitics.spef > ../reports/usb_phy_parasitics.spef
write_sdc usb_phy_exc.sdc
report_area
report_timing
report_power_domain

------

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

level shifters in run.tcl

Power Domain : usb_phy

 **********************************************************
 Level shifter summary for power domain
 **********************************************************
 No. of violating level shifters - dont_touch    : 0
 No. of violating level shifters - no dont_touch : 0
 No. of level shifters in domain                 : 14
 **********************************************************

===================================================================================================================================================
 Level Shifter       Reference      Input P/G      Output P/G     Main P/G       Violation   Reason                 Voltage             Type
                                    Net Info(Volt) Net Info(Volt) Net Info(Dir.)
===================================================================================================================================================
 RxValid_o_UPF_LS    LSUPX1_HVT     VDDL(0.95)     VDD(1.16)      VDD(Output)    FALSE       -                      (0.7,1.16)          LH
                                    VSS(0.00)      VSS(0.00)      ---
 RxActive_o_UPF_LS   LSUPX1_HVT     VDDL(0.95)     VDD(1.16)      VDD(Output)    FALSE       -                      (0.7,1.16)          LH
                                    VSS(0.00)      VSS(0.00)      ---
 RxError_o_UPF_LS    LSUPX1_HVT     VDDL(0.95)     VDD(1.16)      VDD(Output)    FALSE       -                      (0.7,1.16)          LH
                                    VSS(0.00)      VSS(0.00)      ---
 DataIn_o[7]_UPF_LS  LSUPX1_HVT     VDDL(0.95)     VDD(1.16)      VDD(Output)    FALSE       -                      (0.7,1.16)          LH
                                    VSS(0.00)      VSS(0.00)      ---
 DataIn_o[6]_UPF_LS  LSUPX1_HVT     VDDL(0.95)     VDD(1.16)      VDD(Output)    FALSE       -                      (0.7,1.16)          LH
                                    VSS(0.00)      VSS(0.00)      ---
 DataIn_o[5]_UPF_LS  LSUPX1_HVT     VDDL(0.95)     VDD(1.16)      VDD(Output)    FALSE       -                      (0.7,1.16)          LH
                                    VSS(0.00)      VSS(0.00)      ---
 DataIn_o[4]_UPF_LS  LSUPX1_HVT     VDDL(0.95)     VDD(1.16)      VDD(Output)    FALSE       -                      (0.7,1.16)          LH
                                    VSS(0.00)      VSS(0.00)      ---
 DataIn_o[3]_UPF_LS  LSUPX1_HVT     VDDL(0.95)     VDD(1.16)      VDD(Output)    FALSE       -                      (0.7,1.16)          LH
                                    VSS(0.00)      VSS(0.00)      ---
 DataIn_o[2]_UPF_LS  LSUPX1_HVT     VDDL(0.95)     VDD(1.16)      VDD(Output)    FALSE       -                      (0.7,1.16)          LH
                                    VSS(0.00)      VSS(0.00)      ---
 DataIn_o[1]_UPF_LS  LSUPX1_HVT     VDDL(0.95)     VDD(1.16)      VDD(Output)    FALSE       -                      (0.7,1.16)          LH
                                    VSS(0.00)      VSS(0.00)      ---
 DataIn_o[0]_UPF_LS  LSUPX1_HVT     VDDL(0.95)     VDD(1.16)      VDD(Output)    FALSE       -                      (0.7,1.16)          LH
                                    VSS(0.00)      VSS(0.00)      ---
 LineState_o[1]_UPF_LS
                     LSUPX1_HVT     VDDL(0.95)     VDD(1.16)      VDD(Output)    FALSE       -                      (0.7,1.16)          LH
                                    VSS(0.00)      VSS(0.00)      ---
 LineState_o[0]_UPF_LS
                     LSUPX1_HVT     VDDL(0.95)     VDD(1.16)      VDD(Output)    FALSE       -                      (0.7,1.16)          LH
                                    VSS(0.00)      VSS(0.00)      ---
 fs_ce_UPF_LS        LSUPX1_HVT     VDDL(0.95)     VDD(1.16)      VDD(Output)    FALSE       -                      (0.7,1.16)          LH
                                    VSS(0.00)      VSS(0.00)      ---
===================================================================================================================================================
1
dc_shell> 


Report area:
****************************************
Report : area
Design : usb_phy
Version: R-2020.09-SP5-5
Date   : Mon Mar  2 12:12:39 2026
****************************************

Information: Updating design information... (UID-85)
Library(s) Used:

    saed32hvt_ff1p16v125c (File: /home/user4/Desktop/projects/SAED32_EDK/stdcell_hvt/db_ccs/saed32hvt_ff1p16v125c.db)
    saed32hvt_ulvl_ff1p16v125c_i0p95v (File: /home/user4/Desktop/projects/SAED32_EDK/stdcell_hvt/db_ccs/saed32hvt_ulvl_ff1p16v125c_i0p95v.db)
    saed32lvt_ff0p95v125c (File: /home/user4/Desktop/projects/SAED32_EDK/stdcell_lvt/db_ccs/saed32lvt_ff0p95v125c.db)
    saed32hvt_dlvl_ff0p95v125c_i1p16v (File: /home/user4/Desktop/projects/SAED32_EDK/stdcell_hvt/db_ccs/saed32hvt_dlvl_ff0p95v125c_i1p16v.db)

Number of ports:                           70
Number of nets:                           423
Number of cells:                          335
Number of combinational cells:            234
Number of sequential cells:                98
Number of macros/black boxes:               0
Number of buf/inv:                         25
Number of references:                      18

Combinational area:                603.846149
Buf/Inv area:                       31.768000
Noncombinational area:             648.321365
Macro/Black Box area:                0.000000
Net Interconnect area:             208.733045

Total cell area:                  1252.167514
Total area:                       1460.900560
1

report_timing:

Report : timing
        -path full
        -delay max
        -max_paths 1
Design : usb_phy
Version: R-2020.09-SP5-5
Date   : Mon Mar  2 12:14:41 2026
****************************************
Wire Load Model Mode: enclosed

  Startpoint: i_rx_phy/fs_ce_reg
              (rising edge-triggered flip-flop clocked by usb_clk)
  Endpoint: i_tx_phy/one_cnt_reg[2]
            (rising edge-triggered flip-flop clocked by usb_clk)
  Path Group: usb_clk
  Path Type: max

  Des/Clust/Port     Wire Load Model       Library
  ------------------------------------------------
  usb_rx_phy         8000                  saed32hvt_dlvl_ff0p95v125c_i1p16v
  usb_phy            8000                  saed32hvt_dlvl_ff0p95v125c_i1p16v
  usb_tx_phy         8000                  saed32hvt_dlvl_ff0p95v125c_i1p16v

  Point                                                   Incr       Path      Voltage
  ------------------------------------------------------------------------------------
  clock usb_clk (rise edge)                               0.00       0.00      
  clock network delay (ideal)                             0.00       0.00      
  i_rx_phy/fs_ce_reg/CLK (DFFX1_LVT)                      0.00       0.00 r    0.95
  i_rx_phy/fs_ce_reg/Q (DFFX1_LVT)                        0.07       0.07 r    0.95
  i_rx_phy/fs_ce (usb_rx_phy)                             0.00       0.07 r    
  fs_ce_UPF_LS/Y (LSUPX1_HVT)                             0.06       0.14 r    1.16
  i_tx_phy/fs_ce (usb_tx_phy)                             0.00       0.14 r    
  i_tx_phy/U6/Y (AND2X1_HVT)                              0.04       0.17 r    1.16
  i_tx_phy/U11/Y (NAND3X0_HVT)                            0.03       0.20 f    1.16
  i_tx_phy/U13/Y (INVX1_HVT)                              0.01       0.22 r    1.16
  i_tx_phy/U18/Y (AND2X1_HVT)                             0.03       0.25 r    1.16
  i_tx_phy/U19/Y (NAND2X0_HVT)                            0.02       0.26 f    1.16
  i_tx_phy/U20/Y (NAND2X0_HVT)                            0.02       0.28 r    1.16
  i_tx_phy/U63/Y (AO222X1_HVT)                            0.05       0.33 r    1.16
  i_tx_phy/one_cnt_reg[2]/D (DFFX1_HVT)                   0.00       0.33 r    1.16
  data arrival time                                                  0.33      

  clock usb_clk (rise edge)                               5.00       5.00      
  clock network delay (ideal)                             0.00       5.00      
  i_tx_phy/one_cnt_reg[2]/CLK (DFFX1_HVT)                 0.00       5.00 r    
  library setup time                                     -0.02       4.98      
  data required time                                                 4.98      
  ------------------------------------------------------------------------------------
  data required time                                                 4.98      
  data arrival time                                                 -0.33      
  ------------------------------------------------------------------------------------
  slack (MET)                                                        4.64      

Power_domain report:

Report : power_domain
Design : usb_phy
Version: R-2020.09-SP5-5
Date   : Mon Mar  2 12:20:15 2026
****************************************

------------------------------------------------------------------------------

 Power Domain             : usb_phy
 Current Scope            : usb_phy
 Elements                 : <top_level>
 Available Supply Nets    : VDD, VSS, VDDL
 Available Supply Sets    : usb_phy.primary, high_voltage, low_voltage, 
			    rx_phy.primary

 Connections                -- Power --             -- Ground --
 Primary:                   VDD                     VSS
   (Max Op. Voltage)         (1.16)                  (0.00)
 Default Isolation:            -                       -
   (Max Op. Voltage)          (-)                     (-)
 Default Retention:            -                       -
   (Max Op. Voltage)          (-)                     (-)

------------------------------------------------------------------------------

 Power Domain             : rx_phy
 Current Scope            : usb_phy
 Elements                 : i_rx_phy
 Available Supply Nets    : VDDL, VSS, VDD
 Available Supply Sets    : rx_phy.primary, low_voltage, high_voltage, 
			    usb_phy.primary

 Connections                -- Power --             -- Ground --
 Primary:                   VDDL                    VSS
   (Max Op. Voltage)         (0.95)                  (0.00)
 Default Isolation:            -                       -
   (Max Op. Voltage)          (-)                     (-)
 Default Retention:            -                       -
   (Max Op. Voltage)          (-)                     (-)

------------------------------------------------------------------------------

report_hierarchy:
****************************************
Report : hierarchy
Design : usb_phy
Version: R-2020.09-SP5-5
Date   : Mon Mar  2 12:23:35 2026
****************************************

Attributes:
    r - licensed design

usb_phy            r
    AND3X1_HVT               saed32hvt_ff1p16v125c
                                                 1.16
    AND4X1_HVT               saed32hvt_ff1p16v125c
                                                 1.16
    AO22X1_HVT               saed32hvt_ff1p16v125c
                                                 1.16
    AO222X1_HVT              saed32hvt_ff1p16v125c
                                                 1.16
    AOI21X1_HVT              saed32hvt_ff1p16v125c
                                                 1.16
    DFFSSRX1_HVT             saed32hvt_ff1p16v125c
                                                 1.16
    DFFX1_HVT                saed32hvt_ff1p16v125c
                                                 1.16
    INVX0_HVT                saed32hvt_ff1p16v125c
                                                 1.16
    INVX1_HVT                saed32hvt_ff1p16v125c
                                                 1.16
    LSUPX1_HVT               saed32hvt_ulvl_ff1p16v125c_i0p95v
                                                 0.95->1.16
    NAND2X0_HVT              saed32hvt_ff1p16v125c
                                                 1.16
    NAND3X0_HVT              saed32hvt_ff1p16v125c
                                                 1.16
    NOR2X0_HVT               saed32hvt_ff1p16v125c
                                                 1.16
    NOR3X0_HVT               saed32hvt_ff1p16v125c
                                                 1.16
    OA222X1_HVT              saed32hvt_ff1p16v125c
                                                 1.16
    OAI22X1_HVT              saed32hvt_ff1p16v125c
                                                 1.16
    usb_rx_phy                         r         0.95
        AND2X1_LVT           saed32lvt_ff0p95v125c
                                                 0.95
        AND3X1_LVT           saed32lvt_ff0p95v125c
                                                 0.95
        AO21X1_LVT           saed32lvt_ff0p95v125c
                                                 0.95
        AO22X1_LVT           saed32lvt_ff0p95v125c
                                                 0.95
        AO222X1_LVT          saed32lvt_ff0p95v125c
                                                 0.95
        AOI21X1_LVT          saed32lvt_ff0p95v125c
                                                 0.95
        DFFX1_LVT            saed32lvt_ff0p95v125c
                                                 0.95
        INVX0_LVT            saed32lvt_ff0p95v125c
                                                 0.95
        INVX1_LVT            saed32lvt_ff0p95v125c
                                                 0.95
        LSDNSSX1_HVT         saed32hvt_dlvl_ff0p95v125c_i1p16v
                                                 0.95->0.95
        MUX21X1_LVT          saed32lvt_ff0p95v125c
                                                 0.95
        NAND2X0_LVT          saed32lvt_ff0p95v125c
                                                 0.95
        NAND3X0_LVT          saed32lvt_ff0p95v125c
                                                 0.95
        NOR2X0_LVT           saed32lvt_ff0p95v125c
                                                 0.95
        NOR3X0_LVT           saed32lvt_ff0p95v125c
                                                 0.95
        NOR4X1_LVT           saed32lvt_ff0p95v125c
                                                 0.95
        OA21X1_LVT           saed32lvt_ff0p95v125c
                                                 0.95
        OA221X1_LVT          saed32lvt_ff0p95v125c
                                                 0.95
        OAI21X1_LVT          saed32lvt_ff0p95v125c
                                                 0.95
        OAI22X1_LVT          saed32lvt_ff0p95v125c
                                                 0.95
        OR2X1_LVT            saed32lvt_ff0p95v125c
                                                 0.95
        OR3X1_LVT            saed32lvt_ff0p95v125c
                                                 0.95
        XNOR2X1_LVT          saed32lvt_ff0p95v125c
                                                 0.95
    usb_tx_phy                         r         1.16
        AND2X1_HVT           saed32hvt_ff1p16v125c
                                                 1.16
        AND3X1_HVT           saed32hvt_ff1p16v125c
                                                 1.16
        AND4X1_HVT           saed32hvt_ff1p16v125c
                                                 1.16
        AO21X1_HVT           saed32hvt_ff1p16v125c
                                                 1.16
        AO22X1_HVT           saed32hvt_ff1p16v125c
                                                 1.16
        AO221X1_HVT          saed32hvt_ff1p16v125c
                                                 1.16
        AO222X1_HVT          saed32hvt_ff1p16v125c
                                                 1.16
        AOI21X1_HVT          saed32hvt_ff1p16v125c
                                                 1.16
        AOI22X1_HVT          saed32hvt_ff1p16v125c
                                                 1.16
        DFFX1_HVT            saed32hvt_ff1p16v125c
                                                 1.16
        HADDX1_HVT           saed32hvt_ff1p16v125c
                                                 1.16
        INVX0_HVT            saed32hvt_ff1p16v125c
                                                 1.16
        INVX1_HVT            saed32hvt_ff1p16v125c
                                                 1.16
        NAND2X0_HVT          saed32hvt_ff1p16v125c
                                                 1.16
        NAND3X0_HVT          saed32hvt_ff1p16v125c
                                                 1.16
        NAND4X0_HVT          saed32hvt_ff1p16v125c
                                                 1.16
        OA21X1_HVT           saed32hvt_ff1p16v125c
                                                 1.16
        OA22X1_HVT           saed32hvt_ff1p16v125c
                                                 1.16
        OA221X1_HVT          saed32hvt_ff1p16v125c
                                                 1.16
        OA222X1_HVT          saed32hvt_ff1p16v125c
                                                 1.16
        OAI21X1_HVT          saed32hvt_ff1p16v125c
                                                 1.16
        OAI22X1_HVT          saed32hvt_ff1p16v125c
                                                 1.16
        OR2X1_HVT            saed32hvt_ff1p16v125c
                                                 1.16
report_port
****************************************
Report : port
Design : usb_phy
Version: R-2020.09-SP5-5
Date   : Mon Mar  2 12:25:18 2026
****************************************


                       Pin      Wire     Max     Max     Connection
Port           Dir     Load     Load     Trans   Cap     Class      Attrs
--------------------------------------------------------------------------------
DataOut_i[0]   in      0.0000   0.0000    0.01   --      --         
DataOut_i[1]   in      0.0000   0.0000    0.01   --      --         
DataOut_i[2]   in      0.0000   0.0000    0.01   --      --         
DataOut_i[3]   in      0.0000   0.0000    0.01   --      --         
DataOut_i[4]   in      0.0000   0.0000    0.01   --      --         
DataOut_i[5]   in      0.0000   0.0000    0.01   --      --         
DataOut_i[6]   in      0.0000   0.0000    0.01   --      --         
DataOut_i[7]   in      0.0000   0.0000    0.01   --      --         
TxValid_i      in      0.0000   0.0000    0.01   --      --         
clk            in      0.0000   0.0000    0.01   --      --         
phy_tx_mode    in      0.0000   0.0000    0.01   --      --         
rst            in      0.0000   0.0000    0.01   --      --         
rxd            in      0.0000   0.0000    0.01   --      --         
rxdn           in      0.0000   0.0000    0.01   --      --         
rxdp           in      0.0000   0.0000    0.01   --      --         
DataIn_o[0]    out     0.0000   0.0000   --     100.00   --         
DataIn_o[1]    out     0.0000   0.0000   --     100.00   --         
DataIn_o[2]    out     0.0000   0.0000   --     100.00   --         
DataIn_o[3]    out     0.0000   0.0000   --     100.00   --         
DataIn_o[4]    out     0.0000   0.0000   --     100.00   --         
DataIn_o[5]    out     0.0000   0.0000   --     100.00   --         
DataIn_o[6]    out     0.0000   0.0000   --     100.00   --         
DataIn_o[7]    out     0.0000   0.0000   --     100.00   --         
LineState_o[0] out     0.0000   0.0000   --     100.00   --         
LineState_o[1] out     0.0000   0.0000   --     100.00   --         
RxActive_o     out     0.0000   0.0000   --     100.00   --         
RxError_o      out     0.0000   0.0000   --     100.00   --         
RxValid_o      out     0.0000   0.0000   --     100.00   --         
TxReady_o      out     0.0000   0.0000   --     100.00   --         
txdn           out     0.0000   0.0000   --     100.00   --         
txdp           out     0.0000   0.0000   --     100.00   --         
txoe           out     0.0000   0.0000   --     100.00   --         
usb_rst        out     0.0000   0.0000   --     100.00   --         

report_cells:
dc_shell> report
1
dc_shell> report_cell
 
****************************************
Report : cell
Design : usb_phy
Version: R-2020.09-SP5-5
Date   : Mon Mar  2 12:27:07 2026
****************************************

Attributes:
    b - black box (unknown)
    h - hierarchical
   lo - local optimization is disabled
    n - noncombinational
    r - removable
   ry - resynthesis is disabled
   st - structuring is off
    u - contains unmapped logic

Cell                    Reference      Library         Area   Attributes  V:T:P
--------------------------------------------------------------------------------
DataIn_o[0]_UPF_LS      LSUPX1_HVT     saed32hvt_ulvl_ff1p16v125c_i0p95v
                                                       7.116032
                                                              lo, ry, st  0.95->1.16:125.00:1.01
DataIn_o[1]_UPF_LS      LSUPX1_HVT     saed32hvt_ulvl_ff1p16v125c_i0p95v
                                                       7.116032
                                                              lo, ry, st  0.95->1.16:125.00:1.01
DataIn_o[2]_UPF_LS      LSUPX1_HVT     saed32hvt_ulvl_ff1p16v125c_i0p95v
                                                       7.116032
                                                              lo, ry, st  0.95->1.16:125.00:1.01
DataIn_o[3]_UPF_LS      LSUPX1_HVT     saed32hvt_ulvl_ff1p16v125c_i0p95v
                                                       7.116032
                                                              lo, ry, st  0.95->1.16:125.00:1.01
DataIn_o[4]_UPF_LS      LSUPX1_HVT     saed32hvt_ulvl_ff1p16v125c_i0p95v
                                                       7.116032
                                                              lo, ry, st  0.95->1.16:125.00:1.01
DataIn_o[5]_UPF_LS      LSUPX1_HVT     saed32hvt_ulvl_ff1p16v125c_i0p95v
                                                       7.116032
                                                              lo, ry, st  0.95->1.16:125.00:1.01
DataIn_o[6]_UPF_LS      LSUPX1_HVT     saed32hvt_ulvl_ff1p16v125c_i0p95v
                                                       7.116032
                                                              lo, ry, st  0.95->1.16:125.00:1.01
DataIn_o[7]_UPF_LS      LSUPX1_HVT     saed32hvt_ulvl_ff1p16v125c_i0p95v
                                                       7.116032
                                                              lo, ry, st  0.95->1.16:125.00:1.01
LineState_o[0]_UPF_LS   LSUPX1_HVT     saed32hvt_ulvl_ff1p16v125c_i0p95v
                                                       7.116032
                                                              lo, ry, st  0.95->1.16:125.00:1.01
LineState_o[1]_UPF_LS   LSUPX1_HVT     saed32hvt_ulvl_ff1p16v125c_i0p95v
                                                       7.116032
                                                              lo, ry, st  0.95->1.16:125.00:1.01
RxActive_o_UPF_LS       LSUPX1_HVT     saed32hvt_ulvl_ff1p16v125c_i0p95v
                                                       7.116032
                                                              lo, ry, st  0.95->1.16:125.00:1.01
RxError_o_UPF_LS        LSUPX1_HVT     saed32hvt_ulvl_ff1p16v125c_i0p95v
                                                       7.116032
                                                              lo, ry, st  0.95->1.16:125.00:1.01
RxValid_o_UPF_LS        LSUPX1_HVT     saed32hvt_ulvl_ff1p16v125c_i0p95v
                                                       7.116032
                                                              lo, ry, st  0.95->1.16:125.00:1.01
U16                     INVX0_HVT      saed32hvt_ff1p16v125c
                                                       1.270720           1.16:125.00:1.01
U17                     INVX1_HVT      saed32hvt_ff1p16v125c
                                                       1.270720           1.16:125.00:1.01
U19                     AND4X1_HVT     saed32hvt_ff1p16v125c
                                                       2.541440           1.16:125.00:1.01
U20                     NOR3X0_HVT     saed32hvt_ff1p16v125c
                                                       2.795584           1.16:125.00:1.01
U21                     NAND3X0_HVT    saed32hvt_ff1p16v125c
                                                       1.779008           1.16:125.00:1.01
U22                     NAND2X0_HVT    saed32hvt_ff1p16v125c
                                                       1.524864           1.16:125.00:1.01
U23                     INVX1_HVT      saed32hvt_ff1p16v125c
                                                       1.270720           1.16:125.00:1.01
U24                     NAND2X0_HVT    saed32hvt_ff1p16v125c
                                                       1.524864           1.16:125.00:1.01
U25                     NAND2X0_HVT    saed32hvt_ff1p16v125c
                                                       1.524864           1.16:125.00:1.01
U26                     NOR2X0_HVT     saed32hvt_ff1p16v125c
                                                       2.541440           1.16:125.00:1.01
U27                     AO22X1_HVT     saed32hvt_ff1p16v125c
                                                       2.541440           1.16:125.00:1.01
U28                     AO22X1_HVT     saed32hvt_ff1p16v125c
                                                       2.541440           1.16:125.00:1.01
U29                     AND3X1_HVT     saed32hvt_ff1p16v125c
                                                       2.287296           1.16:125.00:1.01
U30                     AO222X1_HVT    saed32hvt_ff1p16v125c
                                                       3.303872           1.16:125.00:1.01
U31                     NAND3X0_HVT    saed32hvt_ff1p16v125c
                                                       1.779008           1.16:125.00:1.01
U32                     AOI21X1_HVT    saed32hvt_ff1p16v125c
                                                       3.049728           1.16:125.00:1.01
U33                     NAND2X0_HVT    saed32hvt_ff1p16v125c
                                                       1.524864           1.16:125.00:1.01
U34                     OAI22X1_HVT    saed32hvt_ff1p16v125c
                                                       3.049728           1.16:125.00:1.01
U35                     NAND2X0_HVT    saed32hvt_ff1p16v125c
                                                       1.524864           1.16:125.00:1.01
U36                     OA222X1_HVT    saed32hvt_ff1p16v125c
                                                       3.303872           1.16:125.00:1.01
fs_ce_UPF_LS            LSUPX1_HVT     saed32hvt_ulvl_ff1p16v125c_i0p95v
                                                       7.116032
                                                              lo, ry, st  0.95->1.16:125.00:1.01
i_rx_phy                usb_rx_phy                     562.166541
                                                              h, n        0.95:125.00:1.01
i_tx_phy                usb_tx_phy                     507.017290
                                                              h, n        1.16:125.00:1.01
rst_cnt_reg[0]          DFFX1_HVT      saed32hvt_ff1p16v125c
                                                       6.607744
                                                              n           1.16:125.00:1.01
rst_cnt_reg[1]          DFFX1_HVT      saed32hvt_ff1p16v125c
                                                       6.607744
                                                              n           1.16:125.00:1.01
rst_cnt_reg[2]          DFFX1_HVT      saed32hvt_ff1p16v125c
                                                       6.607744
                                                              n           1.16:125.00:1.01
rst_cnt_reg[3]          DFFX1_HVT      saed32hvt_ff1p16v125c
                                                       6.607744
                                                              n           1.16:125.00:1.01
rst_cnt_reg[4]          DFFX1_HVT      saed32hvt_ff1p16v125c
                                                       6.607744
                                                              n           1.16:125.00:1.01
usb_rst_reg             DFFSSRX1_HVT   saed32hvt_ff1p16v125c
                                                       7.370176
                                                              n           1.16:125.00:1.01
--------------------------------------------------------------------------------
Total 42 cells                                            1252.167514
1

-----
PNR

script tcl
#Design_compiler tools

############### setting the variables for paths ###################

set path_ref { /home/user4/Desktop/projects/SAED_32nm/NDM/}
set my_ref_libs "$path_ref/saed32_1p9m_tech.ndm/ \
                 $path_ref/saed32_hvt.ndm/  \
                 $path_ref/saed32_lvt.ndm/   \
                 $path_ref/saed32_rvt.ndm  \
                 $path_ref/saed32_sram_lp.ndm"

set ref_lib {/home/user4/Desktop/projects/SAED32_EDK/stdcell_hvt/milkyway/saed32nm_hvt_1p9m/
             /home/user4/Desktop/projects/SAED32_EDK/stdcell_lvt/milkyway/saed32nm_lvt_1p9m/
              /home/user4/Desktop/projects/SAED32_EDK/stdcell_rvt/milkyway/saed32nm_rvt_1p9m/}

set tech_libs {/home/user4/Desktop/projects/SAED_32nm/tech/saed32nm_1p9m.tf}

create_lib usb_phy.dlib -technology $tech_libs -ref_lib $my_ref_libs

read_verilog /home/user4/Desktop/usb_phy/synth/outputs/icc2_files/usb_phy.v
-------------------
before planning :usb_para_tcl

## reading_tluplus and nxtgrd#####
read_parasitic_tech -tlup /home/user4/Desktop/projects/proj/ref/tech/saed32nm_1p9m_Cmax.tluplus -layermap /home/user4/Desktop/projects/proj/ref/tech/saed32nm_tf_itf_tluplus.map -name maxTLU
read_parasitic_tech -tlup /home/user4/Desktop/projects/proj/ref/tech/saed32nm_1p9m_Cmin.tluplus -layermap /home/user4/Desktop/projects/proj/ref/tech/saed32nm_tf_itf_tluplus.map -name minTLU

###setting_attr site_defs####
set_attribute [get_site_defs unit] symmetry Y
set_attribute [get_site_defs unit] is_default true

##setting_attr_routing_layers###
set_attribute [get_layers {M1 M3 M5 M7 M9}] routing_direction horizontal
set_attribute [get_layers {M2 M4 M6 M8 MRDL}] routing_direction vertical
set_ignored_layers -max_routing_layer M9

return 1
---------------------------------------------------------------------
#report of pre floorplan checks?

****************************************
Report : design
        -library
        -netlist
        -floorplan
        -routing
Design : usb_phy
Version: R-2020.09-SP2
Date   : Tue Mar  3 06:59:13 2026
****************************************
--------------------------------------------------------------------------------
                              LIBRARY INFORMATION
--------------------------------------------------------------------------------

Search path : .

Units : 
    time                : 1.00ns
    resistance          : 1.00MOhm
    capacitance         : 1.00fF
    voltage             : 1.00V
    current             : 1.00uA
    power               : 1.00pW

Tech file : /home/user4/Desktop/projects/SAED_32nm/tech/saed32nm_1p9m.tf

Number of active scenarios 	= 1
Number of inactive scenarios 	= 0

Total number of standard cells 	= 1025

Total number of dont_use lib cells 	= 80
Total number of dont_touch lib cells 	= 80

Total number of buffers 	= 69
Total number of inverters 	= 45
Total number of flip-flops 	= 306
Total number of latches 	= 36
Total number of ICGs 		= 36


Library : saed32_1p9m_tech
  File path : /home/user4/Desktop/projects/SAED_32nm/NDM/saed32_1p9m_tech.ndm

Library : saed32_hvt|saed32_hvt_std
  File path : /home/user4/Desktop/projects/SAED_32nm/NDM/saed32_hvt.ndm/L0
  Source .db libs :
    saed32hvt_ff0p95vn40c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_ff0p95vn40c.db
    saed32hvt_ff1p16vn40c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_ff1p16vn40c.db
    saed32hvt_ff0p95v125c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_ff0p95v125c.db
    saed32hvt_ff1p16v125c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_ff1p16v125c.db
    saed32hvt_ss0p75vn40c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_ss0p75vn40c.db
    saed32hvt_ss0p95vn40c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_ss0p95vn40c.db
    saed32hvt_ss0p75v125c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_ss0p75v125c.db
    saed32hvt_ss0p95v125c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_ss0p95v125c.db

Library : saed32_hvt|saed32_hvt_lsup
  File path : /home/user4/Desktop/projects/SAED_32nm/NDM/saed32_hvt.ndm/L1
  Source .db libs :
    saed32hvt_ulvl_ff1p16vn40c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_ulvl_ff1p16vn40c_i0p95v.db
    saed32hvt_ulvl_ff1p16v125c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_ulvl_ff1p16v125c_i0p95v.db
    saed32hvt_ulvl_ss0p95vn40c_i0p75v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_ulvl_ss0p95vn40c_i0p75v.db
    saed32hvt_ulvl_ss0p95v125c_i0p75v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_ulvl_ss0p95v125c_i0p75v.db
    saed32hvt_ulvl_ss0p75v125c_i0p75v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_ulvl_ss0p75v125c_i0p75v.db
    saed32hvt_ulvl_ss0p95v125c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_ulvl_ss0p95v125c_i0p95v.db
    saed32hvt_ulvl_ss0p75vn40c_i0p75v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_ulvl_ss0p75vn40c_i0p75v.db
    saed32hvt_ulvl_ss0p95vn40c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_ulvl_ss0p95vn40c_i0p95v.db

Library : saed32_hvt|saed32_hvt_lsdn
  File path : /home/user4/Desktop/projects/SAED_32nm/NDM/saed32_hvt.ndm/L2
  Source .db libs :
    saed32hvt_dlvl_ff0p95vn40c_i1p16v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_dlvl_ff0p95vn40c_i1p16v.db
    saed32hvt_dlvl_ff0p95v125c_i1p16v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_dlvl_ff0p95v125c_i1p16v.db
    saed32hvt_dlvl_ss0p75vn40c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_dlvl_ss0p75vn40c_i0p95v.db
    saed32hvt_dlvl_ss0p75v125c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_dlvl_ss0p75v125c_i0p95v.db
    saed32hvt_dlvl_ss0p75v125c_i0p75v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_dlvl_ss0p75v125c_i0p75v.db
    saed32hvt_dlvl_ss0p95v125c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_dlvl_ss0p95v125c_i0p95v.db
    saed32hvt_dlvl_ss0p75vn40c_i0p75v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_dlvl_ss0p75vn40c_i0p75v.db
    saed32hvt_dlvl_ss0p95vn40c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_dlvl_ss0p95vn40c_i0p95v.db

Library : saed32_hvt|saed32_hvt_pg
  File path : /home/user4/Desktop/projects/SAED_32nm/NDM/saed32_hvt.ndm/L3
  Source .db libs :
    saed32hvt_pg_ff0p95v125c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_pg_ff0p95v125c.db
    saed32hvt_pg_ff0p95vn40c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_pg_ff0p95vn40c.db
    saed32hvt_pg_ff1p16v125c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_pg_ff1p16v125c.db
    saed32hvt_pg_ff1p16vn40c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_pg_ff1p16vn40c.db
    saed32hvt_pg_ss0p75vn40c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_pg_ss0p75vn40c.db
    saed32hvt_pg_ss0p95vn40c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_pg_ss0p95vn40c.db
    saed32hvt_pg_ss0p75v125c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_pg_ss0p75v125c.db
    saed32hvt_pg_ss0p95v125c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_hvt/db_nldm/saed32hvt_pg_ss0p95v125c.db

Library : saed32_lvt|saed32_lvt_std
  File path : /home/user4/Desktop/projects/SAED_32nm/NDM/saed32_lvt.ndm/L0
  Source .db libs :
    saed32lvt_ff0p95vn40c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_ff0p95vn40c.db
    saed32lvt_ff1p16vn40c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_ff1p16vn40c.db
    saed32lvt_ff0p95v125c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_ff0p95v125c.db
    saed32lvt_ff1p16v125c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_ff1p16v125c.db
    saed32lvt_ss0p75vn40c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_ss0p75vn40c.db
    saed32lvt_ss0p95vn40c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_ss0p95vn40c.db
    saed32lvt_ss0p75v125c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_ss0p75v125c.db
    saed32lvt_ss0p95v125c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_ss0p95v125c.db

Library : saed32_lvt|saed32_lvt_lsup
  File path : /home/user4/Desktop/projects/SAED_32nm/NDM/saed32_lvt.ndm/L1
  Source .db libs :
    saed32lvt_ulvl_ff1p16vn40c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_ulvl_ff1p16vn40c_i0p95v.db
    saed32lvt_ulvl_ff1p16v125c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_ulvl_ff1p16v125c_i0p95v.db
    saed32lvt_ulvl_ss0p95vn40c_i0p75v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_ulvl_ss0p95vn40c_i0p75v.db
    saed32lvt_ulvl_ss0p95v125c_i0p75v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_ulvl_ss0p95v125c_i0p75v.db
    saed32lvt_ulvl_ss0p75v125c_i0p75v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_ulvl_ss0p75v125c_i0p75v.db
    saed32lvt_ulvl_ss0p95v125c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_ulvl_ss0p95v125c_i0p95v.db
    saed32lvt_ulvl_ss0p75vn40c_i0p75v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_ulvl_ss0p75vn40c_i0p75v.db
    saed32lvt_ulvl_ss0p95vn40c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_ulvl_ss0p95vn40c_i0p95v.db

Library : saed32_lvt|saed32_lvt_lsdn
  File path : /home/user4/Desktop/projects/SAED_32nm/NDM/saed32_lvt.ndm/L2
  Source .db libs :
    saed32lvt_dlvl_ff0p95vn40c_i1p16v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_dlvl_ff0p95vn40c_i1p16v.db
    saed32lvt_dlvl_ff0p95v125c_i1p16v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_dlvl_ff0p95v125c_i1p16v.db
    saed32lvt_dlvl_ss0p75vn40c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_dlvl_ss0p75vn40c_i0p95v.db
    saed32lvt_dlvl_ss0p75v125c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_dlvl_ss0p75v125c_i0p95v.db
    saed32lvt_dlvl_ss0p75v125c_i0p75v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_dlvl_ss0p75v125c_i0p75v.db
    saed32lvt_dlvl_ss0p95v125c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_dlvl_ss0p95v125c_i0p95v.db

Library : saed32_lvt|saed32_lvt_pg
  File path : /home/user4/Desktop/projects/SAED_32nm/NDM/saed32_lvt.ndm/L3
  Source .db libs :
    saed32lvt_pg_ff0p95v125c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_pg_ff0p95v125c.db
    saed32lvt_pg_ff0p95vn40c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_pg_ff0p95vn40c.db
    saed32lvt_pg_ff1p16v125c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_pg_ff1p16v125c.db
    saed32lvt_pg_ff1p16vn40c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_pg_ff1p16vn40c.db
    saed32lvt_pg_ss0p75v125c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_pg_ss0p75v125c.db
    saed32lvt_pg_ss0p75vn40c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_pg_ss0p75vn40c.db
    saed32lvt_pg_ss0p95v125c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_pg_ss0p95v125c.db
    saed32lvt_pg_ss0p95vn40c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_lvt/db_nldm/saed32lvt_pg_ss0p95vn40c.db

Library : saed32_rvt|saed32_rvt_pg
  File path : /home/user4/Desktop/projects/SAED_32nm/NDM/saed32_rvt.ndm/L0
  Source .db libs :
    saed32rvt_pg_ff0p95v125c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_pg_ff0p95v125c.db
    saed32rvt_pg_ff0p95vn40c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_pg_ff0p95vn40c.db
    saed32rvt_pg_ff1p16v125c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_pg_ff1p16v125c.db
    saed32rvt_pg_ff1p16vn40c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_pg_ff1p16vn40c.db
    saed32rvt_pg_ss0p75v125c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_pg_ss0p75v125c.db
    saed32rvt_pg_ss0p75vn40c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_pg_ss0p75vn40c.db
    saed32rvt_pg_ss0p95v125c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_pg_ss0p95v125c.db
    saed32rvt_pg_ss0p95vn40c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_pg_ss0p95vn40c.db

Library : saed32_rvt|saed32_rvt_std
  File path : /home/user4/Desktop/projects/SAED_32nm/NDM/saed32_rvt.ndm/L1
  Source .db libs :
    saed32rvt_ff1p16vn40c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_ff1p16vn40c.db
    saed32rvt_ff1p16v125c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_ff1p16v125c.db
    saed32rvt_ff0p95v125c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_ff0p95v125c.db
    saed32rvt_ff0p95vn40c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_ff0p95vn40c.db
    saed32rvt_ss0p95vn40c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_ss0p95vn40c.db
    saed32rvt_ss0p95v125c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_ss0p95v125c.db
    saed32rvt_ss0p75v125c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_ss0p75v125c.db
    saed32rvt_ss0p75vn40c : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_ss0p75vn40c.db

Library : saed32_rvt|saed32_rvt_lsdn
  File path : /home/user4/Desktop/projects/SAED_32nm/NDM/saed32_rvt.ndm/L2
  Source .db libs :
    saed32rvt_dlvl_ff0p95v125c_i1p16v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_dlvl_ff0p95v125c_i1p16v.db
    saed32rvt_dlvl_ff0p95vn40c_i1p16v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_dlvl_ff0p95vn40c_i1p16v.db
    saed32rvt_dlvl_ss0p75v125c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_dlvl_ss0p75v125c_i0p95v.db
    saed32rvt_dlvl_ss0p75vn40c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_dlvl_ss0p75vn40c_i0p95v.db
    saed32rvt_dlvl_ss0p75v125c_i0p75v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_dlvl_ss0p75v125c_i0p75v.db
    saed32rvt_dlvl_ss0p95v125c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_dlvl_ss0p95v125c_i0p95v.db
    saed32rvt_dlvl_ss0p75vn40c_i0p75v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_dlvl_ss0p75vn40c_i0p75v.db
    saed32rvt_dlvl_ss0p95vn40c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_dlvl_ss0p95vn40c_i0p95v.db

Library : saed32_rvt|saed32_rvt_lsup
  File path : /home/user4/Desktop/projects/SAED_32nm/NDM/saed32_rvt.ndm/L3
  Source .db libs :
    saed32rvt_ulvl_ff1p16v125c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_ulvl_ff1p16v125c_i0p95v.db
    saed32rvt_ulvl_ff1p16vn40c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_ulvl_ff1p16vn40c_i0p95v.db
    saed32rvt_ulvl_ss0p95v125c_i0p75v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_ulvl_ss0p95v125c_i0p75v.db
    saed32rvt_ulvl_ss0p95vn40c_i0p75v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_ulvl_ss0p95vn40c_i0p75v.db
    saed32rvt_ulvl_ss0p75v125c_i0p75v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_ulvl_ss0p75v125c_i0p75v.db
    saed32rvt_ulvl_ss0p95v125c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_ulvl_ss0p95v125c_i0p95v.db
    saed32rvt_ulvl_ss0p75vn40c_i0p75v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_ulvl_ss0p75vn40c_i0p75v.db
    saed32rvt_ulvl_ss0p95vn40c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/stdcell_rvt/db_nldm/saed32rvt_ulvl_ss0p95vn40c_i0p95v.db

Library : saed32_sram_lp
  File path : /home/user4/Desktop/projects/SAED_32nm/NDM/saed32_sram_lp.ndm
  Source .db libs :
    saed32sramlp_ss0p75vn40c_i0p75v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/sram_lp/db_nldm/saed32sramlp_ss0p75vn40c_i0p75v.db
    saed32sramlp_ss0p75v125c_i0p75v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/sram_lp/db_nldm/saed32sramlp_ss0p75v125c_i0p75v.db
    saed32sramlp_ss0p95v125c_i0p75v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/sram_lp/db_nldm/saed32sramlp_ss0p95v125c_i0p75v.db
    saed32sramlp_ss0p95v125c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/sram_lp/db_nldm/saed32sramlp_ss0p95v125c_i0p95v.db
    saed32sramlp_ss0p95vn40c_i0p75v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/sram_lp/db_nldm/saed32sramlp_ss0p95vn40c_i0p75v.db
    saed32sramlp_ss0p95vn40c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/sram_lp/db_nldm/saed32sramlp_ss0p95vn40c_i0p95v.db
    saed32sramlp_ff0p95v125c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/sram_lp/db_nldm/saed32sramlp_ff0p95v125c_i0p95v.db
    saed32sramlp_ff0p95vn40c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/sram_lp/db_nldm/saed32sramlp_ff0p95vn40c_i0p95v.db
    saed32sramlp_ff1p16v125c_i0p85v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/sram_lp/db_nldm/saed32sramlp_ff1p16v125c_i0p85v.db
    saed32sramlp_ff1p16v125c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/sram_lp/db_nldm/saed32sramlp_ff1p16v125c_i0p95v.db
    saed32sramlp_ff1p16v125c_i1p16v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/sram_lp/db_nldm/saed32sramlp_ff1p16v125c_i1p16v.db
    saed32sramlp_ff1p16vn40c_i0p85v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/sram_lp/db_nldm/saed32sramlp_ff1p16vn40c_i0p85v.db
    saed32sramlp_ff1p16vn40c_i0p95v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/sram_lp/db_nldm/saed32sramlp_ff1p16vn40c_i0p95v.db
    saed32sramlp_ff1p16vn40c_i1p16v : /global/gtsna_training/projects/IC_Compiler_II_Implementation/dev_2018.06/ref/SAED32_2012-12-25/lib/sram_lp/db_nldm/saed32sramlp_ff1p16vn40c_i1p16v.db

--------------------------------------------------------------------------------
                              NETLIST INFORMATION
--------------------------------------------------------------------------------

CELL INSTANCE INFORMATION
-----------------------------------------------------------------------------
Cell Instance Type          Count % of         Area  % of siteAreaPerSite
                                  total             total
-----------------------------------------------------------------------------
TOTAL LEAF CELLS              332  100     1252.167   100 unit:4927  

  Standard cells              332  100     1252.167   100 unit:4927  
  Filler cells                  0    0        0.000     0 
  Diode cells                   0    0        0.000     0 
  Hard macro cells              0    0        0.000     0 
  Soft macro cells              0    0        0.000     0 
    Black box cells             0    0        0.000     0 
  Analog block cells            0    0        0.000     0 
  Pad cells                     0    0        0.000     0 
    Flip-chip pad cells         0    0        0.000     0 
  Cover cells                   0    0        0.000     0 
  Flip-chip driver cells        0    0        0.000     0 
  Corner pad cells              0    0        0.000     0 
  Pad spacer cells              0    0        0.000     0 
  Others                        0    0        0.000     0 

Special cells                  20    6      110.298     8 unit:434  
  Level shifter                20    6      110.298     8 unit:434  
  Isolation                     0    0        0.000     0 
  Switch                        0    0        0.000     0 
  Always on                     0    0        0.000     0 
  Retention                     0    0        0.000     0 
  Tie off                       0    0        0.000     0 

LVT                             0    0        0.000     0 
HVT                             0    0        0.000     0 
Normal VT                       0    0        0.000     0 
Others                        332  100     1252.167   100 unit:4927  

Netlist cells                 332  100     1252.167   100 unit:4927  
Physical only                   0    0        0.000     0 

Fixed cells                     0    0        0.000     0 
Moveable cells                332  100     1252.167   100 unit:4927  

Combinational                 234   70      603.846    48 unit:2376  
Sequential                     98   29      648.321    51 unit:2551  
Others                          0    0        0.000     0 

Buffer                         20    6      110.298     8 unit:434  
Inverter                       25    7       31.768     2 unit:125  
Buffer/inverter                45   13      142.066    11 unit:559  

Spare cells                     0    0        0.000     0 
ICG cells                       0    0        0.000     0 
Flip-flop cells                98   29      648.321    51 unit:2551  
Latch cells                     0    0        0.000     0 
Antenna cells                   0    0        0.000     0 

Mux logic                      10    3       33.039     2 unit:130  
Double height                  14    4       99.624     7 unit:392  
Triple height                   0    0        0.000     0 
More than triple height         0    0        0.000     0 

Logic Hierarchies          2

REFERENCE DESIGN INFORMATION
----------------------------------------------------------------------------------------------
Number of reference designs used:50   
----------------------------------------------------------------------------------------------
Name           Type          Count     Width    Height        Area   PinDens SiteName siteArea
----------------------------------------------------------------------------------------------
DFFX1_HVT      lib_cell         50      3.95      1.67       6.608     0.908 unit           26
DFFX1_LVT      lib_cell         47      3.95      1.67       6.608     0.908 unit           26
NAND2X0_HVT    lib_cell         19      0.91      1.67       1.525     3.279 unit            6
AO22X1_HVT     lib_cell         19      1.52      1.67       2.541     2.754 unit           10
LSUPX1_HVT     lib_cell         14      2.13      3.34       7.116     0.703 unit           28
AO22X1_LVT     lib_cell         12      1.52      1.67       2.541     2.754 unit           10
OR2X1_LVT      lib_cell         11      1.22      1.67       2.033     2.459 unit            8
MUX21X1_LVT    lib_cell         10      1.98      1.67       3.304     1.816 unit           13
AND2X1_LVT     lib_cell          9      1.22      1.67       2.033     2.459 unit            8
NOR2X0_LVT     lib_cell          8      1.52      1.67       2.541     1.967 unit           10
INVX1_LVT      lib_cell          8      0.76      1.67       1.271     3.148 unit            5
NAND2X0_LVT    lib_cell          8      0.91      1.67       1.525     3.279 unit            6
AND2X1_HVT     lib_cell          8      1.22      1.67       2.033     2.459 unit            8
INVX1_HVT      lib_cell          8      0.76      1.67       1.271     3.148 unit            5
OA221X1_HVT    lib_cell          7      1.82      1.67       3.050     2.623 unit           12
INVX0_LVT      lib_cell          6      0.76      1.67       1.271     3.148 unit            5
LSDNSSX1_HVT   lib_cell          6      1.06      1.67       1.779     2.248 unit            7
NAND3X0_HVT    lib_cell          6      1.06      1.67       1.779     3.373 unit            7
AO222X1_HVT    lib_cell          5      1.98      1.67       3.304     2.724 unit           13
NAND3X0_LVT    lib_cell          5      1.06      1.67       1.779     3.373 unit            7
OAI22X1_HVT    lib_cell          4      1.82      1.67       3.050     2.295 unit           12
AO222X1_LVT    lib_cell          4      1.98      1.67       3.304     2.724 unit           13
NAND4X0_HVT    lib_cell          4      1.22      1.67       2.033     3.443 unit            8
AOI21X1_LVT    lib_cell          4      1.82      1.67       3.050     1.967 unit           12
AO221X1_HVT    lib_cell          4      1.82      1.67       3.050     2.623 unit           12
OA21X1_HVT     lib_cell          4      1.52      1.67       2.541     2.361 unit           10
AND3X1_HVT     lib_cell          4      1.37      1.67       2.287     2.623 unit            9
AO21X1_LVT     lib_cell          3      1.52      1.67       2.541     2.361 unit           10
AND4X1_HVT     lib_cell          3      1.52      1.67       2.541     2.754 unit           10
INVX0_HVT      lib_cell          3      0.76      1.67       1.271     3.148 unit            5
NOR4X1_LVT     lib_cell          2      1.82      1.67       3.050     2.295 unit           12
OA21X1_LVT     lib_cell          2      1.52      1.67       2.541     2.361 unit           10
NOR3X0_LVT     lib_cell          2      1.67      1.67       2.796     2.146 unit           11
AND3X1_LVT     lib_cell          2      1.37      1.67       2.287     2.623 unit            9
AO21X1_HVT     lib_cell          2      1.52      1.67       2.541     2.361 unit           10
XNOR2X1_LVT    lib_cell          2      2.58      1.67       4.320     1.157 unit           17
OAI21X1_LVT    lib_cell          2      1.82      1.67       3.050     1.967 unit           12
OA222X1_HVT    lib_cell          2      1.98      1.67       3.304     2.724 unit           13
AOI21X1_HVT    lib_cell          2      1.82      1.67       3.050     1.967 unit           12
HADDX1_HVT     lib_cell          1      1.98      1.67       3.304     1.816 unit           13
OA22X1_HVT     lib_cell          1      1.52      1.67       2.541     2.754 unit           10
AOI22X1_HVT    lib_cell          1      1.82      1.67       3.050     2.295 unit           12
OR2X1_HVT      lib_cell          1      1.22      1.67       2.033     2.459 unit            8
OAI21X1_HVT    lib_cell          1      1.82      1.67       3.050     1.967 unit           12
NOR2X0_HVT     lib_cell          1      1.52      1.67       2.541     1.967 unit           10
OAI22X1_LVT    lib_cell          1      1.82      1.67       3.050     2.295 unit           12
OR3X1_LVT      lib_cell          1      1.37      1.67       2.287     2.623 unit            9
NOR3X0_HVT     lib_cell          1      1.67      1.67       2.796     2.146 unit           11
DFFSSRX1_HVT   lib_cell          1      4.41      1.67       7.370     1.085 unit           29
OA221X1_LVT    lib_cell          1      1.82      1.67       3.050     2.623 unit           12

NET INFORMATION
------------------------------------------------------------------
NetType                 Count FloatingNets         Vias Nets/Cells
------------------------------------------------------------------
Total                     386            1            0      1.163
Signal                    385            0            0      1.160
Power                       0            0            0      0.000
Ground                      0            0            0      0.000
Analog Signal               0            0            0      0.000
Analog Ground               0            0            0      0.000
Analog Power                0            0            0      0.000
Clock                       0            0            0      0.000
Tie Low                     0            0            0      0.000
Tie High                    1            1            0      0.003
Others                      0            0            0      0.000

NET FANOUT AND PIN COUNT INFORMATION
---------------------------------------------------
Fanout        Netcount     netPinCount     NetCount
---------------------------------------------------
<2                 221     <2                     1
2                   86     2                    220
3                   35     3                     86
4                    9     4                     35
5                   10     5                      9
6-10                21     6-10                  28
11-20                2     11-20                  5
21-30                0     21-30                  0
31-50                1     31-50                  1
51-100               1     51-100                 1
101-500              0     101-500                0
501-1000             0     501-1000               0
>1000                0     >1000                  0

PORT AND PIN INFORMATION
------------------------------------------------------------------------------
Type         Total     Input    Output     Inout      3-st     Power    Ground
------------------------------------------------------------------------------
Total         1955      1524       431         0         0       346       332
Macro            0         0         0         0         0         0         0
Ports           33        15        18         0         0         0         0
------------------------------------------------------------------------------

--------------------------------------------------------------------------------
                              FLOORPLAN INFORMATION
--------------------------------------------------------------------------------

CORE AND CHIP AREA INFORMATION
---------------------------
Core Area is :        0.000
Chip Area is :        0.000
---------------------------

SITE ROW INFORMATION
-----------------------------------------------------------------------
Site Name    Width    Height   Total Rows     Total Tiles          Area
-----------------------------------------------------------------------
-----------------------------------------------------------------------

BLOCKAGE INFORMATION
------------------------------------------------
Blockage Type                Count          Area
------------------------------------------------
Hard placement                   0         0.000
Soft placement                   0         0.000
Hard macro                       0         0.000
Partial placement                0         0.000
Register                         0         0.000
Placement allow Buffer Only      0         0.000
Placement allow RP Group Only
                                 0         0.000
RP Group                         0         0.000
Category                         0         0.000
Routing                          0         0.000
Routing for Top                  0         0.000
Routing For Design Rule          0         0.000
Shaping                          0         0.000
------------------------------------------------

POWER DOMAIN INFORMATION
--------------------------------------------------------------------
Power Domain Name    VA Name         Primary Power Net Primary Ground Net
--------------------------------------------------------------------
DEFAULT_POWER_DOMAIN DEFAULT_VA      VDD               VSS
--------------------------------------------------------------------

VOLTAGE AREA INFORMATION
-------------------------------------------------------------------------------------
VA Name          Number             Area       Target    bbox    bbox    bbox    bbox
              of shapes                   Utilization     llx     lly     urx     ury
-------------------------------------------------------------------------------------
DEFAULT_VA            1            0.000         1.00    0.00    0.00    0.00    0.00
-------------------------------------------------------------------------------------

GROUP BOUND INFORMATION
----------------------------------------
No Group Bound exists

EXCLUSIVE MOVEBOUND INFORMATION
----------------------------------------
No Exclusive MoveBound exists.

HARD AND SOFT MOVEBOUND INFORMATION
----------------------------------------
No Hard Or Soft MoveBound exists.

ROUTE GUIDE INFORMATION
------------------------------------------------
Route Guide Type             Count          Area
------------------------------------------------
Extra Detour Region              0         0.000
Over icovl CellLayers            0         0.000
River Routing                    0         0.000
Area Double Via                  0         0.000
Access Preference                0         0.000
Default                          0         0.000
Switched Direction Only          0         0.000
Max Patterns                     0         0.000
Others                           0         0.000
------------------------------------------------

MULTIBIT REGISTER INFORMATION
-----------------------------------------------
No Multibit cells exist

MULTIBIT LS/ISO CELLS INFORMATION
-----------------------------------------------
No Multibit cells exist

RP GROUP INFORMATION
----------------------------------------
No RP Group exists

LAYER INFORMATION
--------------------------------------------------------------------------
Layer Name Direction Ignored Pitch  default minWidth minSpacing    sameNet
                                      Width                     MinSpacing
--------------------------------------------------------------------------
M1         Hor       NO       0.15     0.05     0.05       0.05       0.00
M2         Ver       NO       0.15     0.06     0.06       0.06       0.06
M3         Hor       NO       0.30     0.06     0.06       0.06       0.06
M4         Ver       NO       0.30     0.06     0.06       0.06       0.06
M5         Hor       NO       0.61     0.06     0.06       0.06       0.06
M6         Ver       NO       0.61     0.06     0.06       0.06       0.06
M7         Hor       NO       1.22     0.06     0.06       0.06       0.06
M8         Ver       NO       1.22     0.06     0.06       0.06       0.06
M9         Hor       NO       2.43     0.16     0.16       0.16       0.00
MRDL       Ver       YES      4.86     2.00     2.00       2.00       0.00
--------------------------------------------------------------------------
--------------------------------------------------------------------------------
                              ROUTING INFORMATION
--------------------------------------------------------------------------------

Total wire length = 0.00 micron
Total number of wires = 0
Total number of contacts = 0

FINAL WIRING STATISTICS

Signal Wiring Statistics

Metal layer     Num wires  % of total#  Wire length % of total length
PO                      0        0.00%         0.00             0.00%
M1                      0        0.00%         0.00             0.00%
M2                      0        0.00%         0.00             0.00%
M3                      0        0.00%         0.00             0.00%
M4                      0        0.00%         0.00             0.00%
M5                      0        0.00%         0.00             0.00%
M6                      0        0.00%         0.00             0.00%
M7                      0        0.00%         0.00             0.00%
M8                      0        0.00%         0.00             0.00%
M9                      0        0.00%         0.00             0.00%
MRDL                    0        0.00%         0.00             0.00%

Clock Wiring Statistics

Metal layer     Num wires  % of total#  Wire length % of total length
PO                      0        0.00%         0.00             0.00%
M1                      0        0.00%         0.00             0.00%
M2                      0        0.00%         0.00             0.00%
M3                      0        0.00%         0.00             0.00%
M4                      0        0.00%         0.00             0.00%
M5                      0        0.00%         0.00             0.00%
M6                      0        0.00%         0.00             0.00%
M7                      0        0.00%         0.00             0.00%
M8                      0        0.00%         0.00             0.00%
M9                      0        0.00%         0.00             0.00%
MRDL                    0        0.00%         0.00             0.00%

P/G Wiring Statistics

Metal layer     Num wires  % of total#  Wire length % of total length
PO                      0        0.00%         0.00             0.00%
M1                      0        0.00%         0.00             0.00%
M2                      0        0.00%         0.00             0.00%
M3                      0        0.00%         0.00             0.00%
M4                      0        0.00%         0.00             0.00%
M5                      0        0.00%         0.00             0.00%
M6                      0        0.00%         0.00             0.00%
M7                      0        0.00%         0.00             0.00%
M8                      0        0.00%         0.00             0.00%
M9                      0        0.00%         0.00             0.00%
MRDL                    0        0.00%         0.00             0.00%

Shape Pattern Wiring Statistics

Metal layer     Num shapePatterns
                           % of total#  Wire length % of total length
PO                      0        0.00%         0.00             0.00%
M1                      0        0.00%         0.00             0.00%
M2                      0        0.00%         0.00             0.00%
M3                      0        0.00%         0.00             0.00%
M4                      0        0.00%         0.00             0.00%
M5                      0        0.00%         0.00             0.00%
M6                      0        0.00%         0.00             0.00%
M7                      0        0.00%         0.00             0.00%
M8                      0        0.00%         0.00             0.00%
M9                      0        0.00%         0.00             0.00%
MRDL                    0        0.00%         0.00             0.00%

All the PG shape patterns stand for 0 shapes.

Horizontal/Vertical Wire Distribution

Metal layer  Hor. length  % of hor.    Ver. length  % of ver.
PO                   0.00        0.00%         0.00        0.00%
M1                   0.00        0.00%         0.00        0.00%
M2                   0.00        0.00%         0.00        0.00%
M3                   0.00        0.00%         0.00        0.00%
M4                   0.00        0.00%         0.00        0.00%
M5                   0.00        0.00%         0.00        0.00%
M6                   0.00        0.00%         0.00        0.00%
M7                   0.00        0.00%         0.00        0.00%
M8                   0.00        0.00%         0.00        0.00%
M9                   0.00        0.00%         0.00        0.00%
MRDL                 0.00        0.00%         0.00        0.00%

FINAL VIA STATISTICS

No vias found in the design.

FINAL DRC STATISTICS


#for the timing check:
Information: Timer using 1 threads
  ----------------------------------------------------------------------------------------------------
  Rule         Type   Count      Message
  ----------------------------------------------------------------------------------------------------
  TCK-001      Warn   117        The reported endpoint '%endpoint' is unconstrained. Reason: '%re...
  TCK-002      Warn   98         The register clock pin '%pin' has no fanin clocks. Mode:'%mode'.
  ----------------------------------------------------------------------------------------------------
Total messages: 215, Errors: 0, Warnings: 215, Info: 0


Messages
--------
Warning: The register clock pin 'i_tx_phy/state_reg[0]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/append_eop_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/append_eop_sync1_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/append_eop_sync2_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/append_eop_sync3_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/append_eop_sync4_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/tx_ip_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/data_done_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/tx_ip_sync_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/txoe_r1_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/txoe_r2_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/txoe_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/bit_cnt_reg[0]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/bit_cnt_reg[1]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/bit_cnt_reg[2]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/sd_raw_o_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/sd_bs_o_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/sd_nrzi_o_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/txdn_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/txdp_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/one_cnt_reg[0]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/one_cnt_reg[1]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/one_cnt_reg[2]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/sft_done_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/sft_done_r_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/state_reg[2]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/ld_data_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/TxReady_o_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/state_reg[1]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/hold_reg_reg[7]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/hold_reg_d_reg[7]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/hold_reg_reg[6]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/hold_reg_d_reg[6]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/hold_reg_reg[5]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/hold_reg_d_reg[5]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/hold_reg_reg[4]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/hold_reg_d_reg[4]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/hold_reg_reg[3]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/hold_reg_d_reg[3]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/hold_reg_reg[2]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/hold_reg_d_reg[2]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/hold_reg_reg[1]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/hold_reg_d_reg[1]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/hold_reg_reg[0]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_tx_phy/hold_reg_d_reg[0]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/rx_en_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/rxd_s0_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/rxd_s1_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/rxd_s_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/rxdp_s0_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/rxdp_s1_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/rxdp_s_r_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/rxdp_s_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/rxdn_s0_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/rxdn_s1_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/rxdn_s_r_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/rxdn_s_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/rxd_r_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/dpll_state_reg[0]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/dpll_state_reg[1]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/fs_ce_r1_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/fs_ce_r2_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/rx_valid_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/rx_valid_r_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/rx_active_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/fs_state_reg[0]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/fs_state_reg[1]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/fs_state_reg[2]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/sync_err_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/shift_en_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/sd_nrzi_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/one_cnt_reg[2]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/bit_stuff_err_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/one_cnt_reg[0]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/one_cnt_reg[1]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/bit_cnt_reg[0]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/bit_cnt_reg[1]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/bit_cnt_reg[2]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/rx_valid1_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/se0_r_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/byte_err_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/se0_s_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/sd_r_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/hold_reg_reg[7]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/hold_reg_reg[6]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/hold_reg_reg[5]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/hold_reg_reg[4]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/hold_reg_reg[3]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/hold_reg_reg[2]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/hold_reg_reg[1]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/hold_reg_reg[0]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'i_rx_phy/fs_ce_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'rst_cnt_reg[1]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'rst_cnt_reg[0]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'rst_cnt_reg[2]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'rst_cnt_reg[3]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'rst_cnt_reg[4]/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The register clock pin 'usb_rst_reg/CLK' has no fanin clocks. Mode:'default'. (TCK-002)
Warning: The reported endpoint 'usb_rst' is unconstrained. Reason: 'no check'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'txdp' is unconstrained. Reason: 'no check'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'txdn' is unconstrained. Reason: 'no check'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'txoe' is unconstrained. Reason: 'no check'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'TxReady_o' is unconstrained. Reason: 'no check'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'RxValid_o' is unconstrained. Reason: 'no check'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'RxActive_o' is unconstrained. Reason: 'no check'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'RxError_o' is unconstrained. Reason: 'no check'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'DataIn_o[7]' is unconstrained. Reason: 'no check'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'DataIn_o[6]' is unconstrained. Reason: 'no check'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'DataIn_o[5]' is unconstrained. Reason: 'no check'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'DataIn_o[4]' is unconstrained. Reason: 'no check'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'DataIn_o[3]' is unconstrained. Reason: 'no check'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'DataIn_o[2]' is unconstrained. Reason: 'no check'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'DataIn_o[1]' is unconstrained. Reason: 'no check'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'DataIn_o[0]' is unconstrained. Reason: 'no check'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'LineState_o[1]' is unconstrained. Reason: 'no check'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'LineState_o[0]' is unconstrained. Reason: 'no check'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/state_reg[0]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/append_eop_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/append_eop_sync1_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/append_eop_sync2_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/append_eop_sync3_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/append_eop_sync4_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/tx_ip_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/data_done_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/tx_ip_sync_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/txoe_r1_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/txoe_r2_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/txoe_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/bit_cnt_reg[0]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/bit_cnt_reg[1]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/bit_cnt_reg[2]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/sd_raw_o_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/sd_bs_o_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/sd_nrzi_o_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/txdn_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/txdp_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/one_cnt_reg[0]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/one_cnt_reg[1]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/one_cnt_reg[2]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/sft_done_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/sft_done_r_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/state_reg[2]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/ld_data_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/TxReady_o_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/state_reg[1]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/hold_reg_reg[7]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/hold_reg_d_reg[7]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/hold_reg_reg[6]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/hold_reg_d_reg[6]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/hold_reg_reg[5]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/hold_reg_d_reg[5]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/hold_reg_reg[4]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/hold_reg_d_reg[4]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/hold_reg_reg[3]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/hold_reg_d_reg[3]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/hold_reg_reg[2]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/hold_reg_d_reg[2]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/hold_reg_reg[1]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/hold_reg_d_reg[1]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/hold_reg_reg[0]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_tx_phy/hold_reg_d_reg[0]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/rx_en_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/rxd_s0_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/rxd_s1_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/rxd_s_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/rxdp_s0_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/rxdp_s1_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/rxdp_s_r_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/rxdp_s_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/rxdn_s0_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/rxdn_s1_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/rxdn_s_r_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/rxdn_s_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/rxd_r_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/dpll_state_reg[0]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/dpll_state_reg[1]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/fs_ce_r1_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/fs_ce_r2_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/rx_valid_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/rx_valid_r_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/rx_active_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/fs_state_reg[0]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/fs_state_reg[1]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/fs_state_reg[2]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/sync_err_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/shift_en_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/sd_nrzi_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/one_cnt_reg[2]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/bit_stuff_err_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/one_cnt_reg[0]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/one_cnt_reg[1]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/bit_cnt_reg[0]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/bit_cnt_reg[1]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/bit_cnt_reg[2]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/rx_valid1_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/se0_r_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/byte_err_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/se0_s_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/sd_r_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/hold_reg_reg[7]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/hold_reg_reg[6]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/hold_reg_reg[5]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/hold_reg_reg[4]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/hold_reg_reg[3]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/hold_reg_reg[2]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/hold_reg_reg[1]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/hold_reg_reg[0]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'i_rx_phy/fs_ce_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'rst_cnt_reg[1]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'rst_cnt_reg[0]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'rst_cnt_reg[2]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'rst_cnt_reg[3]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'rst_cnt_reg[4]/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'usb_rst_reg/D' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
Warning: The reported endpoint 'usb_rst_reg/RSTB' is unconstrained. Reason: 'unclocked'. Mode:'default'. (TCK-001)
1
1
1

tcl used in floorplan:

script:

#Design_compiler tools

############### setting the variables for paths ###################

set path_ref { /home/user4/Desktop/projects/SAED_32nm/NDM/}
set my_ref_libs "$path_ref/saed32_1p9m_tech.ndm/ \
                 $path_ref/saed32_hvt.ndm/  \
                 $path_ref/saed32_lvt.ndm/   \
                 $path_ref/saed32_rvt.ndm  \
                 $path_ref/saed32_sram_lp.ndm"

set ref_lib {/home/user4/Desktop/projects/SAED32_EDK/stdcell_hvt/milkyway/saed32nm_hvt_1p9m/
             /home/user4/Desktop/projects/SAED32_EDK/stdcell_lvt/milkyway/saed32nm_lvt_1p9m/
              /home/user4/Desktop/projects/SAED32_EDK/stdcell_rvt/milkyway/saed32nm_rvt_1p9m/}

set tech_libs {/home/user4/Desktop/projects/SAED_32nm/tech/saed32nm_1p9m.tf}

create_lib usb_phy.dlib -technology $tech_libs -ref_lib $my_ref_libs


read_verilog /home/user4/Desktop/usb_phy/synth/outputs/icc2_files/usb_phy.v
link_block

2. usb_lay_para.tcl

## reading_tluplus and nxtgrd#####
read_parasitic_tech -tlup /home/user4/Desktop/projects/SAED_32nm/tech/saed32nm_1p9m_Cmax.tluplus -layermap /home/user4/Desktop/projects/SAED_32nm/tech/saed32nm_tf_itf_tluplus.map -name maxTLU
read_parasitic_tech -tlup /home/user4/Desktop/projects/SAED_32nm/tech/saed32nm_1p9m_Cmin.tluplus -layermap /home/user4/Desktop/projects/SAED_32nm/tech/saed32nm_tf_itf_tluplus.map -name minTLU

###setting_attr site_defs####
set_attribute [get_site_defs unit] symmetry Y
set_attribute [get_site_defs unit] is_default true

##setting_attr_routing_layers###
set_attribute [get_layers {M1 M3 M5 M7 M9}] routing_direction horizontal
set_attribute [get_layers {M2 M4 M6 M8 MRDL}] routing_direction vertical
set_ignored_layers -max_routing_layer M9

return 1

3.floorplam_usb.tcl

# pre_floorplan_checks

file mkdir ../reports/floorplan
check_timing > ../reports/floorplan/check_timing_pre_floorplan.rpt
check_netlist > ../reports/floorplan/check_netlist_pre_floorplan.rpt
report_design -all > ../reports/floorplan/design_report_pre_floorplan.rpt
report_design_mismatch > ../reports/floorplan/design_mismatch_pre.rpt

#creating_flooprlamn and blockages

initialize_floorplan -boundary {{0.000 0.000} {55 55}} -core_offset {5}
report_utilization
load_upf /home/user4/Desktop/usb_phy/synth/outputs/icc2_files/usb_phy.upf
commit_upf

shape_blocks
set_block_pin_constraints -self -allowed_layers {M3 M4 M5 M6}

place_pins -self
read_sdc /home/user4/Desktop/usb_phy/synth/logs/usb_phy_exc.sdc -echo

#Boundary cells

set_attribute [get_lib_cells *DCAP*] dont_use false

set_boundary_cell_rules \
  -left_boundary_cell saed32_rvt|saed32_rvt_std/DCAP_RVT \
  -right_boundary_cell saed32_rvt|saed32_rvt_std/DCAP_RVT \
  -top_boundary_cell saed32_rvt|saed32_rvt_std/DCAP_RVT \
  -bottom_boundary_cell saed32_rvt|saed32_rvt_std/DCAP_RVT

compile_targeted_boundary_cells -target_objects [get_voltage_areas]

check_legality -cells [get_cells *DCAP*] > ../reports/floorplan/check_legality_boundary_cells.rpt

set_dont_touch [get_lib_cells */TIE*] false

set_lib_cell_purpose -include optimization [get_lib_cell */TIE*]

#save_block

save_block -as floorplan_done1

----------------------------------------

powerplan_pre script:

remove_pg_strategies -all
remove_pg_patterns -all
remove_pg_regions -all
remove_pg_via_master_rules -all
remove_pg_strategy_via_rules -all
remove_routes -net_types {power ground} -ring -stripe -macro_pin_connect -lib_cell_pin_connect > /dev/null

connect_pg_net

##ring pattern###

create_pg_ring_pattern ring_pattern -horizontal_layer  M7 -horizontal_width {1} -horizontal_spacing {0.6} \
                                    -vertical_layer M8 -vertical_width {1} -vertical_spacing {0.5} -corner_bridge true

set_pg_strategy core_ring -pattern {{name: ring_pattern} {nets: {VDD VSS VDDL}} {offset: {0.5 0.5}}} -core\
                                   -extension { { {side:4} {stop: first_target} }\
                                                  { {side: 1} {direction: B} {nets: VDD} {stop: design_boundary_and_generate_pin} }\
                                                  { {side: 2} {direction: L} {nets: VSS} {stop: design_boundary_and_generate_pin} }\
                                                  { {side: 3} {direction: T} {nets: VDDL} {stop: design_boundary_and_generate_pin} } }

compile_pg -strategies core_ring

####upper_mesh

create_pg_mesh_pattern mesh_pattern -layers {{{vertical_layer: M8} {width: 0.6} {pitch: 8} {spacing: interleaving}}\
                                             {{horizontal_layer:M7} {width: 0.6} {pitch: 8} {spacing: interleaving} {trim: true}}}\
                                           -via_rule { {intersection: adjacent} {via_master: default} }

##router_top_mesh###

set_pg_strategy m7_m8_default_mesh -voltage_areas DEFAULT_VA\
                                                 -pattern {{name: mesh_pattern} {nets: VDD VSS}}\
                                                 -blockage {{nets: VDD} {voltage_areas: rx_phy}}\
                                                 -extension {{nets:VDD VSS} { direction: T L} {stop: outermost_ring}}

set_pg_strategy m7_m8_rx_mesh -voltage_areas rx_phy\
                                                  -pattern {{name: mesh_pattern} {nets: VDDL VSS}}\
                                                  -blockage {{nets: VDDL} {voltage_areas: DEFAULT_VA}}\
                                                  -extension {{nets:VDDL VSS} {direction: R B} {stop: outermost_ring}}

compile_pg -strategies {m7_m8_default_mesh m7_m8_rx_mesh}

#### lower_mesh
 
create_pg_mesh_pattern mesh_m2 -layers {{{vertical_layer: M2} {width: 0.10} {pitch:7} {spacing: interleaving} {offset: 0.75} {trim: true}}}\
                                             -via_rule { {intersection: adjacent} {via_master: default} }

set_pg_strategy m2_default_mesh -voltage_areas DEFAULT_VA\
                                               -pattern {{name: mesh_m2} {nets: VDD VSS}}\
                                              -blockage {{nets: VDD} {voltage_areas: rx_phy}}

set_pg_strategy m2_rx_phy_mesh -voltage_areas rx_phy \
                                          -pattern {{name: mesh_m2} {nets: VDDL VSS}} \
                                          -blockage {{nets: VDDL } {voltage_areas: DEFAULT_VA}}\
                                          -extension { {nets: VSS} {direction: T} {stop: core_boundary} }

compile_pg -strategies { m2_default_mesh m2_rx_phy_mesh}

############### rails

create_pg_std_cell_conn_pattern P_std_cell_rail -layers M1

set_pg_strategy M1_VA_rails -voltage_area DEFAULT_VA -pattern {{name: P_std_cell_rail} {nets: VDD}}

set_pg_strategy M1_VSS_rails \
                -core \
                -pattern {{name: P_std_cell_rail} {nets: VSS}}

compile_pg -strategies {M1_VA_rails M1_rx_phy_rails M1_VSS_rails }

file mkdir ../reports/powerplan

check_pg_missing_vias > ../reports/powerplan/pg_missing_vias.rpt
check_pg_drc > ../reports/powerplan/pg_drc.rpt
check_pg_connectivity > ../reports/powerplan/pg_connectivity.rpt

save_block -as power_plan_done
-------------------------------

and after doing ckeck pg connectivity:

check_pg_connectivityLoading cell instances...
Number of Standard Cells: 332
Number of Macro Cells: 0
Number of IO Pad Cells: 0
Number of Blocks: 0
Loading P/G wires and vias...
Number of VDD Wires: 42
Number of VDD Vias: 251
Number of VDD Terminals: 1
Number of VDDL Wires: 12
Number of VDDL Vias: 28
Number of VDDL Terminals: 1
Number of VSS Wires: 63
Number of VSS Vias: 586
Number of VSS Terminals: 1
**************Verify net VDD connectivity*****************
  Number of floating wires: 0
  Number of floating vias: 0
  Number of floating std cells: 176
  Number of floating hard macros: 0
  Number of floating I/O pads: 0
  Number of floating terminals: 0
  Number of floating hierarchical blocks: 0
************************************************************
**************Verify net VDDL connectivity*****************
  Number of floating wires: 0
  Number of floating vias: 0
  Number of floating std cells: 170
  Number of floating hard macros: 0
  Number of floating I/O pads: 0
  Number of floating terminals: 0
  Number of floating hierarchical blocks: 0
************************************************************
**************Verify net VSS connectivity*****************
  Number of floating wires: 0
  Number of floating vias: 0
  Number of floating std cells: 332
  Number of floating hard macros: 0
  Number of floating I/O pads: 0
  Number of floating terminals: 0
  Number of floating hierarchical blocks: 0
************************************************************
Overall runtime: 0 seconds.

-------------------------------

# timing report

report_timing****************************************
Report : timing
        -path_type full
        -delay_type max
        -max_paths 1
        -report_by design
Design : usb_phy
Version: R-2020.09-SP2
Date   : Thu Mar  5 12:49:33 2026
****************************************

  Startpoint: i_rx_phy/fs_ce_reg (rising edge-triggered flip-flop clocked by usb_clk)
  Endpoint: i_tx_phy/one_cnt_reg[2] (rising edge-triggered flip-flop clocked by usb_clk)
  Mode: default
  Corner: default
  Scenario: default
  Path Group: usb_clk
  Path Type: max

  Point                                            Incr      Path  
  ------------------------------------------------------------------------
  clock usb_clk (rise edge)                        0.00      0.00
  clock network delay (ideal)                      0.00      0.00

  i_rx_phy/fs_ce_reg/CLK (DFFX1_HVT)               0.00      0.00 r
  i_rx_phy/fs_ce_reg/Q (DFFX1_HVT)                 0.10      0.10 f
  fs_ce_UPF_LS/Y (LSUPX1_HVT)                      0.09      0.19 f
  i_tx_phy/U6/Y (AND2X2_HVT)                       0.05      0.24 f
  i_tx_phy/U11/Y (NAND3X0_HVT)                     0.03      0.28 r
  i_tx_phy/U13/Y (INVX0_HVT)                       0.04      0.31 f
  i_tx_phy/U18/Y (AND2X1_HVT)                      0.04      0.36 f
  i_tx_phy/U19/Y (NAND2X0_HVT)                     0.02      0.38 r
  i_tx_phy/U20/Y (NAND2X0_HVT)                     0.03      0.41 f
  i_tx_phy/U63/Y (AO222X1_HVT)                     0.04      0.44 f
  i_tx_phy/one_cnt_reg[2]/D (DFFX1_HVT)            0.00      0.44 f
  data arrival time                                          0.44

  clock usb_clk (rise edge)                        5.00      5.00
  clock network delay (ideal)                      0.00      5.00
  i_tx_phy/one_cnt_reg[2]/CLK (DFFX1_HVT)          0.00      5.00 r
  library setup time                              -0.01      4.99
  data required time                                         4.99
  ------------------------------------------------------------------------
  data required time                                         4.99
  data arrival time                                         -0.44
  ------------------------------------------------------------------------
  slack (MET)                                                4.54


1
report_timing > ../reports/placement/timing_pre_placement_opt.rpt
icc2_shell> report_constraints -all_violators
****************************************
Report : constraint
        -all_violators
Design : usb_phy
Version: R-2020.09-SP2
Date   : Thu Mar  5 12:51:02 2026
****************************************

   late_timing
   -----------

Endpoint                         Path Delay     Path Required       CRP    Slack Group    Scenario
----------------------------------------------------------------------------------------------------------
No paths.

   early_timing
   -----------

Endpoint                         Path Delay     Path Required       CRP    Slack Group    Scenario
----------------------------------------------------------------------------------------------------------
No paths.

   Mode: default Corner: default
   Scenario: default
  ---------------------------------------------------------------------------
   Number of max_transition violation(s): 0

   Mode: default Corner: default
   Scenario: default
  ---------------------------------------------------------------------------
   Number of max_capacitance violation(s): 0


   Mode: default Corner: default
   Scenario: default
  ---------------------------------------------------------------------------
   Number of min_capacitance violation(s): 0

   Total number of violation(s): 0
1
-------------------------------------------

## cts script:
file mkdir ../reports/clock

check_design -checks pre_clock_tree_stage > ../reports/clock/check_design_pre_cts.rpt

check_legality -verbose > ../reports/clock/check_legality_pre_cts.rpt

report_timing > ../reports/clock/report_timing_pre_cts.rpt

report_congestion > ../reports/clock/report_congestion_pre_cts.rpt

report_clock_timing -type skew > ../reports/clock/report_clock_timing_pre_cts.rpt

report_clock_tree_options > ../reports/clock/report_clock_tree_options_pre_cts.rpt

report_clock_qor -type structure > ../reports/clock/report_clock_qor_pre_cts.rpt

########clock_cells

set CTS_CELLS [get_lib_cells " */NBUFF*LVT */NBUFF*RVT \
                               */INVX*_LVT */INVX*RVT \
                              */CGL* */LSUP* */DFF* "]

set_dont_touch $CTS_CELLS false
set_lib_cell_purpose -exclude cts [get_lib_cells]
set_lib_cell_purpose -include cts $CTS_CELLS


foreach_in_collection scen [all_scenarios] {
   current_scenario $scen
  set_clock_uncertainty 0.1 -setup [all_clocks]
  set_clock_uncertainty 0.05 -hold [all_clocks]
}

set_clock_tree_options -target_skew 0.03 -target_latency 0.03 -clock usb_clk

set_app_options -name clock_opt.flow.enable_ccd -value false

set_max_transition 5.0 [current_design]

######applying_clock

clock_opt

##### report_post_cts

report_routing_rules -verbose > ../reports/clock/report_routing_rules_post_cts.rpt

report_clock_routing_rules > ../reports/clock/report_clock_routing_rules_post_cts.rpt

report_timing > ../reports/clock/report_timing_post_cts.rpt

report_timing -delay_type min > ../reports/clock/report_timing_min_post_cts.rpt

report_clocks -skew > ../reports/clock/report_clock_skew_post_cts.rpt

 ##save_block

save_block -as post_cts_done  


--------------------------------------
route_script:

file mkdir ../reports/route_rpt
check_design -checks pre_route_stage

report_timing > ../reports/route_rpt/report_timing_pre_rte_stg.rpt

report_timing -delay_type min > ../reports/route_rpt/report_timing_min_pre_rte_stg.rpt

report_congestion > ../reports/route_rpt/report_congestion_pre_rte_stg.rpt

report_clock_timing -type skew > ../reports/route_rpt/report/report_clock_timing_pre_rte_stg.rpt

report_design > ../reports/route_rpt/report_design_pre_stg.rpt

check_design -checks pre_route_stage > ../reports/route_rpt/check_design_pre_rte_stg.rpt

report_constraints > ../reports/route_rpt/report_constraints.rpt


####set_app_option

set_app_options -list {route.global.timing_driven {true}}
set_app_options -list {route.detail.timing_driven {true}}
set_app_options -list {route.track.timing_driven {true}}


### filler_cells_insertions

create_stdcell_fillers -lib_cells [get_lib_cells *SHFILL*_LVT]
connect_pg_net

remove_stdcell_fillers_with_violation


### route_init

route_auto

save_block -as init_route_route_done1

#####route_opt

route_opt

route_eco

-------------------------------------
#reports:

Route-opt final QoR
___________________
Scenario Mapping Table
1: default

Pathgroup Mapping Table
1: **default**
2: **async_default**
3: **clock_gating_default**
4: **in2reg_default**
5: **reg2out_default**
6: **in2out_default**
7: usb_clk

-------------------------------------------------------------------------------------------------------------------------------------------------------------
PATHGROUP QOR 
-------------------------------------------------------------------------------------------------------------------------------------------------------------
Scene  PG      WNS        TNS    NSV      WHV        THV    NHV
    1   1   0.0000     0.0000      0   0.0000     0.0000      0
    1   2   0.0000     0.0000      0   0.0000     0.0000      0
    1   3   0.0000     0.0000      0   0.0000     0.0000      0
    1   4   0.0000     0.0000      0   0.0000     0.0000      0
    1   5   0.0000     0.0000      0   0.0000     0.0000      0
    1   6   0.0000     0.0000      0   0.0000     0.0000      0
    1   7   0.0000     0.0000      0   0.0000     0.0000      0
----------------------------------------------------------------------------------------------------------------------------------------------------------------------
SCENARIO QOR 
----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Scene  PG      WNS        TNS   R2RTNS    NSV      WHV        THV    NHV  MaxTrnV   MaxTranC  MaxCapV    Leakage
    1   *   0.0000     0.0000   0.0000      0   0.0000     0.0000      0        0     0.0000        0 3228050688
----------------------------------------------------------------------------------------------------------------------------------------------------------------------
DESIGN QOR 
----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Scene  PG      WNS        TNS   R2RTNS    NSV      WHV        THV    NHV  MaxTrnV   MaxTranC  MaxCapV    Leakage         Area    InstCnt
    *   *   0.0000     0.0000   0.0000      0   0.0000     0.0000      0        0     0.0000        0 3228050688      1364.75        395
----------------------------------------------------------------------------------------------------------------------------------------------------------------------

Route-opt final QoR Summary         WNS        TNS   R2RTNS    NSV      WHV        THV    NHV  MaxTrnV  MaxCapV    Leakage         Area    InstCnt
Route-opt final QoR Summary      0.0000     0.0000   0.0000      0   0.0000     0.0000      0        0        0 3228050688      1364.75        395

Route-opt command complete                CPU:  8059 s (  2.24 hr )  ELAPSE: 145763 s ( 40.49 hr )  MEM-PEAK:  1537 MB
Route-opt command statistics  CPU=11 sec (0.00 hr) ELAPSED=12 sec (0.00 hr) MEM-PEAK=1.501 GB
Information: Ending 'route_opt' (FLW-8001)
Information: Time: 2026-03-06 04:53:45 / Session: 40.49 hr / Command: 0.00 hr / Memory: 1537 MB (FLW-8100)
Information: Running auto PG connection. (NDM-099)
1
route_ecoGenerating Timing information  
Design  Scenario default (Mode default Corner default)
Estimated unitRes/unitCap :
Layer M1 : Res  2 ohm/um Cap  0.130745 ff/um
Layer M2 : Res  1.78571 ohm/um Cap  0.135212 ff/um
Layer M3 : Res  1.78571 ohm/um Cap  0.0918491 ff/um
Layer M4 : Res  1.78571 ohm/um Cap  0.0918436 ff/um
Layer M5 : Res  1.78571 ohm/um Cap  0.0769444 ff/um
Layer M6 : Res  1.78571 ohm/um Cap  0.0769428 ff/um
Layer M7 : Res  1.78571 ohm/um Cap  0.0730916 ff/um
Layer M8 : Res  1.78571 ohm/um Cap  0.0726471 ff/um
Layer M9 : Res  1.75 ohm/um Cap  0.0841194 ff/um
Layer MRDL : Res  0.175 ohm/um Cap  0.103558 ff/um
Generating Timing information  ... Done
Information: Serialized np data
INFO: timer data saved to /tmp/route_block_271441_650629696.timdat
Cell Min-Routing-Layer = M1
Cell Max-Routing-Layer = M9
Turn off antenna since no rule is specified
Warning: Cannot find a default contact code for layer CO. (ZRT-022)
Info: number of net_type_blockage: 0 
Found 1 voltage-areas.
When applicable Min-max layer allow_pin_connection mode will allow paths of length 3.55 outside the layer range.
Warning: Standard cell pin LSUPX1_HVT/VDDL has no valid via regions. (ZRT-044)
[ECO: DbIn With Extraction] Elapsed real time: 0:00:00 
[ECO: DbIn With Extraction] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[ECO: DbIn With Extraction] Stage (MB): Used   19  Alloctr   19  Proc    0 
[ECO: DbIn With Extraction] Total (MB): Used   22  Alloctr   22  Proc 5052 
[ECO: Analysis] Elapsed real time: 0:00:00 
[ECO: Analysis] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[ECO: Analysis] Stage (MB): Used   19  Alloctr   19  Proc    0 
[ECO: Analysis] Total (MB): Used   22  Alloctr   22  Proc 5052 
Num of eco nets = 451
Num of open eco nets = 0
[ECO: Init] Elapsed real time: 0:00:00 
[ECO: Init] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[ECO: Init] Stage (MB): Used   19  Alloctr   19  Proc    0 
[ECO: Init] Total (MB): Used   22  Alloctr   23  Proc 5052 
Loading timing information to the router from design
Timing information loaded to the router.
Printing options for 'route.common.*'
common.enable_multi_thread                              :        true                
common.interactive_mode_clear_timing                    :        true                
common.verbose_level                                    :        0                   

Printing options for 'route.detail.*'
detail.antenna                                          :        true                
detail.eco_route_use_soft_spacing_for_timing_optimization:       true                
detail.force_end_on_preferred_grid                      :        true                
detail.timing_driven                                    :        true                

Printing options for 'route.auto_via_ladder.*'

Information: RC layer preference is turned on for this design. (ZRT-613)

Begin ECO DRC check ...

Checked 1/4 Partitions, Violations =    7
Checked 2/4 Partitions, Violations =    7
Checked 3/4 Partitions, Violations =    7
Checked 4/4 Partitions, Violations =    7

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      7

[DRC CHECK] Elapsed real time: 0:00:00 
[DRC CHECK] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[DRC CHECK] Stage (MB): Used    0  Alloctr    0  Proc    0 
[DRC CHECK] Total (MB): Used   56  Alloctr   57  Proc 5052 

Total Wire Length =                    6156 micron
Total Number of Contacts =             2942
Total Number of Wires =                2811
Total Number of PtConns =              421
Total Number of Routed Wires =       2811
Total Routed Wire Length =           6114 micron
Total Number of Routed Contacts =       2942
        Layer             M1 :        210 micron
        Layer             M2 :       2642 micron
        Layer             M3 :       2670 micron
        Layer             M4 :        498 micron
        Layer             M5 :        135 micron
        Layer             M6 :          0 micron
        Layer             M7 :          0 micron
        Layer             M8 :          0 micron
        Layer             M9 :          0 micron
        Layer           MRDL :          0 micron
        Via   VIA45SQ_C(rot) :         18
        Via        VIA45LG_C :          6
        Via        VIA34SQ_C :        179
        Via   VIA34SQ_C(rot) :          1
        Via        VIA34LG_C :          1
        Via        VIA23SQ_C :          8
        Via   VIA23SQ_C(rot) :       1335
        Via   VIA23LG_C(rot) :          1
        Via        VIA12SQ_C :       1372
        Via   VIA12SQ_C(rot) :         13
        Via   VIA12LG_C(rot) :          1
        Via    VIA12SQ_C_2x1 :          1
        Via    VIA12SQ_C_1x2 :          6

 
Redundant via conversion report:
--------------------------------

  Total optimized via conversion rate =  0.24% (7 / 2942 vias)
 
    Layer VIA1       =  0.50% (7      / 1393    vias)
        Weight 1     =  0.50% (7       vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 99.50% (1386    vias)
    Layer VIA2       =  0.00% (0      / 1344    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (1344    vias)
    Layer VIA3       =  0.00% (0      / 181     vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (181     vias)
    Layer VIA4       =  0.00% (0      / 24      vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (24      vias)
 
  Total double via conversion rate    =  0.24% (7 / 2942 vias)
 
    Layer VIA1       =  0.50% (7      / 1393    vias)
    Layer VIA2       =  0.00% (0      / 1344    vias)
    Layer VIA3       =  0.00% (0      / 181     vias)
    Layer VIA4       =  0.00% (0      / 24      vias)
 
  The optimized via conversion rate based on total routed via count =  0.24% (7 / 2942 vias)
 
    Layer VIA1       =  0.50% (7      / 1393    vias)
        Weight 1     =  0.50% (7       vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 99.50% (1386    vias)
    Layer VIA2       =  0.00% (0      / 1344    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (1344    vias)
    Layer VIA3       =  0.00% (0      / 181     vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (181     vias)
    Layer VIA4       =  0.00% (0      / 24      vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (24      vias)
 
Total number of nets = 451, of which 0 are not extracted
Total number of open nets = 0, of which 0 are frozen
Information: Using 1 threads for routing. (ZRT-444)
Start DR iteration 0: non-uniform partition
Routed  1/6 Partitions, Violations =    7
Routed  2/6 Partitions, Violations =    7
Routed  3/6 Partitions, Violations =    8
Routed  4/6 Partitions, Violations =    8
Routed  5/6 Partitions, Violations =    10
Routed  6/6 Partitions, Violations =    11

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      11
        Diff net spacing : 1
        Less than minimum area : 3
        Off-grid : 6
        Same net spacing : 1

[Iter 0] Elapsed real time: 0:00:01 
[Iter 0] Elapsed cpu  time: sys=0:00:00 usr=0:00:01 total=0:00:01
[Iter 0] Stage (MB): Used   34  Alloctr   34  Proc    0 
[Iter 0] Total (MB): Used   56  Alloctr   57  Proc 5052 

End DR iteration 0 with 6 parts

Start DR iteration 1: non-uniform partition
Routed  1/7 Partitions, Violations =    11
Routed  2/7 Partitions, Violations =    11
Routed  3/7 Partitions, Violations =    10
Routed  4/7 Partitions, Violations =    10
Routed  5/7 Partitions, Violations =    9
Routed  6/7 Partitions, Violations =    9
Routed  7/7 Partitions, Violations =    7

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      7
        Diff net spacing : 1
        Off-grid : 6

[Iter 1] Elapsed real time: 0:00:01 
[Iter 1] Elapsed cpu  time: sys=0:00:00 usr=0:00:01 total=0:00:01
[Iter 1] Stage (MB): Used   34  Alloctr   34  Proc    0 
[Iter 1] Total (MB): Used   56  Alloctr   57  Proc 5052 

End DR iteration 1 with 7 parts

Start DR iteration 2: non-uniform partition
Routed  1/6 Partitions, Violations =    7
Routed  2/6 Partitions, Violations =    7
Routed  3/6 Partitions, Violations =    7
Routed  4/6 Partitions, Violations =    7
Routed  5/6 Partitions, Violations =    7
Routed  6/6 Partitions, Violations =    7

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      7
        Needs fat contact : 1
        Off-grid : 6

[Iter 2] Elapsed real time: 0:00:02 
[Iter 2] Elapsed cpu  time: sys=0:00:00 usr=0:00:02 total=0:00:02
[Iter 2] Stage (MB): Used   34  Alloctr   34  Proc    0 
[Iter 2] Total (MB): Used   56  Alloctr   57  Proc 5052 

End DR iteration 2 with 6 parts

Start DR iteration 3: non-uniform partition
Routed  1/6 Partitions, Violations =    7
Routed  2/6 Partitions, Violations =    7
Routed  3/6 Partitions, Violations =    7
Routed  4/6 Partitions, Violations =    7
Routed  5/6 Partitions, Violations =    7
Routed  6/6 Partitions, Violations =    7

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      7
        Needs fat contact : 1
        Off-grid : 6

[Iter 3] Elapsed real time: 0:00:03 
[Iter 3] Elapsed cpu  time: sys=0:00:00 usr=0:00:02 total=0:00:02
[Iter 3] Stage (MB): Used   34  Alloctr   34  Proc    0 
[Iter 3] Total (MB): Used   56  Alloctr   57  Proc 5052 

End DR iteration 3 with 6 parts

Start DR iteration 4: non-uniform partition
Routed  1/6 Partitions, Violations =    7
Routed  2/6 Partitions, Violations =    7
Routed  3/6 Partitions, Violations =    7
Routed  4/6 Partitions, Violations =    7
Routed  5/6 Partitions, Violations =    7
Routed  6/6 Partitions, Violations =    7

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      7
        Needs fat contact : 1
        Off-grid : 6

[Iter 4] Elapsed real time: 0:00:03 
[Iter 4] Elapsed cpu  time: sys=0:00:00 usr=0:00:03 total=0:00:03
[Iter 4] Stage (MB): Used   34  Alloctr   34  Proc    0 
[Iter 4] Total (MB): Used   56  Alloctr   57  Proc 5052 

End DR iteration 4 with 6 parts

Start DR iteration 5: non-uniform partition
Routed  1/6 Partitions, Violations =    7
Routed  2/6 Partitions, Violations =    7
Routed  3/6 Partitions, Violations =    7
Routed  4/6 Partitions, Violations =    7
Routed  5/6 Partitions, Violations =    7
Routed  6/6 Partitions, Violations =    7

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      7
        Off-grid : 6
        Short : 1

[Iter 5] Elapsed real time: 0:00:04 
[Iter 5] Elapsed cpu  time: sys=0:00:00 usr=0:00:04 total=0:00:04
[Iter 5] Stage (MB): Used   34  Alloctr   34  Proc    0 
[Iter 5] Total (MB): Used   56  Alloctr   57  Proc 5052 

End DR iteration 5 with 6 parts

Start DR iteration 6: non-uniform partition
Routed  1/6 Partitions, Violations =    7
Routed  2/6 Partitions, Violations =    7
Routed  3/6 Partitions, Violations =    7
Routed  4/6 Partitions, Violations =    7
Routed  5/6 Partitions, Violations =    7
Routed  6/6 Partitions, Violations =    7

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      7
        Needs fat contact : 1
        Off-grid : 6

[Iter 6] Elapsed real time: 0:00:05 
[Iter 6] Elapsed cpu  time: sys=0:00:00 usr=0:00:05 total=0:00:05
[Iter 6] Stage (MB): Used   34  Alloctr   34  Proc    0 
[Iter 6] Total (MB): Used   56  Alloctr   57  Proc 5052 

End DR iteration 6 with 6 parts

Start DR iteration 7: non-uniform partition
Routed  1/6 Partitions, Violations =    7
Routed  2/6 Partitions, Violations =    7
Routed  3/6 Partitions, Violations =    7
Routed  4/6 Partitions, Violations =    7
Routed  5/6 Partitions, Violations =    7
Routed  6/6 Partitions, Violations =    7

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      7
        Needs fat contact : 1
        Off-grid : 6

[Iter 7] Elapsed real time: 0:00:06 
[Iter 7] Elapsed cpu  time: sys=0:00:00 usr=0:00:06 total=0:00:06
[Iter 7] Stage (MB): Used   34  Alloctr   34  Proc    0 
[Iter 7] Total (MB): Used   56  Alloctr   57  Proc 5052 

End DR iteration 7 with 6 parts

Start DR iteration 8: non-uniform partition
Routed  1/6 Partitions, Violations =    7
Routed  2/6 Partitions, Violations =    7
Routed  3/6 Partitions, Violations =    7
Routed  4/6 Partitions, Violations =    8
Routed  5/6 Partitions, Violations =    8
Routed  6/6 Partitions, Violations =    8

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      8
        Needs fat contact : 1
        Off-grid : 7

[Iter 8] Elapsed real time: 0:00:07 
[Iter 8] Elapsed cpu  time: sys=0:00:00 usr=0:00:06 total=0:00:06
[Iter 8] Stage (MB): Used   34  Alloctr   34  Proc    0 
[Iter 8] Total (MB): Used   56  Alloctr   57  Proc 5052 

End DR iteration 8 with 6 parts

Start DR iteration 9: non-uniform partition
Routed  1/5 Partitions, Violations =    8
Routed  2/5 Partitions, Violations =    8
Routed  3/5 Partitions, Violations =    7
Routed  4/5 Partitions, Violations =    7
Routed  5/5 Partitions, Violations =    7

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      7
        Diff net spacing : 1
        Off-grid : 6

[Iter 9] Elapsed real time: 0:00:08 
[Iter 9] Elapsed cpu  time: sys=0:00:00 usr=0:00:07 total=0:00:07
[Iter 9] Stage (MB): Used   34  Alloctr   34  Proc    0 
[Iter 9] Total (MB): Used   56  Alloctr   57  Proc 5052 

End DR iteration 9 with 5 parts

Start DR iteration 10: non-uniform partition
Routed  1/5 Partitions, Violations =    7
Routed  2/5 Partitions, Violations =    7
Routed  3/5 Partitions, Violations =    7
Routed  4/5 Partitions, Violations =    7
Routed  5/5 Partitions, Violations =    7

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      7
        Needs fat contact : 1
        Off-grid : 6

[Iter 10] Elapsed real time: 0:00:09 
[Iter 10] Elapsed cpu  time: sys=0:00:00 usr=0:00:08 total=0:00:08
[Iter 10] Stage (MB): Used   34  Alloctr   34  Proc    0 
[Iter 10] Total (MB): Used   56  Alloctr   57  Proc 5052 

End DR iteration 10 with 5 parts

Stop DR since not converging

Information: Detail Routing terminated early because DRCs were not converging: true (ZRT-312)

DR finished with 7 violations


DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      7
        Needs fat contact : 1
        Off-grid : 6


Total Wire Length =                    6155 micron
Total Number of Contacts =             2942
Total Number of Wires =                2809
Total Number of PtConns =              422
Total Number of Routed Wires =       2809
Total Routed Wire Length =           6113 micron
Total Number of Routed Contacts =       2942
        Layer             M1 :        212 micron
        Layer             M2 :       2642 micron
        Layer             M3 :       2668 micron
        Layer             M4 :        498 micron
        Layer             M5 :        135 micron
        Layer             M6 :          0 micron
        Layer             M7 :          0 micron
        Layer             M8 :          0 micron
        Layer             M9 :          0 micron
        Layer           MRDL :          0 micron
        Via   VIA45SQ_C(rot) :         18
        Via        VIA45LG_C :          6
        Via        VIA34SQ_C :        179
        Via   VIA34SQ_C(rot) :          1
        Via        VIA34LG_C :          1
        Via        VIA23SQ_C :          8
        Via   VIA23SQ_C(rot) :       1335
        Via   VIA23LG_C(rot) :          1
        Via        VIA12SQ_C :       1371
        Via   VIA12SQ_C(rot) :         13
        Via   VIA12LG_C(rot) :          1
        Via    VIA12SQ_C_2x1 :          2
        Via    VIA12SQ_C_1x2 :          6

 
Redundant via conversion report:
--------------------------------

  Total optimized via conversion rate =  0.27% (8 / 2942 vias)
 
    Layer VIA1       =  0.57% (8      / 1393    vias)
        Weight 1     =  0.57% (8       vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 99.43% (1385    vias)
    Layer VIA2       =  0.00% (0      / 1344    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (1344    vias)
    Layer VIA3       =  0.00% (0      / 181     vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (181     vias)
    Layer VIA4       =  0.00% (0      / 24      vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (24      vias)
 
  Total double via conversion rate    =  0.27% (8 / 2942 vias)
 
    Layer VIA1       =  0.57% (8      / 1393    vias)
    Layer VIA2       =  0.00% (0      / 1344    vias)
    Layer VIA3       =  0.00% (0      / 181     vias)
    Layer VIA4       =  0.00% (0      / 24      vias)
 
  The optimized via conversion rate based on total routed via count =  0.27% (8 / 2942 vias)
 
    Layer VIA1       =  0.57% (8      / 1393    vias)
        Weight 1     =  0.57% (8       vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 99.43% (1385    vias)
    Layer VIA2       =  0.00% (0      / 1344    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (1344    vias)
    Layer VIA3       =  0.00% (0      / 181     vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (181     vias)
    Layer VIA4       =  0.00% (0      / 24      vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (24      vias)
 



Begin timing optimization in DR ...

Printing options for 'route.common.*'
common.enable_multi_thread                              :        true                
common.interactive_mode_clear_timing                    :        true                
common.verbose_level                                    :        0                   

Printing options for 'route.detail.*'
detail.antenna                                          :        true                
detail.eco_route_use_soft_spacing_for_timing_optimization:       true                
detail.force_end_on_preferred_grid                      :        true                
detail.timing_driven                                    :        true                

Printing options for 'route.auto_via_ladder.*'

Information: RC layer preference is turned on for this design. (ZRT-613)
[Dr init] Elapsed real time: 0:00:09 
[Dr init] Elapsed cpu  time: sys=0:00:00 usr=0:00:08 total=0:00:08
[Dr init] Stage (MB): Used    7  Alloctr    7  Proc    0 
[Dr init] Total (MB): Used   30  Alloctr   30  Proc 5052 

Begin timing soft drc check ...

Created 0 soft drcs


DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      7
[SOFT DRC] Elapsed real time: 0:00:00 
[SOFT DRC] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[SOFT DRC] Stage (MB): Used    0  Alloctr    0  Proc    0 
[SOFT DRC] Total (MB): Used   30  Alloctr   30  Proc 5052 
[DR] Elapsed real time: 0:00:09 
[DR] Elapsed cpu  time: sys=0:00:00 usr=0:00:08 total=0:00:08
[DR] Stage (MB): Used    0  Alloctr    0  Proc    0 
[DR] Total (MB): Used   22  Alloctr   23  Proc 5052 
[DR: Done] Elapsed real time: 0:00:09 
[DR: Done] Elapsed cpu  time: sys=0:00:00 usr=0:00:08 total=0:00:08
[DR: Done] Stage (MB): Used    0  Alloctr    0  Proc    0 
[DR: Done] Total (MB): Used   22  Alloctr   23  Proc 5052 


Finished timing optimization in DR ...


Nets that have been changed:
Net 1 = n16
Net 2 = n19
Net 3 = n20
Net 4 = n21
Net 5 = n22
Net 6 = n26
Net 7 = rst_cnt[1]
Net 8 = i_tx_phy/tx_ip_sync
Net 9 = i_tx_phy/n11
Net 10 = i_tx_phy/one_cnt[0]
Net 11 = i_rx_phy/n116
Net 12 = i_rx_phy/rxdp_s
Net 13 = i_rx_phy/rxdn_s
Net 14 = i_rx_phy/N31
Net 15 = i_rx_phy/N32
Net 16 = i_rx_phy/n38
Net 17 = i_rx_phy/n46
Net 18 = i_rx_phy/n90
Net 19 = i_rx_phy/n94
Net 20 = i_rx_phy/n99
Net 21 = i_rx_phy/n102
Net 22 = i_rx_phy/n106
Net 23 = i_rx_phy/n108
Net 24 = i_rx_phy/n109
Net 25 = i_rx_phy/dpll_state[1]
Net 26 = ctsbuf_net_32
Net 27 = i_rx_phy/cts0
Total number of changed nets = 27 (out of 451)

[DR: Done] Elapsed real time: 0:00:09 
[DR: Done] Elapsed cpu  time: sys=0:00:00 usr=0:00:08 total=0:00:08
[DR: Done] Stage (MB): Used    0  Alloctr    0  Proc    0 
[DR: Done] Total (MB): Used   22  Alloctr   23  Proc 5052 
[ECO: DR] Elapsed real time: 0:00:09 
[ECO: DR] Elapsed cpu  time: sys=0:00:00 usr=0:00:08 total=0:00:08
[ECO: DR] Stage (MB): Used   19  Alloctr   19  Proc    0 
[ECO: DR] Total (MB): Used   22  Alloctr   23  Proc 5052 

ECO Route finished with 0 open nets, of which 0 are frozen

ECO Route finished with 7 violations

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      7
        Needs fat contact : 1
        Off-grid : 6



Total Wire Length =                    6155 micron
Total Number of Contacts =             2942
Total Number of Wires =                2809
Total Number of PtConns =              422
Total Number of Routed Wires =       2809
Total Routed Wire Length =           6113 micron
Total Number of Routed Contacts =       2942
        Layer             M1 :        212 micron
        Layer             M2 :       2642 micron
        Layer             M3 :       2668 micron
        Layer             M4 :        498 micron
        Layer             M5 :        135 micron
        Layer             M6 :          0 micron
        Layer             M7 :          0 micron
        Layer             M8 :          0 micron
        Layer             M9 :          0 micron
        Layer           MRDL :          0 micron
        Via   VIA45SQ_C(rot) :         18
        Via        VIA45LG_C :          6
        Via        VIA34SQ_C :        179
        Via   VIA34SQ_C(rot) :          1
        Via        VIA34LG_C :          1
        Via        VIA23SQ_C :          8
        Via   VIA23SQ_C(rot) :       1335
        Via   VIA23LG_C(rot) :          1
        Via        VIA12SQ_C :       1371
        Via   VIA12SQ_C(rot) :         13
        Via   VIA12LG_C(rot) :          1
        Via    VIA12SQ_C_2x1 :          2
        Via    VIA12SQ_C_1x2 :          6

 
Redundant via conversion report:
--------------------------------

  Total optimized via conversion rate =  0.27% (8 / 2942 vias)
 
    Layer VIA1       =  0.57% (8      / 1393    vias)
        Weight 1     =  0.57% (8       vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 99.43% (1385    vias)
    Layer VIA2       =  0.00% (0      / 1344    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (1344    vias)
    Layer VIA3       =  0.00% (0      / 181     vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (181     vias)
    Layer VIA4       =  0.00% (0      / 24      vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (24      vias)
 
  Total double via conversion rate    =  0.27% (8 / 2942 vias)
 
    Layer VIA1       =  0.57% (8      / 1393    vias)
    Layer VIA2       =  0.00% (0      / 1344    vias)
    Layer VIA3       =  0.00% (0      / 181     vias)
    Layer VIA4       =  0.00% (0      / 24      vias)
 
  The optimized via conversion rate based on total routed via count =  0.27% (8 / 2942 vias)
 
    Layer VIA1       =  0.57% (8      / 1393    vias)
        Weight 1     =  0.57% (8       vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 99.43% (1385    vias)
    Layer VIA2       =  0.00% (0      / 1344    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (1344    vias)
    Layer VIA3       =  0.00% (0      / 181     vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (181     vias)
    Layer VIA4       =  0.00% (0      / 24      vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (24      vias)
 

Total number of nets = 451
0 open nets, of which 0 are frozen
Total number of excluded ports = 0 ports of 0 unplaced cells connected to 0 nets
                                 0 ports without pins of 0 cells connected to 0 nets
                                 0 ports of 0 cover cells connected to 0 non-pg nets
Total number of DRCs = 7
Total number of antenna violations = antenna checking not active
Information: Routes in non-preferred voltage areas = 0 (ZRT-559)

Total Wire Length =                    6155 micron
Total Number of Contacts =             2942
Total Number of Wires =                2809
Total Number of PtConns =              422
Total Number of Routed Wires =       2809
Total Routed Wire Length =           6113 micron
Total Number of Routed Contacts =       2942
        Layer             M1 :        212 micron
        Layer             M2 :       2642 micron
        Layer             M3 :       2668 micron
        Layer             M4 :        498 micron
        Layer             M5 :        135 micron
        Layer             M6 :          0 micron
        Layer             M7 :          0 micron
        Layer             M8 :          0 micron
        Layer             M9 :          0 micron
        Layer           MRDL :          0 micron
        Via   VIA45SQ_C(rot) :         18
        Via        VIA45LG_C :          6
        Via        VIA34SQ_C :        179
        Via   VIA34SQ_C(rot) :          1
        Via        VIA34LG_C :          1
        Via        VIA23SQ_C :          8
        Via   VIA23SQ_C(rot) :       1335
        Via   VIA23LG_C(rot) :          1
        Via        VIA12SQ_C :       1371
        Via   VIA12SQ_C(rot) :         13
        Via   VIA12LG_C(rot) :          1
        Via    VIA12SQ_C_2x1 :          2
        Via    VIA12SQ_C_1x2 :          6

 
Redundant via conversion report:
--------------------------------

  Total optimized via conversion rate =  0.27% (8 / 2942 vias)
 
    Layer VIA1       =  0.57% (8      / 1393    vias)
        Weight 1     =  0.57% (8       vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 99.43% (1385    vias)
    Layer VIA2       =  0.00% (0      / 1344    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (1344    vias)
    Layer VIA3       =  0.00% (0      / 181     vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (181     vias)
    Layer VIA4       =  0.00% (0      / 24      vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (24      vias)
 
  Total double via conversion rate    =  0.27% (8 / 2942 vias)
 
    Layer VIA1       =  0.57% (8      / 1393    vias)
    Layer VIA2       =  0.00% (0      / 1344    vias)
    Layer VIA3       =  0.00% (0      / 181     vias)
    Layer VIA4       =  0.00% (0      / 24      vias)
 
  The optimized via conversion rate based on total routed via count =  0.27% (8 / 2942 vias)
 
    Layer VIA1       =  0.57% (8      / 1393    vias)
        Weight 1     =  0.57% (8       vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 99.43% (1385    vias)
    Layer VIA2       =  0.00% (0      / 1344    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (1344    vias)
    Layer VIA3       =  0.00% (0      / 181     vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (181     vias)
    Layer VIA4       =  0.00% (0      / 24      vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (24      vias)
 
Topology ECO not run, no qualifying violations or in frozen nets.
Updating the database ...
...updated 27 nets
[ECO: End] Elapsed real time: 0:00:09 
[ECO: End] Elapsed cpu  time: sys=0:00:00 usr=0:00:08 total=0:00:09
[ECO: End] Stage (MB): Used   -2  Alloctr   -2  Proc    0 
[ECO: End] Total (MB): Used    0  Alloctr    1  Proc 5052 
icc2_shell> report_timing
Information: Deserialized np data
INFO: timer data loaded from /tmp/route_block_271441_650629696.timdat
Information: Freeing timing information from routing. (ZRT-574)
Information: The RC mode used is DR for design 'usb_phy'. (NEX-022)
Extracting design(INC): route_block 
Information: coupling capacitance is lumped to ground. (NEX-030)
Information: 448 nets are successfully extracted. (NEX-028)
****************************************
Report : timing
        -path_type full
        -delay_type max
        -max_paths 1
        -report_by design
Design : usb_phy
Version: R-2020.09-SP2
Date   : Fri Mar  6 04:54:24 2026
****************************************

  Startpoint: i_rx_phy/rxdp_s_reg (rising edge-triggered flip-flop clocked by usb_clk)
  Endpoint: i_rx_phy/shift_en_reg (rising edge-triggered flip-flop clocked by usb_clk)
  Mode: default
  Corner: default
  Scenario: default
  Path Group: usb_clk
  Path Type: max

  Point                                            Incr      Path  
  ------------------------------------------------------------------------
  clock usb_clk (rise edge)                        0.00      0.00
  clock network delay (propagated)                 0.06      0.06

  i_rx_phy/rxdp_s_reg/CLK (DFFX1_HVT)              0.00      0.06 r
  i_rx_phy/rxdp_s_reg/Q (DFFX1_HVT)                0.11      0.17 f
  i_rx_phy/U15/Y (NOR2X1_HVT)                      0.06      0.23 r
  i_rx_phy/U33/Y (NOR4X1_HVT)                      0.08      0.31 f
  i_rx_phy/ctmTdsLR_1_836/Y (NAND2X0_HVT)          0.05      0.36 r
  i_rx_phy/U54/Y (NOR2X1_HVT)                      0.08      0.44 f
  i_rx_phy/ctmTdsLR_1_818/Y (OA221X1_HVT)          0.05      0.49 f
  i_rx_phy/ctmTdsLR_4_920/Y (NAND2X0_HVT)          0.02      0.52 r
  i_rx_phy/ctmTdsLR_1_917/Y (NAND3X0_HVT)          0.04      0.56 f
  i_rx_phy/shift_en_reg/D (DFFX1_HVT)              0.00      0.56 f
  data arrival time                                          0.56

  clock usb_clk (rise edge)                        5.00      5.00
  clock network delay (propagated)                 0.06      5.06
  i_rx_phy/shift_en_reg/CLK (DFFX1_HVT)            0.00      5.06 r
  clock uncertainty                               -0.10      4.96
  library setup time                              -0.03      4.93
  data required time                                         4.93
  ------------------------------------------------------------------------
  data required time                                         4.93
  data arrival time                                         -0.56
  ------------------------------------------------------------------------
  slack (MET)                                                4.37


1
icc2_shell> report_qor
****************************************
Report : qor
Design : usb_phy
Version: R-2020.09-SP2
Date   : Fri Mar  6 04:54:34 2026
****************************************


Scenario           'default'
Timing Path Group  '**in2reg_default**'
----------------------------------------
Levels of Logic:                      9
Critical Path Length:              0.40
Critical Path Slack:               4.47
Critical Path Clk Period:          5.00
Total Negative Slack:              0.00
No. of Violating Paths:               0
Worst Hold Violation:              0.00
Total Hold Violation:              0.00
No. of Hold Violations:               0
----------------------------------------

Scenario           'default'
Timing Path Group  '**reg2out_default**'
----------------------------------------
Levels of Logic:                      2
Critical Path Length:              0.21
Critical Path Slack:               4.66
Critical Path Clk Period:          5.00
Total Negative Slack:              0.00
No. of Violating Paths:               0
Worst Hold Violation:              0.00
Total Hold Violation:              0.00
No. of Hold Violations:               0
----------------------------------------

Scenario           'default'
Timing Path Group  'usb_clk'
----------------------------------------
Levels of Logic:                      7
Critical Path Length:              0.50
Critical Path Slack:               4.37
Critical Path Clk Period:          5.00
Total Negative Slack:              0.00
No. of Violating Paths:               0
Worst Hold Violation:              0.00
Total Hold Violation:              0.00
No. of Hold Violations:               0
----------------------------------------


Cell Count
----------------------------------------
Hierarchical Cell Count:              2
Hierarchical Port Count:             41
Leaf Cell Count:                    395
Buf/Inv Cell Count:                  68
Buf Cell Count:                      17
Inv Cell Count:                      51
CT Buf/Inv Cell Count:                0
Combinational Cell Count:           297
   Single-bit Isolation Cell Count:                        0
   Multi-bit Isolation Cell Count:                         0
   Isolation Cell Banking Ratio:                           0.00%
   Single-bit Level Shifter Cell Count:                    20
   Multi-bit Level Shifter Cell Count:                     0
   Level Shifter Cell Banking Ratio:                       0.00%
   Single-bit ELS Cell Count:                              0
   Multi-bit ELS Cell Count:                               0
   ELS Cell Banking Ratio:                                 0.00%
Sequential Cell Count:               98
   Integrated Clock-Gating Cell Count:                     0
   Sequential Macro Cell Count:                            0
   Single-bit Sequential Cell Count:                       98
   Multi-bit Sequential Cell Count:                        0
   Sequential Cell Banking Ratio:                          0.00%
   BitsPerflop:                                            1.00
Macro Count:                          0
----------------------------------------


Area
----------------------------------------
Combinational Area:              715.42
Noncombinational Area:           649.34
Buf/Inv Area:                    145.12
Total Buffer Area:                78.28
Total Inverter Area:              66.84
Macro/Black Box Area:              0.00
Net Area:                             0
Net XLength:                    3014.74
Net YLength:                    3093.99
----------------------------------------
Cell Area (netlist):                           1364.75
Cell Area (netlist and physical only):         3382.66
Net Length:                     6108.73


Design Rules
----------------------------------------
Total Number of Nets:               451
Nets with Violations:                 0
Max Trans Violations:                 0
Max Cap Violations:                   0
----------------------------------------

1
icc2_shell> report_timing
****************************************
Report : timing
        -path_type full
        -delay_type max
        -max_paths 1
        -report_by design
Design : usb_phy
Version: R-2020.09-SP2
Date   : Fri Mar  6 04:54:48 2026
****************************************

  Startpoint: i_rx_phy/rxdp_s_reg (rising edge-triggered flip-flop clocked by usb_clk)
  Endpoint: i_rx_phy/shift_en_reg (rising edge-triggered flip-flop clocked by usb_clk)
  Mode: default
  Corner: default
  Scenario: default
  Path Group: usb_clk
  Path Type: max

  Point                                            Incr      Path  
  ------------------------------------------------------------------------
  clock usb_clk (rise edge)                        0.00      0.00
  clock network delay (propagated)                 0.06      0.06

  i_rx_phy/rxdp_s_reg/CLK (DFFX1_HVT)              0.00      0.06 r
  i_rx_phy/rxdp_s_reg/Q (DFFX1_HVT)                0.11      0.17 f
  i_rx_phy/U15/Y (NOR2X1_HVT)                      0.06      0.23 r
  i_rx_phy/U33/Y (NOR4X1_HVT)                      0.08      0.31 f
  i_rx_phy/ctmTdsLR_1_836/Y (NAND2X0_HVT)          0.05      0.36 r
  i_rx_phy/U54/Y (NOR2X1_HVT)                      0.08      0.44 f
  i_rx_phy/ctmTdsLR_1_818/Y (OA221X1_HVT)          0.05      0.49 f
  i_rx_phy/ctmTdsLR_4_920/Y (NAND2X0_HVT)          0.02      0.52 r
  i_rx_phy/ctmTdsLR_1_917/Y (NAND3X0_HVT)          0.04      0.56 f
  i_rx_phy/shift_en_reg/D (DFFX1_HVT)              0.00      0.56 f
  data arrival time                                          0.56

  clock usb_clk (rise edge)                        5.00      5.00
  clock network delay (propagated)                 0.06      5.06
  i_rx_phy/shift_en_reg/CLK (DFFX1_HVT)            0.00      5.06 r
  clock uncertainty                               -0.10      4.96
  library setup time                              -0.03      4.93
  data required time                                         4.93
  ------------------------------------------------------------------------
  data required time                                         4.93
  data arrival time                                         -0.56
  ------------------------------------------------------------------------
  slack (MET)                                                4.37


1
icc2_shell> check_routes
Cell Min-Routing-Layer = M1
Cell Max-Routing-Layer = M9
Turn off antenna since no rule is specified
Warning: Cannot find a default contact code for layer CO. (ZRT-022)
Info: number of net_type_blockage: 0 
Found 1 voltage-areas.
When applicable Min-max layer allow_pin_connection mode will allow paths of length 5.32 outside the layer range.
Warning: Standard cell pin LSUPX1_HVT/VDDL has no valid via regions. (ZRT-044)


Start checking for open nets ... 

Total number of nets = 451, of which 0 are not extracted
Total number of open nets = 0, of which 0 are frozen

Check 451 nets, 0 have Errors

[CHECK OPEN NETS] Elapsed real time: 0:00:00 
[CHECK OPEN NETS] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
