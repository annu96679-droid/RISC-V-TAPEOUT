# OpenROAD Flow Setup and Floorplan + Placement

## 1. Install OpenROAD Flow Scripts

**OpenROAD (Open Rapid Object-Oriented Design)** is an open-source EDA (Electronic Design Automation) tool that aims to provide a fully autonomous RTL-to-GDSII flow for digital ASIC design.  It supports synthesis, floorplanning, placement, clock tree synthesis, routing, and final layout generation. OpenROAD enables rapid design iterations, making it ideal for academic research and industry prototyping.

## Steps:

```bash
#Clone the OpenROAD Repository
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts

#Open the directory
cd OpenROAD-flow-scripts
```
<img width="1285" height="802" alt="Screenshot 2025-10-21 191140" src="https://github.com/user-attachments/assets/0a94a7fe-ab67-4bf9-be3b-94d71092d706" />
<img width="1281" height="801" alt="Screenshot 2025-10-21 191149" src="https://github.com/user-attachments/assets/4710803b-73ff-4f9d-a99a-8f2833aacdf6" />
<img width="1205" height="145" alt="Screenshot 2025-10-21 191242" src="https://github.com/user-attachments/assets/e9074a47-f100-4dc5-860a-5e2c74fb92f7" />

```bash
#Run the Setup Script
sudo ./setup.sh

```

<img width="1286" height="804" alt="Screenshot 2025-10-21 195213" src="https://github.com/user-attachments/assets/bf9289af-9b74-44a3-a0ad-66c9837a688e" />
<img width="1290" height="800" alt="Screenshot 2025-10-21 195254" src="https://github.com/user-attachments/assets/0028f98d-032e-4a15-9393-fd5e4e130fac" />

```bash
#Build OpenROAD
./build_openroad.sh --local

```

<img width="1274" height="809" alt="Screenshot 2025-10-21 211327" src="https://github.com/user-attachments/assets/4833e7de-c58d-4448-b9d3-c2c401dd08a3" />
<img width="1262" height="803" alt="Screenshot 2025-10-22 165740" src="https://github.com/user-attachments/assets/60961298-fc59-448e-adb6-f0bddae80564" />

```bash
#Verify Installation

source ./env.sh
yosys -help  
openroad -help

```

<img width="1285" height="803" alt="Screenshot 2025-10-22 165849" src="https://github.com/user-attachments/assets/be92affc-37ed-415f-b6fd-57534c4bcc9f" />
<img width="1282" height="804" alt="Screenshot 2025-10-22 165900" src="https://github.com/user-attachments/assets/d0a2fec0-6fe1-4752-9670-ad442585c0c8" />

```bash

#Run the OpenROAD Flow

cd flow
make

```
<img width="1284" height="805" alt="Screenshot 2025-10-22 170046" src="https://github.com/user-attachments/assets/5894bfb4-a040-4428-9067-9a9805f3be21" />

## Execute Floorplan and Placement
```bash
#Launch the graphical user interface (GUI) to visualize the final layout
 make gui_final

```
<img width="1285" height="802" alt="Screenshot 2025-10-25 111047" src="https://github.com/user-attachments/assets/15d92cfc-1566-46bf-87e8-f3133d120338" />
<img width="1285" height="806" alt="Screenshot 2025-10-22 170245" src="https://github.com/user-attachments/assets/93055875-3a94-40b5-bc34-3f0290bd71a8" />

**The Central Canvas: The Chip Layout**
This is the visual core of the image. It shows the physical implementation of the "gcd" circuit.

* The Grid of White Rectangles: These are the standard cell rows. The automated tool places the individual logic gates (from the nangates45 library) in these rows.

* The Small Colored Rectangles inside the Rows: These are the standard cells themselves (the individual logic gates like NAND, NOR, etc.). Different colors often represent different types of cells or different layers of the transistor.

* The Thin Colored Lines: This is the interconnect or routing. These are the metal wires that connect the standard cells together according to the circuit's netlist.

* Different colors represent different metal layers in the chip. A modern chip can have 10+ layers of metal wiring stacked on top of each other. The tool uses these layers to efficiently connect everything without wires shorting.

* In simple terms: The white rows are the "land" where the logic gates (colored blocks) are placed. The colored lines are the "roads" (wires) that connect the gates together.

## Short summary

## Steps followed:

```bash
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts
cd OpenROAD-flow-scripts
sudo ./setup.sh
./build_openroad.sh --local
source ./env.sh
yosys -help  
openroad -help
cd flow
make
make gui_final

```
## Challenges faced 

**Getting stuck on the 3rd step**
<img width="1156" height="801" alt="Screenshot 2025-10-21 235649" src="https://github.com/user-attachments/assets/14b41d93-dd4a-4514-a77f-8e3d2e39f172" />

**Steps to fix it**

```bash
#Enable swap (RAM is low)

sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

#Follow these commands

cd ~/Openroad-flow-scripts/tools/Openroad
rm -rf build
mkdir build
cd build
cmake .. -DEOPENROAD_BUILD_TESTS=OFF
gmake -j$(nproc)

```


UART_BLOCK PHysical design
------------------------------

setup.tcl

################ project 2nd : UART ###################
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

set synthetic_library dw_foundation.sldb
set my_lib "impl_top.mw"
#file delete -force $my_lib ; # don't run while working on first time 
create_mw_lib usb_phy.mw -technology $tech_libs  -mw_reference_library $ref_lib -open

############### setting the paths for target and link, reference library#############

set path_hvt {/home/user4/Desktop/projects/SAED32_EDK/stdcell_hvt/db_ccs/}
set path_lvt {/home/user4/Desktop/projects/SAED32_EDK/stdcell_lvt/db_ccs/}
set path_rvt {/home/user4/Desktop/projects/SAED32_EDK/stdcell_rvt/db_ccs/}



set link_library " $path_lvt/saed32lvt_ff0p95v125c.db \
		$path_lvt/saed32lvt_ff0p95vn40c.db \
		 $path_lvt/saed32lvt_ss0p95v125c.db \
		$path_hvt/saed32hvt_ss0p95v125c.db "


		
set ref_libs " $path_lvt/saed32lvt_ff0p95v125c.db \
		$path_lvt/saed32lvt_ff0p95vn40c.db \
		 $path_lvt/saed32lvt_ss0p95v125c.db \
		$path_hvt/saed32hvt_ss0p95v125c.db "

set target_library "$path_lvt/saed32lvt_ff0p95v125c.db \
		$path_lvt/saed32lvt_ff0p95vn40c.db \
		 $path_lvt/saed32lvt_ss0p95v125c.db \
		$path_hvt/saed32hvt_ss0p95v125c.db "
current_mw_lib
report_mw_lib
--------------------------------

run.tcl

source -echo /home/user4/Desktop/uart_working/synth/scripts/setup.tcl
set rtl_path "/home/user4/Desktop/uart_working/rtls"
analyze -format verilog [glob  /home/user4/Desktop/uart_working/rtl/*.v]
elaborate impl_top
current_design
remove_upf
 load_upf /home/user4/Desktop/uart_working/synth/constraints/impl_top.upf
 remove_sdc
read_sdc  /home/user4/Desktop/uart_working/synth/constraints/impl_top.sdc -echo

set_voltage 0.95 -object_list VDD
set_voltage 0.0 -object_list VSS
set_svf impl_top.svf

create_operating_conditions -process 1.0 -voltage 0.95 -temperature 125.0 -library saed32lvt_ff0p95v125c -name ff0p95v125c 
set_operating_conditions  -max ff0p95v125c

compile_ultra -no_autoungroup -no_boundary_optimization 

###### reports & files

write_file -format verilog -hierarchy -output impl_top_gln.v
write_icc2_files -output ../outputs/icc2_files1
write_parasitics -format reduced  -output spimemio_parasitics.spef > ../reports/impl_top_parasitics.spef
write_sdc impl_top.sdc
report_area
report_timing
report_power_domain
----------------------------------
Output of these commands

        'dw_foundation.sldb'. (UISN-26)
Information: Evaluating DesignWare library utilization. (UISN-27)

============================================================================
| DesignWare Building Block Library  |         Version         | Available |
============================================================================
| Basic DW Building Blocks           | R-2020.09-DWBB_202009.5 |     *     |
| Licensed DW Building Blocks        | R-2020.09-DWBB_202009.5 |     *     |
============================================================================

Information: Sequential output inversion is enabled.  SVF file must be used for formal verification. (OPT-1208)

Information: There are 8 potential problems in your design. Please run 'check_design' for more information. (LINT-99)

Information: Related supplies are not explicitly specified on 13 port(s) and primary supplies (VDD, VSS) of top power domain will be assumed as the related supply. (UPF-405)
Information: Total 0 repeater cells are inserted. (UPF-464)
Information: A total of 0 isolation cells are inserted. (UPF-214)
  Simplifying Design 'impl_top'

  Loading target library 'saed32lvt_ff0p95vn40c'
  Loading target library 'saed32lvt_ss0p95v125c'
  Loading target library 'saed32hvt_ss0p95v125c'
Loaded alib file './alib-52/saed32lvt_ff0p95v125c.db.alib'
Information: State dependent leakage is now switched from on to off.

  Beginning Pass 1 Mapping
  ------------------------
  Processing 'uart_rx_CLK_HZ50000000_BAUD_RATE9600'
Information: Added key list 'DesignWare' to design 'uart_rx_CLK_HZ50000000_BAUD_RATE9600'. (DDB-72)
 Implement Synthetic for 'uart_rx_CLK_HZ50000000_BAUD_RATE9600'.
  Processing 'uart_tx_CLK_HZ50000000_BAUD_RATE9600'
Information: Added key list 'DesignWare' to design 'uart_tx_CLK_HZ50000000_BAUD_RATE9600'. (DDB-72)
Information: The register 'tx_shift_reg[9]' is a constant and will be removed. (OPT-1206)
 Implement Synthetic for 'uart_tx_CLK_HZ50000000_BAUD_RATE9600'.
  Processing 'impl_top'
Information: In design 'impl_top', the register 'led_reg_reg[1]' is removed because it is merged to 'tx_data_reg[1]'. (OPT-1215)
Information: In design 'impl_top', the register 'led_reg_reg[2]' is removed because it is merged to 'tx_data_reg[2]'. (OPT-1215)
Information: In design 'impl_top', the register 'led_reg_reg[3]' is removed because it is merged to 'tx_data_reg[3]'. (OPT-1215)
Information: In design 'impl_top', the register 'led_reg_reg[4]' is removed because it is merged to 'tx_data_reg[4]'. (OPT-1215)
Information: In design 'impl_top', the register 'led_reg_reg[5]' is removed because it is merged to 'tx_data_reg[5]'. (OPT-1215)
Information: In design 'impl_top', the register 'led_reg_reg[6]' is removed because it is merged to 'tx_data_reg[6]'. (OPT-1215)
Information: In design 'impl_top', the register 'led_reg_reg[7]' is removed because it is merged to 'tx_data_reg[7]'. (OPT-1215)
Information: In design 'impl_top', the register 'tx_data_reg[0]' is removed because it is merged to 'led_reg_reg[0]'. (OPT-1215)
Memory usage for J1 task 554 Mbytes -- main task 554 Mbytes.

  Updating timing information
Information: Updating design information... (UID-85)
Information: The library cell 'PMT3_LVT' in the library 'saed32lvt_ff0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'PMT2_LVT' in the library 'saed32lvt_ff0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'PMT1_LVT' in the library 'saed32lvt_ff0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NMT3_LVT' in the library 'saed32lvt_ff0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NMT2_LVT' in the library 'saed32lvt_ff0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NMT1_LVT' in the library 'saed32lvt_ff0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'XOR3X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'XOR3X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'XOR2X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'XOR2X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'XNOR3X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'XNOR3X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'XNOR2X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'XNOR2X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'TNBUFFX8_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'TNBUFFX4_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'TNBUFFX32_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'TNBUFFX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'TNBUFFX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'TNBUFFX16_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFSSRX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFSSRX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFNX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFNX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFNASX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFNASX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFNASRX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFNASRX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFNARX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFNARX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFASX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFASX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFASRX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFASRX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFASRSX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFASRSX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFARX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFARX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'PMT3_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'PMT2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'PMT1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'PGX4_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'PGX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'PGX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR4X4_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR4X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR4X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR3X4_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR3X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR3X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR2X4_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR2X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR2X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OAI22X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OAI22X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OAI222X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OAI222X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OAI221X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OAI221X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OAI21X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OAI21X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OA22X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OA22X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OA222X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OA222X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OA221X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OA221X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OA21X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OA21X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR4X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR4X0_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR3X4_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR3X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR3X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR3X0_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR2X4_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR2X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR2X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR2X0_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NMT3_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NMT2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NMT1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NBUFFX8_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NBUFFX4_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NBUFFX32_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NBUFFX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NBUFFX16_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND4X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND4X0_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND3X4_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND3X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND3X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND3X0_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND2X4_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND2X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND2X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND2X0_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'MUX41X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'MUX41X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'MUX21X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'MUX21X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LNANDX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LNANDX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LATCHX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LATCHX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LASX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LASX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LASRX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LASRX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LASRQX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LASRQX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LASRNX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LASRNX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LARX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LARX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'INVX8_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'INVX4_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'INVX32_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'INVX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'INVX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'INVX16_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'INVX0_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'IBUFFX8_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'IBUFFX4_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'IBUFFX32_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'IBUFFX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'IBUFFX16_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'HADDX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'HADDX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'FADDX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'FADDX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFSSRX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFSSRX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNASX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNASX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNASRX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNASRX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNASRQX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNASRQX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNASRNX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNASRNX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNARX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNARX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFASX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFASX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFASRX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFASRX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFARX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFARX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DELLN3X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DELLN2X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DELLN1X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DEC24X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DEC24X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'CGLPPRX8_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'CGLPPRX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'BSLEX4_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'BSLEX2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'BSLEX1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AOI22X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AOI22X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AOI222X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AOI222X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AOI221X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AOI221X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AOI21X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AOI21X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AO22X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AO22X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AO222X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AO222X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AO221X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AO221X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AO21X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AO21X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND4X4_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND4X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND4X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND3X4_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND3X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND3X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND2X4_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND2X2_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND2X1_LVT' in the library 'saed32lvt_ff0p95vn40c' is not characterized for internal power. (PWR-536)
Information: The library cell 'XOR3X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'XOR3X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'XOR2X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'XOR2X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'XNOR3X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'XNOR3X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'XNOR2X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'XNOR2X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'TNBUFFX8_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'TNBUFFX4_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'TNBUFFX32_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'TNBUFFX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'TNBUFFX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'TNBUFFX16_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFSSRX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFSSRX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFNX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFNX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFNASX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFNASX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFNASRX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFNASRX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFNARX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFNARX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFASX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFASX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFASRX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFASRX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFASRSX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFASRSX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFARX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFARX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'PMT3_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'PMT2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'PMT1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'PGX4_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'PGX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'PGX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR4X4_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR4X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR4X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR3X4_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR3X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR3X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR2X4_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR2X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR2X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OAI22X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OAI22X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OAI222X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OAI222X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OAI221X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OAI221X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OAI21X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OAI21X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OA22X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OA22X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OA222X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OA222X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OA221X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OA221X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OA21X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OA21X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR4X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR4X0_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR3X4_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR3X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR3X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR3X0_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR2X4_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR2X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR2X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR2X0_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NMT3_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NMT2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NMT1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NBUFFX8_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NBUFFX4_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NBUFFX32_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NBUFFX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NBUFFX16_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND4X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND4X0_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND3X4_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND3X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND3X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND3X0_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND2X4_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND2X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND2X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND2X0_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'MUX41X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'MUX41X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'MUX21X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'MUX21X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LNANDX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LNANDX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LATCHX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LATCHX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LASX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LASX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LASRX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LASRX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LASRQX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LASRQX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LASRNX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LASRNX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LARX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LARX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'INVX8_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'INVX4_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'INVX32_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'INVX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'INVX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'INVX16_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'INVX0_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'IBUFFX8_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'IBUFFX4_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'IBUFFX32_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'IBUFFX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'IBUFFX16_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'HADDX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'HADDX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'FADDX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'FADDX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFSSRX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFSSRX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNASX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNASX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNASRX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNASRX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNASRQX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNASRQX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNASRNX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNASRNX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNARX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNARX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFASX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFASX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFASRX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFASRX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFARX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFARX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DELLN3X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DELLN2X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DELLN1X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DEC24X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DEC24X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'CGLPPRX8_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'CGLPPRX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'BSLEX4_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'BSLEX2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'BSLEX1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AOI22X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AOI22X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AOI222X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AOI222X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AOI221X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AOI221X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AOI21X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AOI21X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AO22X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AO22X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AO222X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AO222X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AO221X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AO221X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AO21X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AO21X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND4X4_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND4X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND4X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND3X4_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND3X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND3X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND2X4_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND2X2_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND2X1_LVT' in the library 'saed32lvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'XOR3X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'XOR3X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'XOR2X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'XOR2X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'XNOR3X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'XNOR3X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'XNOR2X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'XNOR2X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'TNBUFFX8_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'TNBUFFX4_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'TNBUFFX32_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'TNBUFFX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'TNBUFFX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'TNBUFFX16_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFSSRX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFSSRX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFNX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFNX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFNASX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFNASX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFNASRX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFNASRX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFNARX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFNARX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFASX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFASX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFASRX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFASRX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFASRSX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFASRSX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFARX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'SDFFARX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'PMT3_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'PMT2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'PMT1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'PGX4_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'PGX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'PGX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR4X4_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR4X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR4X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR3X4_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR3X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR3X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR2X4_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR2X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OR2X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OAI22X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OAI22X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OAI222X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OAI222X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OAI221X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OAI221X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OAI21X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OAI21X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OA22X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OA22X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OA222X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OA222X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OA221X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OA221X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OA21X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'OA21X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR4X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR4X0_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR3X4_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR3X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR3X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR3X0_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR2X4_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR2X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR2X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NOR2X0_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NMT3_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NMT2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NMT1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NBUFFX8_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NBUFFX4_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NBUFFX32_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NBUFFX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NBUFFX16_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND4X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND4X0_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND3X4_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND3X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND3X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND3X0_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND2X4_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND2X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND2X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'NAND2X0_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'MUX41X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'MUX41X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'MUX21X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'MUX21X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LNANDX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LNANDX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LATCHX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LATCHX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LASX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LASX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LASRX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LASRX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LASRQX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LASRQX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LASRNX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LASRNX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LARX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'LARX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'INVX8_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'INVX4_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'INVX32_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'INVX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'INVX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'INVX16_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'INVX0_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'IBUFFX8_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'IBUFFX4_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'IBUFFX32_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'IBUFFX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'IBUFFX16_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'HADDX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'HADDX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'FADDX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'FADDX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFSSRX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFSSRX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNASX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNASX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNASRX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNASRX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNASRQX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNASRQX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNASRNX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNASRNX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNARX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFNARX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFASX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFASX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFASRX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFASRX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFARX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DFFARX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DELLN3X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DELLN2X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DELLN1X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DEC24X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'DEC24X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'CGLPPRX8_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'CGLPPRX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'BSLEX4_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'BSLEX2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'BSLEX1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AOI22X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AOI22X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AOI222X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AOI222X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AOI221X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AOI221X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AOI21X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AOI21X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AO22X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AO22X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AO222X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AO222X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AO221X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AO221X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AO21X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AO21X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND4X4_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND4X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND4X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND3X4_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND3X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND3X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND2X4_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND2X2_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The library cell 'AND2X1_HVT' in the library 'saed32hvt_ss0p95v125c' is not characterized for internal power. (PWR-536)
Information: The target library(s) contains cell(s), other than black boxes, that are not characterized for internal power. (PWR-24)
Information: Total 0 level shifters are removed. (MV-238)
Information: Total 0 level shifters are inserted. (MV-239)
Information: The register 'uart_tx_inst/tx_shift_reg[0]' is a constant and will be removed. (OPT-1206)

Threshold voltage group cell usage:
>> saed32cell_hvt 0.00%, saed32cell_lvt 100.00%

  Beginning Mapping Optimizations  (Ultra High effort)
  -------------------------------

                                  TOTAL                                                
   ELAPSED            WORST NEG   SETUP    DESIGN                              LEAKAGE 
    TIME      AREA      SLACK     COST    RULE COST         ENDPOINT            POWER  
  --------- --------- --------- --------- --------- ------------------------- ---------
    0:00:07    1088.2      0.00       0.0       0.0                           28913482.0000
    0:00:07    1088.2      0.00       0.0       0.0                           28913482.0000

Threshold voltage group cell usage:
>> saed32cell_hvt 0.00%, saed32cell_lvt 100.00%

  Beginning Constant Register Removal
  -----------------------------------

Threshold voltage group cell usage:
>> saed32cell_hvt 0.00%, saed32cell_lvt 100.00%

  Beginning Global Optimizations
  ------------------------------
  Numerical Synthesis (Phase 1)
  Numerical Synthesis (Phase 2)
  Global Optimization (Phase 1)
  Global Optimization (Phase 2)
  Global Optimization (Phase 3)
  Global Optimization (Phase 4)
  Global Optimization (Phase 5)
  Global Optimization (Phase 6)
  Global Optimization (Phase 7)
  Global Optimization (Phase 8)
  Global Optimization (Phase 9)
  Global Optimization (Phase 10)
  Global Optimization (Phase 11)
  Global Optimization (Phase 12)
  Global Optimization (Phase 13)
  Global Optimization (Phase 14)
  Global Optimization (Phase 15)
  Global Optimization (Phase 16)
  Global Optimization (Phase 17)
  Global Optimization (Phase 18)
  Global Optimization (Phase 19)
  Global Optimization (Phase 20)
  Global Optimization (Phase 21)
  Global Optimization (Phase 22)
  Global Optimization (Phase 23)
  Global Optimization (Phase 24)
  Global Optimization (Phase 25)
  Global Optimization (Phase 26)
  Global Optimization (Phase 27)
  Global Optimization (Phase 28)
  Global Optimization (Phase 29)
  Global Optimization (Phase 30)

Threshold voltage group cell usage:
>> saed32cell_hvt 0.00%, saed32cell_lvt 100.00%

  Beginning Isolate Ports
  -----------------------

Threshold voltage group cell usage:
>> saed32cell_hvt 0.00%, saed32cell_lvt 100.00%

  Beginning Delay Optimization
  ----------------------------
    0:00:07     999.8      0.00       0.0       0.0                           26135724.0000
    0:00:07     999.8      0.00       0.0       0.0                           26135724.0000
    0:00:07     999.8      0.00       0.0       0.0                           26135724.0000
    0:00:07     999.8      0.00       0.0       0.0                           26135724.0000

Threshold voltage group cell usage:
>> saed32cell_hvt 0.00%, saed32cell_lvt 100.00%
    0:00:08     999.3      0.00       0.0       0.0                           26128362.0000
    0:00:08     999.3      0.00       0.0       0.0                           26128362.0000

  Beginning WLM Backend Optimization
  --------------------------------------
Information: Total 0 level shifters are removed incrementally. (MV-238)
Information: Total 0 level shifters are inserted incrementally. (MV-239)
    0:00:08     995.7      0.00       0.0       0.0                           26019856.0000
    0:00:08     995.7      0.00       0.0       0.0                           26019856.0000
    0:00:08     995.7      0.00       0.0       0.0                           26019856.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000

Threshold voltage group cell usage:
>> saed32cell_hvt 0.00%, saed32cell_lvt 100.00%


  Beginning Leakage Power Optimization  (max_leakage_power 0)
  ------------------------------------

                                  TOTAL                                                
   ELAPSED            WORST NEG   SETUP    DESIGN                              LEAKAGE 
    TIME      AREA      SLACK     COST    RULE COST         ENDPOINT            POWER  
  --------- --------- --------- --------- --------- ------------------------- ---------
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
  Global Optimization (Phase 31)
  Global Optimization (Phase 32)
  Global Optimization (Phase 33)
  Global Optimization (Phase 34)
  Global Optimization (Phase 35)
  Global Optimization (Phase 36)
  Global Optimization (Phase 37)
  Global Optimization (Phase 38)
  Global Optimization (Phase 39)
  Global Optimization (Phase 40)
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000

                                  TOTAL                                                
   ELAPSED            WORST NEG   SETUP    DESIGN                              LEAKAGE 
    TIME      AREA      SLACK     COST    RULE COST         ENDPOINT            POWER  
  --------- --------- --------- --------- --------- ------------------------- ---------
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     997.5      0.00       0.0       0.0                           25982770.0000
    0:00:08     997.5      0.00       0.0       0.0                           25982770.0000
    0:00:08     997.5      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000
    0:00:08     994.0      0.00       0.0       0.0                           25982770.0000


Note: Symbol # after min delay cost means estimated hold TNS across all active scenarios 


  Optimization Complete
  ---------------------
  Loading target library 'saed32lvt_ff0p95vn40c'
  Loading target library 'saed32lvt_ss0p95v125c'
  Loading target library 'saed32hvt_ss0p95v125c'
Information: State dependent leakage is now switched from off to on.
Information: Propagating switching activity (low effort zero delay simulation). (PWR-6)
1
Current design is 'impl_top'.
write_file -format verilog -hierarchy -output impl_top_gln.vWriting verilog file '/home/user4/Desktop/uart_working/synth/log/impl_top_gln.v'.
Warning: Verilog 'assign' or 'tran' statements are written out. (VO-4)
1
write_icc2_files -output ../outputs/icc2_files
Error: Output directory '../outputs/icc2_files' already exists, use the '-force' option to overwrite. (DCT-236)
0
write_icc2_files -output ../outputs/icc2_files1Memory usage for J2 task 583 Mbytes -- main task 583 Mbytes.
Memory usage for J3 task 583 Mbytes -- main task 583 Mbytes.
Memory usage for J4 task 583 Mbytes -- main task 583 Mbytes.
Memory usage for J5 task 583 Mbytes -- main task 583 Mbytes.
Information: During the execution of write_icc2_files error messages were issued, please check ../outputs/icc2_files1/write_icc2_files.log files. (DCT-228)
1
write_parasitics -format reduced  -output spimemio_parasitics.spef > ../reports/impl_top_parasitics.spefwrite_sdc impl_top.sdc1
report_area 
****************************************
Report : area
Design : impl_top
Version: R-2020.09-SP5-5
Date   : Fri Mar  6 19:13:18 2026
****************************************

Information: Updating design information... (UID-85)
Library(s) Used:

    saed32lvt_ff0p95v125c (File: /home/user4/Desktop/projects/SAED32_EDK/stdcell_lvt/db_ccs/saed32lvt_ff0p95v125c.db)

Number of ports:                           38
Number of nets:                           353
Number of cells:                          288
Number of combinational cells:            206
Number of sequential cells:                79
Number of macros/black boxes:               0
Number of buf/inv:                         28
Number of references:                       6

Combinational area:                471.945412
Buf/Inv area:                       35.580160
Noncombinational area:             522.011793
Macro/Black Box area:                0.000000
Net Interconnect area:             212.843051

Total cell area:                   993.957205
Total area:                       1206.800256
1
dc_shell> report_timing
 
****************************************
Report : timing
        -path full
        -delay max
        -max_paths 1
Design : impl_top
Version: R-2020.09-SP5-5
Date   : Fri Mar  6 19:14:23 2026
****************************************
Wire Load Model Mode: enclosed

  Startpoint: uart_rx_inst/clk_cnt_reg[1]
              (rising edge-triggered flip-flop clocked by impl_clk)
  Endpoint: uart_rx_inst/clk_cnt_reg[15]
            (rising edge-triggered flip-flop clocked by impl_clk)
  Path Group: impl_clk
  Path Type: max

  Des/Clust/Port     Wire Load Model       Library
  ------------------------------------------------
  impl_top           8000                  saed32lvt_ff0p95v125c
  uart_rx_CLK_HZ50000000_BAUD_RATE9600
                     8000                  saed32lvt_ff0p95v125c

  Point                                                   Incr       Path      Voltage
  ------------------------------------------------------------------------------------
  clock impl_clk (rise edge)                              0.00       0.00      
  clock network delay (ideal)                             0.00       0.00      
  uart_rx_inst/clk_cnt_reg[1]/CLK (DFFX1_LVT)             0.00       0.00 r    0.95
  uart_rx_inst/clk_cnt_reg[1]/Q (DFFX1_LVT)               0.06       0.06 r    0.95
  uart_rx_inst/U16/Y (NAND3X0_LVT)                        0.02       0.08 f    0.95
  uart_rx_inst/U17/Y (INVX1_LVT)                          0.02       0.10 r    0.95
  uart_rx_inst/U71/Y (NAND3X0_LVT)                        0.03       0.13 f    0.95
  uart_rx_inst/U73/Y (INVX1_LVT)                          0.01       0.15 r    0.95
  uart_rx_inst/U78/Y (NAND3X0_LVT)                        0.02       0.17 f    0.95
  uart_rx_inst/U79/Y (INVX1_LVT)                          0.02       0.19 r    0.95
  uart_rx_inst/U85/Y (NAND3X0_LVT)                        0.03       0.21 f    0.95
  uart_rx_inst/U87/Y (INVX1_LVT)                          0.01       0.23 r    0.95
  uart_rx_inst/U92/Y (NAND3X0_LVT)                        0.03       0.26 f    0.95
  uart_rx_inst/U93/Y (INVX1_LVT)                          0.01       0.27 r    0.95
  uart_rx_inst/U98/Y (NAND3X0_LVT)                        0.02       0.30 f    0.95
  uart_rx_inst/U99/Y (INVX1_LVT)                          0.01       0.31 r    0.95
  uart_rx_inst/U100/Y (OA21X1_LVT)                        0.03       0.34 r    0.95
  uart_rx_inst/U103/Y (NAND2X0_LVT)                       0.02       0.36 f    0.95
  uart_rx_inst/U106/Y (AO21X1_LVT)                        0.02       0.38 f    0.95
  uart_rx_inst/U107/Y (OA222X1_LVT)                       0.03       0.41 f    0.95
  uart_rx_inst/clk_cnt_reg[15]/D (DFFX1_LVT)              0.00       0.41 f    0.95
  data arrival time                                                  0.41      

  clock impl_clk (rise edge)                              5.00       5.00      
  clock network delay (ideal)                             0.00       5.00      
  uart_rx_inst/clk_cnt_reg[15]/CLK (DFFX1_LVT)            0.00       5.00 r    
  library setup time                                     -0.02       4.98      
  data required time                                                 4.98      
  ------------------------------------------------------------------------------------
  data required time                                                 4.98      
  data arrival time                                                 -0.41      
  ------------------------------------------------------------------------------------
  slack (MET)                                                        4.57      


1
dc_shell> 
report_power_domain 
****************************************
Report : power_domain
Design : impl_top
Version: R-2020.09-SP5-5
Date   : Fri Mar  6 19:14:36 2026
****************************************

------------------------------------------------------------------------------

 Power Domain             : impl_top
 Current Scope            : impl_top
 Elements                 : <top_level>
 Available Supply Nets    : VDD, VSS
 Available Supply Sets    : impl_top.primary, high_voltage

 Connections                -- Power --             -- Ground --
 Primary:                   VDD                     VSS
   (Max Op. Voltage)         (0.95)                  (0.00)
 Default Isolation:            -                       -
   (Max Op. Voltage)          (-)                     (-)
 Default Retention:            -                       -
   (Max Op. Voltage)          (-)                     (-)

------------------------------------------------------------------------------
1
--------------------------------------------------
#Floorplan

setup.tcl












icc2_shell> report_design
****************************************
Report : design
Design : impl_top
Version: R-2020.09-SP2
Date   : Sat Mar  7 04:48:40 2026
****************************************

Total number of std cells in library : 1025
Total number of dont_use lib cells   : 80
Total number of dont_touch lib cells : 80
Total number of buffers              : 69
Total number of inverters            : 45
Total number of flip-flops           : 306
Total number of latches              : 36
Total number of ICGs                 : 36

Cell Instance Type  Count         Area
--------------------------------------
TOTAL LEAF CELLS      285      993.957
Standard cells        285      993.957
Hard macro cells        0        0.000
Soft macro cells        0        0.000
Always on cells         0        0.000
Physical only           0        0.000
Fixed cells             0        0.000
Moveable cells        285      993.957
Sequential             79      522.012
Buffer/inverter        28       35.580
ICG cells               0        0.000

Logic Hierarchies                    : 2
Design Masters count                 : 23
Total Flat nets count                : 335
Total FloatingNets count             : 6
Total no of Ports                    : 13
Number of Master Clocks in design    : 0
Number of Generated Clocks in design : 0
Number of Path Groups in design      : 6 (0 of them Non Default)
Number of Scan Chains in design      : 0
List of Modes                        : default
List of Corners                      : default
List of Scenarios                    : default

Core Area                            : 0.000
Chip Area                            : 0.000
Total Site Row Area                  : 0.000
Number of Blockages                  : 0
Total area of Blockages              : 0.000
Number of Power Domains              : 1
Number of Voltage Areas              : 1
Number of Group Bounds               : 0
Number of Exclusive MoveBounds       : 0
Number of Hard or Soft MoveBounds    : 0
Number of Multibit Registers         : 0
Number of Multibit LS/ISO Cells      : 0
Number of Top Level RP Groups        : 0
Number of Tech Layers                : 71 (71 of them have unknown routing dir.)

Total wire length                    : 0.00 micron
Total number of wires                : 0
Total number of contacts             : 0

icc2_shell> report_timing
Information: Timer using 1 threads
Warning: Corner default:  0 process number, 1 process label, 0 voltage, and 0 temperature mismatches. (PVT-030)
Warning: 285 cells affected for early, 285 for late. (PVT-031)
Warning: 0 port driving_cells affected for early, 0 for late. (PVT-034)
Warning: no valid parasitic for default corner(LATE) (NEX-018)
Warning: no valid parasitic for default corner(EARLY) (NEX-018)
Warning: no valid parasitic for (all corners) corner((both specs)) (NEX-018)
Information: The stitching and editing of coupling caps is turned OFF for design 'uart.dlib:impl_top.design'. (TIM-125)
Warning: No physical information exists for design 'impl_top'. Zero interconnect delay is used in timing analysis. (TIM-101)
Warning: The extractor can not be initialized for design 'impl_top'. Zero interconnect delay is used in timing analysis. (TIM-102)
Warning: Batch extraction is skipped for block "impl_top"
Information: Update timing completed net estimation for all the timing graph nets (TIM-111)
Information: Net estimation statistics: timing graph nets = 329, routed nets = 0, across physical hierarchy nets = 0, parasitics cached nets = 0, delay annotated nets = 0, parasitics annotated nets = 0, multi-voltage nets = 0. (TIM-112)
************************************************************
Timer Settings:
Delay Calculation Style:                   auto
Signal Integrity Analysis:                 disabled
Timing Window Analysis:                    disabled
Advanced Waveform Propagation:             disabled
Variation Type:                            fixed_derate
Clock Reconvergence Pessimism Removal:     disabled
Advanced Receiver Model:                   disabled
************************************************************
****************************************
Report : timing
        -path_type full
        -delay_type max
        -max_paths 1
        -report_by design
Design : impl_top
Version: R-2020.09-SP2
Date   : Sat Mar  7 04:53:49 2026
****************************************
No paths.
-------------------------------------

floorplan.tcl
########################################
##     pre floorplan_checks
########################################
file mkdir ../reports/floorplan
check_timing > ../reports/floorplan/check_timing_pre_floorplan.rpt
check_netlist > ../reports/floorplan/check_netlist_pre_floorplan.rpt
report_design -all > ../reports/floorplan/design_report_pre_floorplan.rpt
report_design_mismatch > ../reports/floorplan/design_mismatch_pre.rpt


########################################
#       creating floorplan & blockage
########################################

initialize_floorplan -boundary {{0.000 0.000} {150 150}} -core_offset {5}
report_utilization
load_upf ../../synth/outputs/icc2_files/impl_top.upf
commit_upf

shape_blocks
set_block_pin_constraints -self -allowed_layers {M3 M4 M5 M6}

place_pins -self

read_sdc ../../synth/constraints/impl_top.sdc -echo
########################################
##          Boundary_cells
########################################

set_attribute [get_lib_cells *DCAP*] dont_use false

set_boundary_cell_rules -left_boundary_cell saed32_rvt|saed32_rvt_std/DCAP_RVT  -right_boundary_cell saed32_rvt|saed32_rvt_std/DCAP_RVT\
		    -top_boundary_cell saed32_rvt|saed32_rvt_std/DCAP_RVT -bottom_boundary_cell saed32_rvt|saed32_rvt_std/DCAP_RVT

compile_targeted_boundary_cells -target_objects [get_voltage_areas]

check_legality -cells [get_cells *DCAP*] > ../reports/floorplan/check_legality_boundary_cells.rpt





set_dont_touch [get_lib_cells */TIE*] false

set_lib_cell_purpose -include optimization [get_lib_cell */TIE*]


########################################
###            save_block
########################################

save_block -as floorplan_done
----------------------
result:

--------------------------------------------------------------------------

CELL INSTANCE INFORMATION
-----------------------------------------------------------------------------
Cell Instance Type          Count % of         Area  % of siteAreaPerSite
                                  total             total
-----------------------------------------------------------------------------
TOTAL LEAF CELLS              853  100     1715.726   100 unit:6751  

  Standard cells              285   33      993.957    57 unit:3911  
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
  Others                      568   66      721.769    42 unit:2840  

Special cells                   0    0        0.000     0 
  Level shifter                 0    0        0.000     0 
  Isolation                     0    0        0.000     0 
  Switch                        0    0        0.000     0 
  Always on                     0    0        0.000     0 
  Retention                     0    0        0.000     0 
  Tie off                       0    0        0.000     0 

LVT                             0    0        0.000     0 
HVT                             0    0        0.000     0 
Normal VT                       0    0        0.000     0 
Others                        853  100     1715.726   100 unit:6751  

Netlist cells                 285   33      993.957    57 unit:3911  
Physical only                 568   66      721.769    42 unit:2840  

Fixed cells                   568   66      721.769    42 unit:2840  
Moveable cells                285   33      993.957    57 unit:3911  

Combinational                 774   90     1193.714    69 unit:4697  
Sequential                     79    9      522.012    30 unit:2054  
Others                          0    0        0.000     0 

Buffer                          0    0        0.000     0 
Inverter                       28    3       35.580     2 unit:140  
Buffer/inverter                28    3       35.580     2 unit:140  

Spare cells                     0    0        0.000     0 
ICG cells                       0    0        0.000     0 
Flip-flop cells                79    9      522.012    30 unit:2054  
Latch cells                     0    0        0.000     0 
Antenna cells                   0    0        0.000     0 

Mux logic                       0    0        0.000     0 
Double height                   0    0        0.000     0 
Triple height                   0    0        0.000     0 
More than triple height         0    0        0.000     0 

Logic Hierarchies          2

REFERENCE DESIGN INFORMATION
----------------------------------------------------------------------------------------------
Number of reference designs used:24   
----------------------------------------------------------------------------------------------
Name           Type          Count     Width    Height        Area   PinDens SiteName siteArea
----------------------------------------------------------------------------------------------
DCAP_RVT       end_cap         568      0.76      1.67       1.271     1.574 unit            5
DFFX1_LVT      lib_cell         79      3.95      1.67       6.608     0.908 unit           26
AO22X1_LVT     lib_cell         39      1.52      1.67       2.541     2.754 unit           10
NAND2X0_LVT    lib_cell         24      0.91      1.67       1.525     3.279 unit            6
INVX1_LVT      lib_cell         23      0.76      1.67       1.271     3.148 unit            5
NAND3X0_LVT    lib_cell         13      1.06      1.67       1.779     3.373 unit            7
OA222X1_LVT    lib_cell         13      1.98      1.67       3.304     2.724 unit           13
AO21X1_LVT     lib_cell         11      1.52      1.67       2.541     2.361 unit           10
AO221X1_LVT    lib_cell         11      1.82      1.67       3.050     2.623 unit           12
AO222X1_LVT    lib_cell         11      1.98      1.67       3.304     2.724 unit           13
AND3X1_LVT     lib_cell         10      1.37      1.67       2.287     2.623 unit            9
AND2X1_LVT     lib_cell          9      1.22      1.67       2.033     2.459 unit            8
NAND4X0_LVT    lib_cell          9      1.22      1.67       2.033     3.443 unit            8
OA221X1_LVT    lib_cell          6      1.82      1.67       3.050     2.623 unit           12
OAI22X1_LVT    lib_cell          5      1.82      1.67       3.050     2.295 unit           12
OA22X1_LVT     lib_cell          5      1.52      1.67       2.541     2.754 unit           10
INVX0_LVT      lib_cell          5      0.76      1.67       1.271     3.148 unit            5
OR3X1_LVT      lib_cell          3      1.37      1.67       2.287     2.623 unit            9
OA21X1_LVT     lib_cell          3      1.52      1.67       2.541     2.361 unit           10
AND4X1_LVT     lib_cell          2      1.52      1.67       2.541     2.754 unit           10
AOI222X1_LVT   lib_cell          1      2.28      1.67       3.812     2.361 unit           15
AOI22X1_LVT    lib_cell          1      1.82      1.67       3.050     2.295 unit           12
OR2X1_LVT      lib_cell          1      1.22      1.67       2.033     2.459 unit            8
NOR2X0_LVT     lib_cell          1      1.52      1.67       2.541     1.967 unit           10

NET INFORMATION
------------------------------------------------------------------
NetType                 Count FloatingNets         Vias Nets/Cells
------------------------------------------------------------------
Total                     335            6            0      0.393
Signal                    334            5            0      0.392
Power                       0            0            0      0.000
Ground                      0            0            0      0.000
Analog Signal               0            0            0      0.000
Analog Ground               0            0            0      0.000
Analog Power                0            0            0      0.000
Clock                       0            0            0      0.000
Tie Low                     0            0            0      0.000
Tie High                    1            1            0      0.001
Others                      0            0            0      0.000

NET FANOUT AND PIN COUNT INFORMATION
---------------------------------------------------
Fanout        Netcount     netPinCount     NetCount
---------------------------------------------------
<2                 143     <2                     6
2                   83     2                    137
3                   55     3                     83
4                   27     4                     55
5                   12     5                     27
6-10                 9     6-10                  19
11-20                5     11-20                  7
21-30                0     21-30                  0
31-50                0     31-50                  0
51-100               1     51-100                 1
101-500              0     101-500                0
501-1000             0     501-1000               0
>1000                0     >1000                  0

PORT AND PIN INFORMATION
------------------------------------------------------------------------------
Type         Total     Input    Output     Inout      3-st     Power    Ground
------------------------------------------------------------------------------
Total         2925      2561       364         0         0       853       853
Macro            0         0         0         0         0         0         0
Ports           13         4         9         0         0         0         0
------------------------------------------------------------------------------

--------------------------------------------------------------------------------
                              FLOORPLAN INFORMATION
--------------------------------------------------------------------------------

CORE AND CHIP AREA INFORMATION
---------------------------
Core Area is :    22302.153
Chip Area is :    25600.000
---------------------------

SITE ROW INFORMATION
-----------------------------------------------------------------------
Site Name    Width    Height   Total Rows     Total Tiles          Area
-----------------------------------------------------------------------
unit          0.15      1.67           89           87754     22302.153
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
impl_top             DEFAULT_VA      VDD               VSS
--------------------------------------------------------------------

VOLTAGE AREA INFORMATION
-------------------------------------------------------------------------------------
VA Name          Number             Area       Target    bbox    bbox    bbox    bbox
              of shapes                   Utilization     llx     lly     urx     ury
-------------------------------------------------------------------------------------
DEFAULT_VA            1        22302.153         1.00    0.00    0.00  149.87  148.81
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
----------------------------------
pns.tcl

remove_pg_strategies -all
remove_pg_patterns -all
remove_pg_regions -all
remove_pg_via_master_rules -all
remove_pg_strategy_via_rules -all
remove_routes -net_types {power ground} -ring -stripe -macro_pin_connect -lib_cell_pin_connect > /dev/null

connect_pg_net

######ring pattern############

create_pg_ring_pattern ring_pattern -horizontal_layer M7 -horizontal_width {1} -horizontal_spacing {0.6} \
  				    -vertical_layer M8 -vertical_width {1} -vertical_spacing {0.5} -corner_bridge true

set_pg_strategy core_ring -pattern {{name: ring_pattern} {nets: {VDD VSS}} {offset: {0.5 0.5}}} -core\
  			  -extension { { {side:4} {stop: first_target} }\
              			       { {side: 1} {direction: B} {nets: VDD} {stop: design_boundary_and_generate_pin} } \
              			       { {side: 2} {direction: L} {nets: VSS} {stop: design_boundary_and_generate_pin} } }
              			 

compile_pg -strategies core_ring

###### upper_mesh


create_pg_mesh_pattern mesh_pattern -layers {{{vertical_layer: M8} {width: 0.6} {pitch: 10} {spacing:interleaving}}\
            				     {{horizontal_layer: M7} {width: 0.6} {pitch: 11} {spacing:interleaving} {trim:true}}}\
  				    -via_rule { {intersection: adjacent} {via_master: default} }

#############router_top_mesh######

set_pg_strategy m7_m8_default_mesh -voltage_areas DEFAULT_VA \
  			 	  -pattern {{name: mesh_pattern} {nets: VDD VSS}} \
 			          -extension {{nets:VDD VSS}{direction: T L R B}{stop: outermost_ring}}


compile_pg -strategies { m7_m8_default_mesh }

####### optional if pg_drc are more and via are not connecting the btw m1 and m8,m9 then we will use these lower mesh by using m2 metal later.
####### lower_mesh

create_pg_mesh_pattern mesh_m2 -layers {{{vertical_layer: M2} {width: 0.10} {pitch: 7} {spacing:interleaving} {offset: 0.75} {trim:true}}}\
   			        -via_rule { {intersection: adjacent} {via_master: default} }

set_pg_strategy m2_default_mesh -core  \
  			       -pattern {{name: mesh_m2} {nets: VDD VSS}} 
  			       


compile_pg -strategies { m2_default_mesh }





######## rails 

create_pg_std_cell_conn_pattern P_std_cell_rail -layers M1  
	

set_pg_strategy M1_VA_rails -core -pattern {{name: P_std_cell_rail} {nets: VDD VSS}}



compile_pg -strategies {M1_VA_rails }


file mkdir ../reports/powerplan

check_pg_missing_vias > ../reports/powerplan/pg_missing_vias.rpt
check_pg_drc > ../reports/powerplan/pg_drc.rpt
check_pg_connectivity > ../reports/powerplan/pg_connectivity.rpt

save_block -as powerplan_done

return
-------------------------------

reports:

No strategy-level via rule is specified, the default rule will be applied.
Automatic PG net connection through connect_pg_net is disabled.
Updating PG strategies.
Updating strategy m2_default_mesh.
Loading library and design information.
Number of Standard Cells: 0
Number of Hard Macros: 0
Number of Pads: 0
Creating straps and vias in power plan.
Creating wire shapes for strategies m2_default_mesh .
Creating wire shapes runtime: 0 seconds
Blockage cutting and DRC fixing for wire shapes for strategies m2_default_mesh .
Check and fix DRC for 43 wires for strategy m2_default_mesh.
Checking DRC for 43 wires:0% 10% 20% 30% 40% 50% 60% 70% 80% 90% 100%
Creating 43 wires after DRC fixing.
Wire DRC checking runtime 0.00 seconds.
Creating via shapes for strategies m2_default_mesh .
Working on strategy m2_default_mesh.
Number of detected intersections: 0
Total runtime of via shapes creation: 0 seconds
Runtime of via DRC checking for strategy m2_default_mesh: 0.00 seconds.
Creating via connection between strategies and existing shapes.
Check and fix DRCs for 580 stacked vias.
Checking DRC for 580 stacked vias:0% 5% 10% 15% 20% 25% 30% 35% 40% 45% 50% 55% 60% 65% 70% 75% 80% 85% 90% 95% 100%
Via DRC checking runtime 0.00 seconds.
via connection runtime: 0 seconds.
Removing dangling/floating wire/vias after DRC check.
Start iteration 1:
Checking potential dangling/floating power plan wires.
Checking dangling/floating vias between strategies and existing shapes.
Checking 580 stacked vias:0% 10% 20% 30% 40% 50% 60% 70% 80% 90% 100%
Finish removing all dangling or floating wires and vias.
Commiting wires and vias.
Committed 43 wires.
Committing wires takes 0.00 seconds.
Committed 2900 vias.
Committed 0 wires for via creation.
Committing vias takes 0.00 seconds.
Overall PG creation runtime: 0 seconds.
Successfully compiled PG.
Overall runtime: 0 seconds.
1
icc2_shell> 
create_pg_std_cell_conn_pattern P_std_cell_rail -layers M1  
Successfully create standard cell rail pattern P_std_cell_rail.
set_pg_strategy M1_VA_rails -core -pattern {{name: P_std_cell_rail} {nets: VDD VSS}}
Successfully set PG strategy M1_VA_rails.
compile_pg -strategies {M1_VA_rails }Sanity check for inputs.
No strategy-level via rule is specified, the default rule will be applied.
Automatic PG net connection through connect_pg_net is disabled.
Updating PG strategies.
Updating strategy M1_VA_rails.
Loading library and design information.
Number of Standard Cells: 0
Number of Hard Macros: 0
Number of Pads: 0
Creating standard cell rails.
Creating standard cell rails for strategy M1_VA_rails.
DRC checking and fixing for standard cell rail strategy M1_VA_rails.
Checking DRC for 90 wires:0% 10% 20% 30% 40% 50% 60% 70% 80% 90% 100%
Creating 90 wires after DRC fixing.
Wire DRC checking runtime 1.00 seconds.
Creating via connection between strategies and existing shapes.
Check and fix DRCs for 3240 stacked vias.
Checking DRC for 3240 stacked vias:0% 5% 10% 15% 20% 25% 30% 35% 40% 45% 50% 55% 60% 65% 70% 75% 80% 85% 90% 95% 100%
Via DRC checking runtime 6.00 seconds.
via connection runtime: 6 seconds.
Removing dangling/floating wire/vias after DRC check.
Start iteration 1:
Checking potential dangling/floating power plan wires.
Checking dangling/floating vias between strategies and existing shapes.
Checking 3000 stacked vias:0% 10% 20% 30% 40% 50% 60% 70% 80% 90% 100%
Start iteration 2:
Checking potential dangling/floating power plan wires.
Checking dangling/floating vias between strategies and existing shapes.
Checking 1873 stacked vias:0% 10% 20% 30% 40% 50% 60% 70% 80% 90% 100%
Finish removing all dangling or floating wires and vias.
Commiting wires and vias.
Committed 90 wires.
Committing wires takes 0.00 seconds.
Committed 1873 vias.
Committed 0 wires for via creation.
Committing vias takes 0.00 seconds.
Overall PG creation runtime: 7 seconds.
Successfully compiled PG.
Overall runtime: 7 seconds.
1
check_pg_connectivityLoading cell instances...
Number of Standard Cells: 853
Number of Macro Cells: 0
Number of IO Pad Cells: 0
Number of Blocks: 0
Loading P/G wires and vias...
Number of VDD Wires: 98
Number of VDD Vias: 2638
Number of VDD Terminals: 1
Number of VSS Wires: 99
Number of VSS Vias: 2647
Number of VSS Terminals: 1
**************Verify net VDD connectivity*****************
  Number of floating wires: 0
  Number of floating vias: 0
  Number of floating std cells: 285
  Number of floating hard macros: 0
  Number of floating I/O pads: 0
  Number of floating terminals: 0
  Number of floating hierarchical blocks: 0
************************************************************
**************Verify net VSS connectivity*****************
  Number of floating wires: 0
  Number of floating vias: 0
  Number of floating std cells: 285
  Number of floating hard macros: 0
  Number of floating I/O pads: 0
  Number of floating terminals: 0
  Number of floating hierarchical blocks: 0
************************************************************
Overall runtime: 0 seconds.
check_pg_drc Command check_pg_drc started  at Sat Mar  7 07:27:12 2026
Command check_pg_drc finished at Sat Mar  7 07:27:12 2026
CPU usage for check_pg_drc: 0.39 seconds ( 0.00 hours)
Elapsed time for check_pg_drc: 0.28 seconds ( 0.00 hours)
No errors found.
check_pg_missing_viasCheck net VDD vias...
Number of missing vias on VIA1 layer: 22
Total number of missing vias: 22
Checking net VDD vias took 0 seconds.
Check net VSS vias...
Number of missing vias on VIA1 layer: 40
Total number of missing vias: 40
Checking net VSS vias took 0 seconds.
Overall runtime: 1 seconds.
check_pg_drcCommand check_pg_drc started  at Sat Mar  7 07:41:31 2026
Command check_pg_drc finished at Sat Mar  7 07:41:32 2026
CPU usage for check_pg_drc: 0.30 seconds ( 0.00 hours)
Elapsed time for check_pg_drc: 0.36 seconds ( 0.00 hours)
No errors found.
---------------------------------

placemnt.tcl

########magnet_placement###############
#######################################

magnet_placement -multiple_long_port_mode auto [get_ports *32*]

########################################
############pre_place_checks############
########################################
file mkdir ../reports/placement

check_design -checks pre_placement_stage > ../reports/placement/check_design_pre_placement_stage.rpt

########################################
###########cell_density#################
########################################

set_app_options -list {place.coarse.max_density {0.6}}

#######################################
###############ndr_rules###############
#######################################

source ../constraints/ndr.tcl

########################################
###############placement################
########################################
set_parasitic_parameters -early_spec maxTLU -late_spec minTLU
set_voltage 0.95 -object_list VDD
set_voltage 0.0 -object_list VSS

create_placement -timing_driven -effort medium -congestion -congestion_effort medium

report_timing > ../reports/placement/timing_pre_placement_opt.rpt

########################################
################place_opt###############
########################################

place_opt

report_timing > ../reports/placement/timing_post_placement_opt.rpt

########################################
##############save_block################
########################################

save_block -as placement_done
--------------------------------------------------------

report

unds/Regions In This Design:
        Area    Num Cells  Exclusive Name
 (square um)
     22302.2          864        Yes DEFAULT_VA

Starting legalizer.
Warning: Exclusive bound 'DEFAULT' has no cells.
Optimizing Exclusive Bound 'DEFAULT_VA' (2/2)
    Done Exclusive Bound 'DEFAULT_VA' (2/2) (0 sec)
Legalization complete (0 total sec)

****************************************
  Report : Placement Attempts
  Site   : unit
****************************************

number of cells:                    864
number of references:                59
number of site rows:                 89
number of locations attempted:     4544
number of locations failed:         987  (21.7%)

Legality of references at locations:
34 references had failures.

Worst 10 references by total failures:
 CELLS       UNFLIPPED                       FLIPPED                REFERENCE NAME
         ATTEMPTS  FAILURES            ATTEMPTS  FAILURES           
    73        736       122 ( 16.6%)        236        30 ( 12.7%)  DFFX1_HVT
    27        316        84 ( 26.6%)        220        61 ( 27.7%)  AO22X1_RVT
    11        104        44 ( 42.3%)         56        25 ( 44.6%)  AO221X1_RVT
    11        314        40 ( 12.7%)         88        27 ( 30.7%)  NBUFFX2_HVT
    12        136        32 ( 23.5%)         88        31 ( 35.2%)  AO22X1_HVT
    11         96        40 ( 41.7%)         48        15 ( 31.2%)  OA222X1_RVT
    10         80        30 ( 37.5%)         56        16 ( 28.6%)  AO222X1_RVT
    20        184        29 ( 15.8%)         88        13 ( 14.8%)  INVX0_HVT
    15        136        22 ( 16.2%)         80        16 ( 20.0%)  NAND2X0_RVT
     6         80        20 ( 25.0%)         64        17 ( 26.6%)  DFFX2_HVT

Worst 10 references by failure rate:
 CELLS       UNFLIPPED                       FLIPPED                REFERENCE NAME
         ATTEMPTS  FAILURES            ATTEMPTS  FAILURES           
     1          8         6 ( 75.0%)          8         4 ( 50.0%)  OAI22X1_RVT
     1          8         5 ( 62.5%)          0         0 (  0.0%)  AOI22X1_RVT
     1          8         4 ( 50.0%)          0         0 (  0.0%)  NOR2X0_RVT
     1         10         5 ( 50.0%)          0         0 (  0.0%)  OA21X1_HVT
     2         16         9 ( 56.2%)         16         7 ( 43.8%)  OA21X1_RVT
     2         16         8 ( 50.0%)         16         6 ( 37.5%)  OA222X1_HVT
    11        104        44 ( 42.3%)         56        25 ( 44.6%)  AO221X1_RVT
     2         24        11 ( 45.8%)         16         6 ( 37.5%)  AND3X1_RVT
     3         24        11 ( 45.8%)          8         2 ( 25.0%)  OR3X1_LVT
    11         96        40 ( 41.7%)         48        15 ( 31.2%)  OA222X1_RVT

Legality of references in rows:
0 references had row failures.

****************************************
  Report : Cell Displacements
****************************************

number of cells aggregated:         296 (4051 total sites)
avg row height over cells:        1.672 um
rms cell displacement:            0.165 um ( 0.10 row height)
rms weighted cell displacement:   0.165 um ( 0.10 row height)
max cell displacement:            1.672 um ( 1.00 row height)
avg cell displacement:            0.039 um ( 0.02 row height)
avg weighted cell displacement:   0.039 um ( 0.02 row height)
number of cells moved:               28
number of large displacements:        0
large displacement threshold:     3.000 row height

Displacements of worst 10 cells:
Cell: uart_tx_inst/ZBUF_120_inst_17 (NBUFFX2_HVT)
  Input location: (79.344,28.424)
  Legal location: (79.344,26.752)
  Displacement:   1.672 um ( 1.00 row height)
Cell: uart_tx_inst/ZBUF_202_inst_18 (NBUFFX2_HVT)
  Input location: (87.248,36.784)
  Legal location: (86.032,36.784)
  Displacement:   1.216 um ( 0.73 row height)
Cell: ZBUF_41_inst_9 (NBUFFX2_HVT)
  Input location: (64.6,50.16)
  Legal location: (65.664,50.16)
  Displacement:   1.064 um ( 0.64 row height)
Cell: uart_tx_inst/U23 (AO22X1_HVT)
  Input location: (85.12,36.784)
  Legal location: (84.208,36.784)
  Displacement:   0.912 um ( 0.55 row height)
Cell: ZBUF_26_inst_10 (NBUFFX2_HVT)
  Input location: (57.304,46.816)
  Legal location: (56.392,46.816)
  Displacement:   0.912 um ( 0.55 row height)
Cell: ZBUF_14_inst_8 (NBUFFX2_HVT)
  Input location: (74.024,48.488)
  Legal location: (74.784,48.488)
  Displacement:   0.760 um ( 0.45 row height)
Cell: uart_rx_inst/U4 (NAND3X2_HVT)
  Input location: (61.712,21.736)
  Legal location: (60.952,21.736)
  Displacement:   0.760 um ( 0.45 row height)
Cell: uart_tx_inst/ZBUF_61_inst_16 (NBUFFX2_HVT)
  Input location: (88.92,21.736)
  Legal location: (88.312,21.736)
  Displacement:   0.608 um ( 0.36 row height)
Cell: ZBUF_7_inst_11 (NBUFFX2_HVT)
  Input location: (72.504,43.472)
  Legal location: (73.112,43.472)
  Displacement:   0.608 um ( 0.36 row height)
Cell: ZBUF_11_inst_12 (NBUFFX2_HVT)
  Input location: (74.48,46.816)
  Legal location: (73.872,46.816)
  Displacement:   0.608 um ( 0.36 row height)

Information: The net parasitics of block impl_top are cleared. (TIM-123)
Information: The stitching and editing of coupling caps is turned OFF for design 'uart.dlib:pre_placement.design'. (TIM-125)
Information: Design Average RC for design pre_placement  (NEX-011)
Information: r = 1.789803 ohm/um, via_r = 0.487609 ohm/cut, c = 0.062405 ff/um, cc = 0.000000 ff/um (X dir) (NEX-017)
Information: r = 1.785715 ohm/um, via_r = 0.598789 ohm/cut, c = 0.069908 ff/um, cc = 0.000000 ff/um (Y dir) (NEX-017)
Information: The RC mode used is VR for design 'impl_top'. (NEX-022)
Information: Update timing completed net estimation for all the timing graph nets (TIM-111)
Information: Net estimation statistics: timing graph nets = 340, routed nets = 0, across physical hierarchy nets = 0, parasitics cached nets = 340, delay annotated nets = 0, parasitics annotated nets = 0, multi-voltage nets = 0. (TIM-112)
END_FUNC: legalize_placement_run_core CPU:   3233 s ( 0.90 hr) ELAPSE:  43930 s (12.20 hr) MEM-PEAK:   950 Mb Sat Mar  7 07:54:37 2026
Information: Initializing classic cellmap without advanced rules enabled and without PDC enabled
Information: The following app options are used in cellmap
        place.legalize.enable_color_aware_placement : false
        place.legalize.use_nll_query_cm : false
        place.legalize.enable_advanced_legalizer : false
        place.legalize.enable_prerouted_net_check : true
        place.legalize.enable_advanced_prerouted_net_check : false
        place.legalize.always_continue : true
        place.legalize.limit_legality_checks : false
        place.common.pnet_aware_density : 1.0000
        place.common.pnet_aware_min_width : 0.0000
        place.common.pnet_aware_layers : {}
        place.common.use_placement_model : false
        place.common.enable_advanced_placement_model : true
        cts.placement.cell_spacing_rule_style : maximum
Total 0.0674 seconds to build cellmap data

Information: Ending place_opt / final_opto / Legalization (1) (FLW-8001)
Information: Time: 2026-03-07 07:54:37 / Session: 12.20 hr / Command: 0.01 hr / Memory: 951 MB (FLW-8100)

Information: Starting place_opt / final_opto / Optimization (2) (FLW-8000)
Information: Time: 2026-03-07 07:54:37 / Session: 12.20 hr / Command: 0.01 hr / Memory: 951 MB (FLW-8100)

Place-opt optimization Phase 54 Iter  1         0.00        0.00      0.00         0       1029.54  113381768.00         296             12.20       950

CCL: Total Usage Adjustment : 1
INFO: Derive row count 24 from GR congestion map (96/4)
INFO: Derive col count 24 from GR congestion map (96/4)
Convert timing mode ...
Information: Initializing classic cellmap without advanced rules enabled and without PDC enabled
Information: The following app options are used in cellmap
        place.legalize.enable_color_aware_placement : false
        place.legalize.use_nll_query_cm : false
        place.legalize.enable_advanced_legalizer : false
        place.legalize.enable_prerouted_net_check : true
        place.legalize.enable_advanced_prerouted_net_check : false
        place.legalize.always_continue : true
        place.legalize.limit_legality_checks : false
        place.common.pnet_aware_density : 1.0000
        place.common.pnet_aware_min_width : 0.0000
        place.common.pnet_aware_layers : {}
        place.common.use_placement_model : false
        place.common.enable_advanced_placement_model : true
        cts.placement.cell_spacing_rule_style : maximum
Total 0.0573 seconds to build cellmap data
Total 0.0108 seconds to load 864 cell instances into cellmap
Moveable cells: 296; Application fixed cells: 0; Macro cells: 0; User fixed cells: 568
Average cell width 2.0802, cell height 1.6720, cell area 3.4782 for total 296 placed and application fixed cells
Information: Initializing classic cellmap without advanced rules enabled and without PDC enabled
Information: The following app options are used in cellmap
        place.legalize.enable_color_aware_placement : false
        place.legalize.use_nll_query_cm : false
        place.legalize.enable_advanced_legalizer : false
        place.legalize.enable_prerouted_net_check : true
        place.legalize.enable_advanced_prerouted_net_check : false
        place.legalize.always_continue : true
        place.legalize.limit_legality_checks : false
        place.common.pnet_aware_density : 1.0000
        place.common.pnet_aware_min_width : 0.0000
        place.common.pnet_aware_layers : {}
        place.common.use_placement_model : false
        place.common.enable_advanced_placement_model : true
        cts.placement.cell_spacing_rule_style : maximum
Total 0.0681 seconds to build cellmap data
Total 0.0149 seconds to load 864 cell instances into cellmap
Moveable cells: 296; Application fixed cells: 0; Macro cells: 0; User fixed cells: 568
Average cell width 2.0802, cell height 1.6720, cell area 3.4782 for total 296 placed and application fixed cells
Place-opt optimization Phase 55 Iter  1         0.00        0.00      0.00         0       1029.54  113381768.00         296             12.20       950
Place-opt optimization Phase 55 Iter  2         0.00        0.00      0.00         0       1029.54  113381768.00         296             12.20       950
Place-opt optimization Phase 55 Iter  3         0.00        0.00      0.00         0       1029.54  113381768.00         296             12.20       950
Place-opt optimization Phase 55 Iter  4         0.00        0.00      0.00         0       1029.54  113381768.00         296             12.20       950
Place-opt optimization Phase 55 Iter  5         0.00        0.00      0.00         0       1029.54  113381768.00         296             12.20       950

Place-opt optimization Phase 56 Iter  1         0.00        0.00      0.00         0       1029.54  113381768.00         296             12.20       950
Place-opt optimization summary            SETUP-COST    R2R-COST HOLD-COST LDRC-COST          AREA     LEAKAGE     INSTCNT            ELAPSE       MEM
Place-opt optimization Phase 56 Iter  2         0.00        0.00      0.00         0       1029.54  113381768.00         296             12.20       950

Place-opt optimization Phase 57 Iter  1         0.00        0.00      0.00         0       1029.54  113381768.00         296             12.20       950
Place-opt optimization Phase 57 Iter  2         0.00        0.00      0.00         0       1029.54  113381768.00         296             12.20       950

Information: Ending place_opt / final_opto / Optimization (2) (FLW-8001)
Information: Time: 2026-03-07 07:54:39 / Session: 12.20 hr / Command: 0.01 hr / Memory: 951 MB (FLW-8100)

Information: Starting place_opt / final_opto / Legalization (2) (FLW-8000)
Information: Time: 2026-03-07 07:54:39 / Session: 12.20 hr / Command: 0.01 hr / Memory: 951 MB (FLW-8100)

START_FUNC : legalize_placement_pre_run_core CPU :   1743 s ( 0.48 hr) ELAPSE :  43932 s (12.20 hr) MEM-PEAK :   950 Mb
END_FUNC : legalize_placement_pre_run_core CPU :   1743 s ( 0.48 hr) ELAPSE :  43932 s (12.20 hr) MEM-PEAK :   950 Mb
START_FUNC: legalize_placement_run_core CPU:   3235 s ( 0.90 hr) ELAPSE:  43932 s (12.20 hr) MEM-PEAK:   950 Mb Sat Mar  7 07:54:39 2026
Place-opt optimization Phase 60 Iter  1         0.00        0.00      0.00         0       1029.54  113381768.00         296             12.20       950
nplDplc2Placer::setParam(effort,1)
nplDplc2Placer::setParam(debug,0)
nplDplc2Placer::setParam(site_check,2)
nplDplc2Placer::setParam(app_firm_wgt,0)
Warning: Routing direction of metal layer PO is neither "horizontal" nor "vertical".  PDC checks will not be performed on this layer. (PDC-003)
PDC app_options settings =========
        place.legalize.enable_prerouted_net_check: 1
        place.legalize.num_tracks_for_access_check: 1
        place.legalize.use_eol_spacing_for_access_check: 0
        place.legalize.allow_touch_track_for_access_check: 1
        place.legalize.reduce_conservatism_in_eol_check: 0
        place.legalize.preroute_shape_merge_distance: 0.0
        place.legalize.enable_non_preferred_direction_span_check: 0

Layer M1: cached 0 shapes out of 90 total shapes.
Layer M2: cached 43 shapes out of 43 total shapes.
Cached 2453 vias out of 5285 total vias.

Legalizing Top Level Design impl_top ... 
Information: Initializing classic cellmap without advanced rules enabled and without PDC enabled
Information: The following app options are used in cellmap
        place.legalize.enable_color_aware_placement : false
        place.legalize.use_nll_query_cm : false
        place.legalize.enable_advanced_legalizer : false
        place.legalize.enable_prerouted_net_check : true
        place.legalize.enable_advanced_prerouted_net_check : false
        place.legalize.always_continue : true
        place.legalize.limit_legality_checks : false
        place.common.pnet_aware_density : 1.0000
        place.common.pnet_aware_min_width : 0.0000
        place.common.pnet_aware_layers : {}
        place.common.use_placement_model : false
        place.common.enable_advanced_placement_model : true
        cts.placement.cell_spacing_rule_style : maximum
Total 0.0631 seconds to build cellmap data
Information: Creating classic rule checker.
Warning: Routing direction of metal layer PO is neither "horizontal" nor "vertical".  PDC checks will not be performed on this layer. (PDC-003)
=====> Processed 59 ref cells (19 fillers) from library

Bounds/Regions In This Design:
        Area    Num Cells  Exclusive Name
 (square um)
     22302.2          864        Yes DEFAULT_VA

Starting legalizer.
Warning: Exclusive bound 'DEFAULT' has no cells.
Optimizing Exclusive Bound 'DEFAULT_VA' (2/2)
    Done Exclusive Bound 'DEFAULT_VA' (2/2) (0 sec)
Legalization complete (0 total sec)

****************************************
  Report : Placement Attempts
  Site   : unit
****************************************

number of cells:                    864
number of references:                59
number of site rows:                 89
number of locations attempted:     3440
number of locations failed:         817  (23.8%)

Legality of references at locations:
34 references had failures.

Worst 10 references by total failures:
 CELLS       UNFLIPPED                       FLIPPED                REFERENCE NAME
         ATTEMPTS  FAILURES            ATTEMPTS  FAILURES           
    73        584       110 ( 18.8%)        200        28 ( 14.0%)  DFFX1_HVT
    27        216        70 ( 32.4%)        152        44 ( 28.9%)  AO22X1_RVT
    11         88        40 ( 45.5%)         40        16 ( 40.0%)  AO221X1_RVT
    11         88        39 ( 44.3%)         48        15 ( 31.2%)  OA222X1_RVT
    10         80        30 ( 37.5%)         56        16 ( 28.6%)  AO222X1_RVT
    12         96        23 ( 24.0%)         56        20 ( 35.7%)  AO22X1_HVT
    20        160        26 ( 16.2%)         72        12 ( 16.7%)  INVX0_HVT
    15        120        21 ( 17.5%)         64        16 ( 25.0%)  NAND2X0_RVT
    11         88        22 ( 25.0%)         32        12 ( 37.5%)  NBUFFX2_HVT
    11         88        21 ( 23.9%)         32         8 ( 25.0%)  NAND3X0_HVT

Worst 10 references by failure rate:
 CELLS       UNFLIPPED                       FLIPPED                REFERENCE NAME
         ATTEMPTS  FAILURES            ATTEMPTS  FAILURES           
     1          8         5 ( 62.5%)          0         0 (  0.0%)  OA21X1_HVT
     1          8         5 ( 62.5%)          0         0 (  0.0%)  AOI22X1_RVT
     1          8         6 ( 75.0%)          8         4 ( 50.0%)  OAI22X1_RVT
     1          8         4 ( 50.0%)          0         0 (  0.0%)  NOR2X0_RVT
     2         16         9 ( 56.2%)         16         7 ( 43.8%)  OA21X1_RVT
     2         16         8 ( 50.0%)          8         3 ( 37.5%)  AND3X1_RVT
    11         88        40 ( 45.5%)         40        16 ( 40.0%)  AO221X1_RVT
     2         16         8 ( 50.0%)         16         6 ( 37.5%)  OA222X1_HVT
     6         48        21 ( 43.8%)         16         6 ( 37.5%)  OA221X1_HVT
     3         24        11 ( 45.8%)          8         2 ( 25.0%)  OR3X1_LVT

Legality of references in rows:
0 references had row failures.

****************************************
  Report : Cell Displacements
****************************************

number of cells aggregated:         296 (4051 total sites)
avg row height over cells:        1.672 um
rms cell displacement:            0.000 um ( 0.00 row height)
rms weighted cell displacement:   0.000 um ( 0.00 row height)
max cell displacement:            0.000 um ( 0.00 row height)
avg cell displacement:            0.000 um ( 0.00 row height)
avg weighted cell displacement:   0.000 um ( 0.00 row height)
number of cells moved:                0
number of large displacements:        0
large displacement threshold:     3.000 row height

Displacements of worst 10 cells:
Cell: uart_tx_inst/U64 (AND2X1_HVT)
  Input location: (95.304,5.016)
  Legal location: (95.304,5.016)
  Displacement:   0.000 um ( 0.00 row height)
Cell: uart_tx_inst/U36 (AND2X1_HVT)
  Input location: (94.544,35.112)
  Legal location: (94.544,35.112)
  Displacement:   0.000 um ( 0.00 row height)
Cell: uart_tx_inst/U29 (AND2X1_HVT)
  Input location: (79.952,45.144)
  Legal location: (79.952,45.144)
  Displacement:   0.000 um ( 0.00 row height)
Cell: uart_tx_inst/U27 (AND2X1_HVT)
  Input location: (78.584,43.472)
  Legal location: (78.584,43.472)
  Displacement:   0.000 um ( 0.00 row height)
Cell: uart_rx_inst/U29 (AND2X1_HVT)
  Input location: (53.352,25.08)
  Legal location: (53.352,25.08)
  Displacement:   0.000 um ( 0.00 row height)
Cell: uart_rx_inst/U94 (AND2X1_HVT)
  Input location: (68.552,11.704)
  Legal location: (68.552,11.704)
  Displacement:   0.000 um ( 0.00 row height)
Cell: uart_rx_inst/U88 (AND2X1_HVT)
  Input location: (71.288,1.672)
  Legal location: (71.288,1.672)
  Displacement:   0.000 um ( 0.00 row height)
Cell: uart_rx_inst/U74 (AND2X1_HVT)
  Input location: (57.456,6.688)
  Legal location: (57.456,6.688)
  Displacement:   0.000 um ( 0.00 row height)
Cell: uart_rx_inst/U66 (AND2X1_HVT)
  Input location: (53.808,3.344)
  Legal location: (53.808,3.344)
  Displacement:   0.000 um ( 0.00 row height)
Cell: uart_rx_inst/U59 (AND2X4_HVT)
  Input location: (61.104,16.72)
  Legal location: (61.104,16.72)
  Displacement:   0.000 um ( 0.00 row height)

Information: The net parasitics of block impl_top are cleared. (TIM-123)
Information: The stitching and editing of coupling caps is turned OFF for design 'uart.dlib:pre_placement.design'. (TIM-125)
Information: Design Average RC for design pre_placement  (NEX-011)
Information: r = 1.789803 ohm/um, via_r = 0.487609 ohm/cut, c = 0.062405 ff/um, cc = 0.000000 ff/um (X dir) (NEX-017)
Information: r = 1.785715 ohm/um, via_r = 0.598789 ohm/cut, c = 0.069908 ff/um, cc = 0.000000 ff/um (Y dir) (NEX-017)
Information: The RC mode used is VR for design 'impl_top'. (NEX-022)
Information: Update timing completed net estimation for all the timing graph nets (TIM-111)
Information: Net estimation statistics: timing graph nets = 340, routed nets = 0, across physical hierarchy nets = 0, parasitics cached nets = 340, delay annotated nets = 0, parasitics annotated nets = 0, multi-voltage nets = 0. (TIM-112)
END_FUNC: legalize_placement_run_core CPU:   3236 s ( 0.90 hr) ELAPSE:  43933 s (12.20 hr) MEM-PEAK:   950 Mb Sat Mar  7 07:54:40 2026
Information: Initializing classic cellmap without advanced rules enabled and without PDC enabled
Information: The following app options are used in cellmap
        place.legalize.enable_color_aware_placement : false
        place.legalize.use_nll_query_cm : false
        place.legalize.enable_advanced_legalizer : false
        place.legalize.enable_prerouted_net_check : true
        place.legalize.enable_advanced_prerouted_net_check : false
        place.legalize.always_continue : true
        place.legalize.limit_legality_checks : false
        place.common.pnet_aware_density : 1.0000
        place.common.pnet_aware_min_width : 0.0000
        place.common.pnet_aware_layers : {}
        place.common.use_placement_model : false
        place.common.enable_advanced_placement_model : true
        cts.placement.cell_spacing_rule_style : maximum
Total 0.0557 seconds to build cellmap data

Information: Ending place_opt / final_opto / Legalization (2) (FLW-8001)
Information: Time: 2026-03-07 07:54:40 / Session: 12.20 hr / Command: 0.01 hr / Memory: 951 MB (FLW-8100)

Place-opt optimization Phase 62 Iter  1         0.00        0.00      0.00         0       1029.54  113381768.00         296             12.20       950

Place-opt optimization Phase 63 Iter  1         0.00        0.00      0.00         0       1029.54  113381768.00         296             12.20       950

Information: Ending place_opt / final_opto (FLW-8001)
Information: Time: 2026-03-07 07:54:40 / Session: 12.20 hr / Command: 0.01 hr / Memory: 951 MB (FLW-8100)
Number of Site types in the design = 1
Setting up Chip Core
Chip Core shape: (0 0) (1498720 1488080)
Number of VARs = 1
Number of unique PDs = 1
Number of Power Domains = 1
Number of Voltage Areas = 1
Number of supply Nets = 2
Number of used supplies = 0
Blocked VAs: 
INFO: Running FTB cleanup in end of npo flow.
Enable dominated scenarios

Place-opt optimization complete                 0.00        0.00      0.00         0       1029.54  113381768.00         296             12.20       950
Co-efficient Ratio Summary:
4.193421605155  6.578038173730  2.479639287277  7.744187540401  0.485050000353  3.179565833784  5.567214754587  2.894454287461  6.565921223252  9.017938005874  778.589143293398  6.343949804579  7.812314908527  4.420834112383  1.811562290796
9.922502608324  6.462395006413  8.237022122693  8.718402961931  4.242940666909  6.875278996466  1.318072329441  4.657879222478  7.635891827610  9.113511869609  095.463573142700  1.484427732259  4.500662903750  2.060531697663  4.588424796212
2.011679507759  6.784739677862  2.430567170402  3.879771500032  8.450958970596  1.115123714701  2.873968927205  1.355121598278  3.513982697572  7.251900473334  894.520307786760  9.731611636798  4.385382466981  9.999761759148  7.347089563210
6.393266767907  8.063326983131  4.288630187880  5.817928323007  2.343539122627  0.037326705045  0.494780240392  8.173631513985  2.544054426410  0.066111627471  216.087608907466  5.306345167197  0.882096768572  5.367847591334  1.826356890279
2.637726098236  5.283406261425  3.867463816766  5.291991874740  2.249505136567  9.662150275706  1.856261198134  4.610361714723  1.210715822142  6.577675360418  689.191647762469  7.562030646796  1.193949660867  3.380650488682  3.459474258902
4.093368484394  9.944897111549  0.206860149244  4.522000104051  6.919095345907  6.892197041709  5.120915900067  4.435466992308  4.268142606444  8.834608693265  682.946509579456  7.350476999954  1.168299101288  8.717343371851  0.993958070837
3.361785277268  3.894677263765  2.116996953270  7.452994665626  2.309097940979  5.558072613699  3.113139776351  0.072170862525  4.751594757158  0.064933720937  578.292473635135  8.115185581004  8.267326333424  3.493832924350  2.162161983179
6.121424225277  3.111567821040  8.235191416280  3.561612866938  0.220195692842  6.031325858445  0.248025513631  3.593595113535  4.075634528401  8.041235275181  623.422115400041  7.760302452592  3.872483581644  8.557737188484  8.373114348293
6.801055033867  1.494124225421  2.305316610900  7.627270112330  8.107178452560  7.471109985851  4.743640423276  4.676979334932  4.216938786544  1.199980497230  418.942325707546  8.503006838023  2.141187127813  0.334141833553  7.515567894379
0.989360291367  0.264254590202  0.920846499784  7.149511774674  5.773404731712  7.472142198159  2.074004443314  1.463713704135  2.498436149488  1.019862474527  340.183194102827  9.245284542816  2.595495769363  0.086963367403  1.037849893641
5.157027411336  1.564766944246  9.766505239565  9.210874802189  6.473823894401  4.638324531610  4.193421605155  6.578038073730  2.479639293666  7.744188040401  406.627163235331  7.956572297864  6.721493958728  9.445438746165  6.592123586390
1.793750587431  0.467080093398  6.343950985160  7.812396408527  4.420834112383  1.811560490796  9.922502608324  6.462395806413  8.237022137669  8.718403261931  873.258580490968  7.527888565022  1.807250444146  5.787932247876  3.589183922191
1.351036960963  7.341410942700  1.484438813840  4.500644403750  2.060531697663  4.588422996212  2.011679507759  6.784739477862  2.430567185478  3.879772800032  294.059311859611  1.512360399537  7.396810220513  5.512169827835  1.398260918372
5.190997333443  6.408244586760  9.731622717389  4.385364966981  9.999761759148  7.347087763210  6.393266767907  8.063326783131  4.288630192856  5.817929623007  683.317436062700  3.732669423913  9.478042539281  7.363161398525  4.405443802100
6.611012747185  8.965545707466  5.306356248788  0.882078268572  5.367847591334  1.826354090279  2.637726098236  5.283406061425  3.867463821732  5.291992174740  673.914037456796  6.215016499027  5.626137313446  1.036181472312  1.071583475365
7.767486041822  1.079584562469  7.562041727387  1.193921160867  3.380650488682  3.459472458902  4.093368484394  9.944897911549  0.206860154210  4.522001404051  040.963058390768  9.219793099360  2.091518506744  3.546609230842  6.814261805588
3.460719326522  4.824446379456  7.350487070545  1.168271601288  8.717343371851  0.993956270837  3.361785277268  3.894677263765  2.116996953270  7.452994765626  689.021911297955  5.807363160176  1.313996235100  7.217097352547  5.159476976900
6.493222093711  0.170310435135  8.115196662695  8.267308833424  3.493832924350  2.162169183179  6.121424225277  3.111567821040  8.235191416280  3.561612966938  471.131786484260  3.132687645747  4.802570963135  9.359522453540  7.563453001280
4.123477518126  5.300052200041  7.760313533183  3.872465081644  8.557737188484  8.373112548293  6.801055033867  1.494124325421  2.305316610058  7.627270212330  46.398159725607  4.711191659759  7.436423832764  6.769795449324  2.169389501551
1.999899972300  5.082026250754  6.850301791961  4.214116962781  3.033414183355  3.751556509437  9.098936029136  7.026425559020  2.092084649026  4.714951277467  00.090879931712  7.472244999394  2.074023043314  1.463714904135  2.498438933099
1.019861974527  9.820610319028  2.792452956234  0.725954772693  6.300869633674  0.310378470936  4.151570274113  3.615647769442  4.697665052443  6.592108848021  44.964757589440  1.463934254306  0.419361760515  5.657804917373  0.247965728727
7.774418754040  1.048505000035  3.317956583378  4.556721475458  7.289445438746  1.656592121786  3.901793750587  4.310467180093  3.986343950033  1.607812496408  17.061673541123  8.318217405142  9.699244626083  2.464624050064  1.382372021226
9.387184029619  3.142429406669  0.968752789964  6.613180723294  4.146578793224  7.876358918112  2.191135103696  0.963734241094  2.700148443939  3.840450164440  92.829435731697  6.634680223131  2.122030279507  7.596785839677  8.622432367170
4.023879771500  0.328450958970  5.961115123714  7.012873968927  2.051355121698  2.783513982681  1.837251909973  3.344364182445  8.676097316203  1.738943953649  29.638745976175  9.148836041453  3.210658926676  7.907807432698  3.131420663018
7.880581792832  3.007234353912  2.627003732670  5.045049478024  0.392817363161  3.985254405441  0.021006611012  7.471858065545  7.074665306332  2.487880982078  89.504009678475  9.133510506227  9.027945977260  9.823653934062  6.142530474638
1.676652919918  7.474022495051  3.656796621502  7.570618562611  9.813446103618  1.472312107158  1.675365776748  6.041822207958  4.562469756280  1.727387219392  74.355429380650  4.886925792159  4.589043693368  4.843940044897  1.115492006860
1.492444522000  1.040516919095  3.459076892197  0.417095120915  9.000674435466  0.923084268142  6.900558834607  1.932652348244  4.637945673580  8.707054611682  34.358034871734  3.371953332072  6.270856936178  5.277269489467  7.263767011699
6.953270745299  4.665626230909  7.940979555807  2.613699311313  9.776351007217  0.962525475159  4.741769006493  2.220937210170  3.104351358191  1.966626058267  93.530198434938  3.292537364308  6.918336561214  2.422528831115  6.782106882351
9.141628035616  1.286693802201  9.569284260313  2.585844502480  2.551363135935  9.521353540756  3.451201280412  3.477518226530  0.052200041752  0.313533283387  87.307662448557  7.371986181050  1.125401536801  0.550339771494  1.242256012305
3.166109007627  2.701123308107  1.784525607471  1.099858514743  6.404232764676  9.794349324216  9.387701551199  9.899972400508  2.026250754661  0.301791061421  04.816473813033  4.141935896438  5.565013979098  9.360292467026  4.254592002092
0.846499784714  9.511774674577  3.404731712747  2.142198159207  4.004443314146  3.713804135249  8.436133099101  9.861974527982  0.610319028279  2.452956234072  75.627356336300  8.696438693045  3.784728964151  5.702742233615  6.476696224697
6.650523956592  1.087480218964  7.382389440146  3.832453161041  9.342160515565  7.803817373024  7.963928727777  4.418754040104  8.505000035331  7.956583378455  78.542820387289  4.454489314381  5.921236463901  7.937506974310  4.670802733986
3.439509851607  8.123964085274  4.208341123831  8.115604907969  9.225026083246  4.623950064138  2.370221226938  7.184029619314  2.429406669096  8.752789964661  42.135308744146  5.787034190501  3.589100722191  1.351037060963  7.341412742700
1.484438813840  4.500644403750  2.060531697663  4.588422996212  2.011679507759  6.784739677862  2.430567170402  3.879771500032  8.450958970596  1.115123714701  39.067755072051  3.551318835418  5.139845411837  2.519090833344  3.640826258676
0.973162271738  9.438536496698  1.999976175914  8.734708776321  0.639326676790  7.806332698313  1.428863018788  0.581792832300  7.234353912262  7.003732670504  61.722856040392  8.173733566610  2.544073010021  0.066111227471  8.589657257074
6.653063562487  8.808820782685  7.253678475913  3.418263540902  7.926377260982  3.652834062614  2.538674638167  6.652919918747  4.022495051365  6.796621502757  17.413792919813  4.461138034107  3.121090181675  3.657768586041  8.221071384562
4.697562041727  3.871193921160  8.673380650488  6.823459472458  9.024093368484  3.949944897111  5.490206860149  2.444522000104  0.516919095345  9.076892197041  81.289375959000  6.744456513658  0.842600026900  5.588347171932  6.522484244637
9.456735048707  0.545116827160  1.288871734337  1.851099395627  0.837336178527  7.268389467726  3.765211699695  3.270745299466  5.626230909794  0.979555807261  47.221289939776  3.510174023697  5.254770194741  7.690065032220  9.371103503104
3.513581151966  6.269582673088  3.342434938329  2.435021621691  8.317961214242  2.527731115678  2.104082351914  1.628035616128  6.693802201956  9.284260313258  69.773190602551  3.631451202256  3.535426163451  2.012805223477  5.181267100052
2.000417760313  5.331833872465  0.816448557737  1.884848373112  5.482936801055  0.338671494124  2.254212305316  6.109007627270  1.123308107178  4.525607471109  09.813213236404  2.327748612429  3.493261769387  7.015512099899  9.723007882026
2.507546850301  7.919614214116  9.627813033414  1.833553751556  5.094379098936  0.291367026425  4.590202092084  6.499784714951  1.774674577340  4.731712747214  32.219668874004  4.433243316448  8.041371098436  1.330992119861  9.745271620610
3.190282792452  9.562340725954  7.726936300869  6.336740310378  4.709364151570  2.741133615647  6.694424697665  0.523956592108  7.480218964738  2.389440146383  35.869786219342  1.605257500538  8.173759847963  9.287278874418  7.540403848505
0.000353317956  5.833784556721  4.754587289445  4.387461656592  1.217863901793  7.505874310467  0.800933986343  9.509851607812  3.964085274420  8.341123831811  67.377145499225  0.260934317358  9.500660982370  2.212260487184  0.296195942429
4.066690968752  7.899646613180  7.232944146578  7.932247876358  9.181122191135  1.036960963734  1.410942700148  4.438813840450  0.644403750206  0.531697663458  95.557038922011  6.795179449419  7.396797222430  5.671705123879  7.715002128450
9.589705961115  1.237147012873  9.689272051355  1.216982783513  9.826811837251  9.099733344364  0.824458676097  3.162271738943  8.536496698199  9.976175914873  58.315839010639  3.266869850531  3.326902731428  8.630188980581  7.928325807234
3.539122627003  7.326705045049  4.780240392817  3.631613985254  4.054410021006  6.110127471858  9.655457074665  3.063562487880  8.820782685725  3.678475913341  93.963575827926  3.772701776387  8.340645742538  6.746382776652  9.199189274022
4.950513656796  6.215027570618  5.626119813446  1.036181472312  1.071581675365  7.767486041822  1.079584562469  7.562041727387  1.193921160867  3.380650488682  45.822890389024  0.933786796674  9.448990715490  2.068602592444  5.220003840516
9.190953459076  8.921970417095  1.209159000674  4.354660923084  2.681426900558  8.346071932652  2.482444637945  6.735048707054  5.116827160128  8.871734337185  21.221022070837  3.361887120993  3.894696863765  2.116997053270  7.452996465626
2.309097940979  5.558072613699  3.113139776351  0.072170962525  4.751594741769  0.064932220937  1.101703104351  3.581151966626  9.582673088334  2.434938329243  61.544382718317  9.612244375252  7.311175382104  0.823510241628  0.356163086693
8.022019569284  2.603132585844  5.024802551363  1.359359521353  5.407563451201  2.804123477518  1.265300052200  0.417760313533  1.833872465081  6.448557737188  59.711807925482  9.368112403063  6.714960842254  2.123054266109  0.076274501123

Place-opt final QoR
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
7: impl_clk

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
    1   *   0.0000     0.0000   0.0000      0   0.0000     0.0000      0        0     0.0000        0  113381768
----------------------------------------------------------------------------------------------------------------------------------------------------------------------
DESIGN QOR 
----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Scene  PG      WNS        TNS   R2RTNS    NSV      WHV        THV    NHV  MaxTrnV   MaxTranC  MaxCapV    Leakage         Area    InstCnt     BufCnt     InvCnt
    *   *   0.0000     0.0000   0.0000      0   0.0000     0.0000      0        0     0.0000        0  113381768      1029.54        296         11         28
----------------------------------------------------------------------------------------------------------------------------------------------------------------------

Place-opt final QoR Summary         WNS        TNS   R2RTNS    NSV      WHV        THV    NHV  MaxTrnV  MaxCapV    Leakage         Area    InstCnt
Place-opt final QoR Summary      0.0000     0.0000   0.0000      0   0.0000     0.0000      0        0        0  113381768      1029.54        296

Place-opt command complete                CPU:  1745 s (  0.48 hr )  ELAPSE: 43933 s ( 12.20 hr )  MEM-PEAK:   950 MB
Place-opt command statistics  CPU=25 sec (0.01 hr) ELAPSED=27 sec (0.01 hr) MEM-PEAK=0.928 GB
Information: Running auto PG connection. (NDM-099)
Information: Ending 'place_opt' (FLW-8001)
Information: Time: 2026-03-07 07:54:41 / Session: 12.20 hr / Command: 0.01 hr / Memory: 951 MB (FLW-8100)
1
Information: 2 out of 3 MSG-3549 messages were not printed due to limit 1  (MSG-3913)
report_timing****************************************
Report : timing
        -path_type full
        -delay_type max
        -max_paths 1
        -report_by design
Design : impl_top
Version: R-2020.09-SP2
Date   : Sat Mar  7 07:58:04 2026
****************************************

  Startpoint: uart_rx_inst/clk_cnt_reg[1] (rising edge-triggered flip-flop clocked by impl_clk)
  Endpoint: uart_rx_inst/clk_cnt_reg[14] (rising edge-triggered flip-flop clocked by impl_clk)
  Mode: default
  Corner: default
  Scenario: default
  Path Group: impl_clk
  Path Type: max

  Point                                            Incr      Path  
  ------------------------------------------------------------------------
  clock impl_clk (rise edge)                       0.00      0.00
  clock network delay (ideal)                      0.00      0.00

  uart_rx_inst/clk_cnt_reg[1]/CLK (DFFX2_HVT)      0.00      0.00 r
  uart_rx_inst/clk_cnt_reg[1]/Q (DFFX2_HVT)        0.10      0.10 r
  uart_rx_inst/U16/Y (NAND3X0_HVT)                 0.06      0.16 f
  uart_rx_inst/U17/Y (INVX0_HVT)                   0.04      0.20 r
  uart_rx_inst/U71/Y (NAND3X0_HVT)                 0.09      0.29 f
  uart_rx_inst/U73/Y (INVX0_HVT)                   0.03      0.33 r
  uart_rx_inst/U78/Y (NAND3X0_HVT)                 0.07      0.40 f
  uart_rx_inst/U79/Y (INVX0_HVT)                   0.04      0.44 r
  uart_rx_inst/U85/Y (NAND3X0_HVT)                 0.09      0.53 f
  uart_rx_inst/U87/Y (INVX0_HVT)                   0.04      0.57 r
  uart_rx_inst/U92/Y (NAND3X0_HVT)                 0.09      0.66 f
  uart_rx_inst/U93/Y (INVX0_HVT)                   0.04      0.69 r
  uart_rx_inst/U98/Y (NAND3X0_HVT)                 0.08      0.77 f
  uart_rx_inst/U99/Y (INVX0_HVT)                   0.03      0.80 r
  uart_rx_inst/U104/Y (AND3X1_HVT)                 0.06      0.87 r
  uart_rx_inst/U105/Y (AO22X1_HVT)                 0.05      0.92 r
  uart_rx_inst/clk_cnt_reg[14]/D (DFFX1_HVT)       0.00      0.92 r
  data arrival time                                          0.92

  clock impl_clk (rise edge)                       5.00      5.00
  clock network delay (ideal)                      0.00      5.00
  uart_rx_inst/clk_cnt_reg[14]/CLK (DFFX1_HVT)     0.00      5.00 r
  library setup time                              -0.04      4.96
  data required time                                         4.96
  ------------------------------------------------------------------------
  data required time                                         4.96
  data arrival time                                         -0.92
  ------------------------------------------------------------------------
  slack (MET)                                                4.04

