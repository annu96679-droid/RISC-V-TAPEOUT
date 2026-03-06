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

