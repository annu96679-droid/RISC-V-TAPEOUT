# Day 3

## Design library cell using Magic Layout and ngspice characterization

## LAB IO Placer

In this lab we will see how can we change the configuration of layout 

```bash
#change the directory
cd  Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/30-10_6-16/results/floorplan

#open the magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &

#This contain the equidistant I/O  coniguration
```
<img width="1151" height="912" alt="Screenshot 2025-10-30 115357" src="https://github.com/user-attachments/assets/e8a70ef0-f65d-45c1-a7f8-696698c3eedc" />
<img width="1153" height="909" alt="Screenshot 2025-10-30 115242" src="https://github.com/user-attachments/assets/c827cc0a-972c-43db-aa32-75edd9e5eabb" />

**Change the configuration**

```bash
#change the directory
cd  Desktop/work/tools/openlane_working_dir/openlane/configurations

#open the floorplan file contains all the switches
less floorplan.tcl
```
<img width="1150" height="907" alt="Screenshot 2025-10-30 115511" src="https://github.com/user-attachments/assets/27be7360-0baa-499d-8ef2-4f4813a8e6df" />

```bash
#change the configuration (In the openlane)
set ::env(FP_TO_MODE) 2   (we change 1 to 2)

#again run the floorplan
run_floorplan
```bash

<img width="1153" height="911" alt="Screenshot 2025-10-30 115641" src="https://github.com/user-attachments/assets/3268a464-1e00-47e3-bccc-bb5b418f4d6b" />
<img width="1155" height="914" alt="Screenshot 2025-10-30 115721" src="https://github.com/user-attachments/assets/af05f3e0-1954-4d44-847c-0be10739b62d" />

**Now have a look to new floorplan**


```bash
#change the directory
cd  Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/30-10_6-16/results/floorplan

#open the magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
<img width="1152" height="914" alt="Screenshot 2025-10-30 120041" src="https://github.com/user-attachments/assets/10e519de-4756-49e2-8a82-72cc10328b74" />

In this image the configuration is changed , Now reset the configuration by follow the same steps

## Labs steps to git clone vsdstdcelldesign

**Clone custom inverter standard cell design from github repository**

```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Clone the repository with custom inverter design
git clone https://github.com/nickson-jose/vsdstdcelldesign

# Change into repository directory
cd vsdstdcelldesign

# Copy magic tech file to the repo directory for easy access
cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .

# Check contents whether everything is present
ls

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &

```
<img width="1145" height="909" alt="Screenshot 2025-10-30 121859" src="https://github.com/user-attachments/assets/c6b5e075-2e46-4e72-a59e-2de75fd5b86f" />
<img width="1047" height="915" alt="Screenshot 2025-10-30 122936" src="https://github.com/user-attachments/assets/6e72ce72-367b-47ff-9a6c-3cf12c18db02" />

## LAB: Introduction to Sky130 basic layers layout and LEF using inverter

**LAYOUT of INVERTER**
<img width="1244" height="941" alt="Screenshot 2025-10-30 124406" src="https://github.com/user-attachments/assets/e878c294-358e-43c8-afee-7704df99ec66" />

**POLYSILICON IDENTIFIED**

<img width="1291" height="909" alt="Screenshot 2025-10-30 124833" src="https://github.com/user-attachments/assets/e9df9cbb-e3df-47ef-ab61-ba186bb61860" />

**NMOS and PMOS identified**

when the polysilicon passes the n diffusion it's a NMOS

<img width="1245" height="908" alt="Screenshot 2025-10-30 124509" src="https://github.com/user-attachments/assets/c473fcb9-395c-4962-bd90-8a9dfef6a26b" />

<img width="1239" height="907" alt="Screenshot 2025-10-30 124541" src="https://github.com/user-attachments/assets/d7e56185-26dc-433c-a356-5f341c48615f" />

**Output Y connectivity to PMOS and NMOS drain verified**

<img width="1191" height="906" alt="Screenshot 2025-10-30 150014" src="https://github.com/user-attachments/assets/3a38e3af-a5b9-4697-83b2-52b9643cc2ad" />


**Connectivity of Source of the PMOS to Vdd line Verified**

<img width="1258" height="907" alt="image" src="https://github.com/user-attachments/assets/c394f0ca-026e-4bb9-880c-35921280d77d" />

**Connectivity of Source of the PMOS to Vdd line Verified**

<img width="1253" height="903" alt="image" src="https://github.com/user-attachments/assets/aa51ee06-170a-41f4-b7aa-fd84e16778e8" />

##  Steps to create std cell layout and extract spice netlist

**1. Now we will have a look on the DRC erors in the layout**

Here rarely I got the 4 to 5 errors which is shown in the images given below.

we can check the DRC error manually :

<img width="1152" height="909" alt="Screenshot 2025-10-30 154717" src="https://github.com/user-attachments/assets/051695f6-7e26-4afd-b887-a70d88339124" />
<img width="1153" height="917" alt="Screenshot 2025-10-30 155234" src="https://github.com/user-attachments/assets/83f5d1a4-3a50-49be-9ba5-852f28c33472" />
<img width="1151" height="912" alt="Screenshot 2025-10-30 155411" src="https://github.com/user-attachments/assets/b1dde504-73ad-4612-b386-0fb3eb551a3a" />

DRC error checked by commands on tkcon main window:

drc on
drc euclidean on
drc style drc(full)
drc check
drc catchup
drc count
drc find
drc why

<img width="1148" height="906" alt="Screenshot 2025-10-30 161950" src="https://github.com/user-attachments/assets/871e3245-a7bf-4559-b768-1a68f9e54f62" />
<img width="1153" height="909" alt="Screenshot 2025-10-30 162001" src="https://github.com/user-attachments/assets/42372abd-6267-46db-8523-212588449974" />
<img width="1150" height="908" alt="Screenshot 2025-10-30 162207" src="https://github.com/user-attachments/assets/272f7c13-57e3-40c2-91e0-8c7d1a6e9844" />

**2. Spice extraction of inverter in magic**
Commands for spice extraction of the custom inverter layout to be used in tkcon window of magic


```bash
# Check current directory
pwd

# Extraction command to extract to .ext format
extract all

# Before converting ext to spice this command enable the parasitic extraction also
ext2spice cthresh 0 rthresh 0

# Converting to ext to spice
ext2spice

```
<img width="1149" height="908" alt="Screenshot 2025-10-30 163156" src="https://github.com/user-attachments/assets/152166c1-fc64-4e6e-83b0-6f8188899e9c" />

From these commands we will get a .ext and .spice file in the vsdstdcelldesign

<img width="1142" height="200" alt="Screenshot 2025-10-30 162736" src="https://github.com/user-attachments/assets/212d401b-39a8-4deb-8203-fbd25055b947" />

**File before edit**

```bash
#open the file
vim sky130_inv.spice

```
<img width="1022" height="908" alt="Screenshot 2025-10-30 163714" src="https://github.com/user-attachments/assets/6cec40ed-7712-47da-8d12-489e4f8f96ae" />

**Editing the spice model file for analysis through simulation**

Measuring unit distance in layout grid

<img width="1023" height="916" alt="Screenshot 2025-10-30 165921" src="https://github.com/user-attachments/assets/87c459fa-16ff-4708-bbf2-a6b5ae6e778b" />

**Final edited spice file ready for ngspice simulation**

<img width="1146" height="912" alt="Screenshot 2025-10-30 183901" src="https://github.com/user-attachments/assets/ed8faef7-ed7d-40fe-8b36-f7791d2dd1c5" />

## Lab steps to characterize inverter using sky130 model files

Commands for ngspice simulation

```bash
# Command to directly load spice file for simulation to ngspice
ngspice sky130_inv.spice

# Now that we have entered ngspice with the simulation spice file loaded we just have to load the plot
plot y vs time a
```

<img width="1155" height="912" alt="Screenshot 2025-10-30 184108" src="https://github.com/user-attachments/assets/129e71e0-cf62-4c93-a6c5-aeb94e5854e2" />

**Generated Plot**

<img width="1145" height="909" alt="Screenshot 2025-10-30 184121" src="https://github.com/user-attachments/assets/7f324a02-e259-4431-a6e4-2c97f887e020" />
