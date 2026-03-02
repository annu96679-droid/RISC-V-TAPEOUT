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
