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
