# DAY2

## Chip floorplan consideration
## floorplan view of an integrated circuit (IC)

<img width="797" height="596" alt="image" src="https://github.com/user-attachments/assets/47c6f7c3-702e-4552-871c-dd29ddad2a18" />

**1. Overall Structure**

* The outer orange box labeled “Die” represents the entire chip area — the physical boundary of the silicon die that will be packaged.

* Inside it, the inner orange box labeled “Core” represents the core area — the actual region where the logic of your design (standard cells, macros, blocks, etc.) will be placed.

* The space between die and core is typically reserved for I/O pads, power rings, or bonding connections.

**2. Blue and Gray Grid Lines**

* The horizontal and vertical blue/gray stripes represent metal layers (routing tracks) used for power distribution and signal routing.

* This grid pattern shows power rails (VDD and GND) and routing channels.

* The yellow crosses (X) mark intersections where vias (vertical connections between metal layers) are placed to connect one layer to another.

**3. Grey Blocks (DEC_P1, DEC_P2, DEC_P3, Block a, b, c)**

* These are placed design blocks or standard cell clusters.

* Each block (like DEC_P1, Block a, etc.) represents a functional module of the circuit.

  * For example:

     * DEC_P1, DEC_P2, DEC_P3 → could be decoder modules.

     * Block a, Block b, Block c → might be logic blocks connected to those decoders.

* These are placed within the core area, aligned to the standard cell rows (horizontal gray stripes).

# LAB 

## Steps to run floorplan using OpenLANE

Before running the floorplan we can have a look on the size of chaip, ulitization factor, aspect ratio , V-metal and H-metal (these are some switches for the floorplanning)

```bash
#Change the directory
cd Desktop/work/tools/openlane_working_dir/openlane/configuration

#open the file
less README.md (this file contains the variables in the synthesis, like maxfanout, LIB min etc : these are the switches)
```

<img width="1291" height="911" alt="Screenshot 2025-10-28 205257" src="https://github.com/user-attachments/assets/9b683f76-2958-4f3f-8add-c62e68b5f180" />
<img width="1283" height="907" alt="Screenshot 2025-10-28 205429" src="https://github.com/user-attachments/assets/ac4ddc17-8b5a-4cbf-b520-bec93d5793cf" />

**1. Switches for floorplan**

```bash
#open the file
less floorplan.tcl (containing the default parameter for the floorplan stage
```

<img width="1292" height="909" alt="Screenshot 2025-10-28 205628" src="https://github.com/user-attachments/assets/ab56a751-a8be-45f4-9650-31e7db755ad7" />

**2. Run 'picorv32a' design floorplan using OpenLANE flow and generate necessary outputs**

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Now we can run floorplan
run_floorplan

```
<img width="1292" height="909" alt="Screenshot 2025-10-28 210310" src="https://github.com/user-attachments/assets/4d87281e-3a5a-45e5-bf8f-883445f04ce2" />
<img width="1296" height="859" alt="Screenshot 2025-10-28 210341" src="https://github.com/user-attachments/assets/a996b573-fc8c-47bc-9d6d-ddbff6a120ab" />

**3. Review floorplan files and steps to view floorplan**

```bash
#change the directory
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/27-10_08-30/

#open the file
less config.tcl

```
<img width="1293" height="914" alt="Screenshot 2025-10-28 211515" src="https://github.com/user-attachments/assets/b767e734-ed93-4416-bd84-5fc58c7938db" />

<img width="1295" height="943" alt="Screenshot 2025-10-28 210914" src="https://github.com/user-attachments/assets/a51ff92a-2fb5-4314-b847-225b43afbca9" />

```bash
#change the directory
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/27-10_08-30/results/floorplan

#open the file
less picorv32a.floorplan.def
```
<img width="1289" height="437" alt="Screenshot 2025-10-28 211704" src="https://github.com/user-attachments/assets/041ba9d9-7c61-4f9f-b938-cd6a74aca02b" />
<img width="1287" height="911" alt="Screenshot 2025-10-28 211626" src="https://github.com/user-attachments/assets/cbfc9221-31f6-4a81-aadf-9aaebe1d06fa" />

```bash
In this image :

                      Die Area : (0  0)(660685  671405)

Here, (0  0) : denotes the lower left x value and lower left y value
      (660685  671405) : denotes upper right x - value and upper right y - value

Units Distance micron 1000 : means 1 micron = 1000 database units

so , To find the dimension of the die : divide the upper right and upper left values by 1000

Now,         Area of die in microns = Die width in microns * Die hieght in microns

                                   = (660685/1000) * (671405/1000)
                                   = 443587.212425 square microns
```


**4. floorplan layout in Magic**

To run this first we need to setup the GUI enviornment , so setup for GUI environment with X11

```bash
Step 1: Move or install VcXsrv on your Windows host (not inside Ubuntu)
https://sourceforge.net/projects/vcxsrv/

Step 2: Configure VcXsrv (XLaunch)

Step 3: Connect Ubuntu to the Display by  using the given command
export DISPLAY=$(grep -m 1 nameserver /etc/resolv.conf | awk '{print $2}'):0
xhost +

Step 4: Test the GUI works
xclock

Step 5: launch Magic

```
**Now, move towards the MAGIC**

Commands to load floorplan def in magic in another terminal

```bash
# Change directory to path containing generated floorplan def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/29-10_08-49/results/floorplan/

# Command to load the floorplan def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

<img width="1141" height="911" alt="Screenshot 2025-10-29 143338" src="https://github.com/user-attachments/assets/4a06dfd0-fc75-435d-a021-dc357af090e8" />

<img width="1152" height="935" alt="Screenshot 2025-10-29 143504" src="https://github.com/user-attachments/assets/53bcda04-88a1-46fe-a6e2-186c32641577" />

## How thw floorplane look like in magic

<img width="898" height="911" alt="image" src="https://github.com/user-attachments/assets/0c702bdb-32b4-4c06-b793-230ca52c0bb6" />

## Horizontal ports(outside the die) & Equidistant placement of ports

<img width="1151" height="938" alt="Screenshot 2025-10-29 143724" src="https://github.com/user-attachments/assets/e9cfcb9d-896f-4c51-ad93-1251af4409a1" />

## Horizontal ports metal layers set by the config.tcl

<img width="1152" height="915" alt="Screenshot 2025-10-29 143946" src="https://github.com/user-attachments/assets/ce661176-2a04-44a0-a080-394631433211" />

## Vertical ports metal layer set by congig.tcl

<img width="1153" height="939" alt="Screenshot 2025-10-29 144033" src="https://github.com/user-attachments/assets/a1d61de0-3deb-46b6-ae7a-93cbffda66ad" />

## Decap and tap cells

1. The decap cells:

* Store charge when circuits are idle

* Instantly release charge when switching occurs

* Smooth out voltage fluctuations

* Protect timing-critical cells from power droop

**Why they’re needed** 

When many logic gates switch simultaneously (especially at clock edges), they momentarily draw a lot of current from the power rails.
This causes a voltage drop (IR drop) and local noise.

2. The Tap cells:

In CMOS:

* PMOS transistors sit in n-wells, which must be tied to VDD

* NMOS transistors sit in p-substrate, which must be tied to VSS

**Why they’re needed**

If you don’t tie wells and substrate to fixed potentials, they can float, creating a parasitic device called a latch-up path — which can short VDD to GND and destroy the chip.
<img width="1156" height="941" alt="Screenshot 2025-10-29 144150" src="https://github.com/user-attachments/assets/30d190a6-7b7c-42cb-b6b2-ae3a935cef14" />

## Diogonally equidistant Tap cells

<img width="1159" height="938" alt="Screenshot 2025-10-29 143827" src="https://github.com/user-attachments/assets/61cec169-8dfe-47cb-8647-a178956cf084" />

## Unplaced standard cells at the origin

Standard cells are always placed in the placement not in the floorplan but they usually present in  the corner in the floorplan

<img width="1151" height="915" alt="Screenshot 2025-10-29 144509" src="https://github.com/user-attachments/assets/fa293307-9b34-479b-8969-14d7c972bc6e" />
<img width="1151" height="915" alt="Screenshot 2025-10-29 144717" src="https://github.com/user-attachments/assets/c463f694-5217-4054-a854-281d7de213ee" />

# Library binding and Placement

how to do placement


<img width="1919" height="973" alt="Screenshot 2025-10-29 180229" src="https://github.com/user-attachments/assets/898e3420-bf57-4135-a484-390db80d4bd9" />

<img width="1919" height="1079" alt="Screenshot 2025-10-29 180300" src="https://github.com/user-attachments/assets/4be4df90-65f8-434a-b009-fe479d3d6819" />

This image is for the connecting the actual circuits with the wores and also connecting the buffer for signal integrity
<img width="1918" height="1068" alt="image" src="https://github.com/user-attachments/assets/1bb7b75f-f9e8-414d-bede-b7097ff6d41c" />


<img width="1796" height="1026" alt="image" src="https://github.com/user-attachments/assets/f87e2b10-6fa1-480a-9b25-e1774dc77fb1" />

CEll design flow

<img width="1886" height="1051" alt="image" src="https://github.com/user-attachments/assets/3ebb7026-8247-4294-b2f5-f15540d2bc9d" />

Something about library

<img width="1787" height="1047" alt="image" src="https://github.com/user-attachments/assets/5e738e37-5a17-4503-a09e-757163f0ab7e" />
 Cell design flow of inverter

<img width="1747" height="1044" alt="Screenshot 2025-10-29 192651" src="https://github.com/user-attachments/assets/073e9b4c-f97a-4910-abfc-0bfe06c2e1e3" />

<img width="1777" height="1039" alt="Screenshot 2025-10-29 192739" src="https://github.com/user-attachments/assets/fd708aae-38f2-4e8b-a032-310fdcba1b07" />
<img width="1759" height="1051" alt="Screenshot 2025-10-29 193608" src="https://github.com/user-attachments/assets/7b5bedea-96fc-41f4-be3e-db6485bf0a4d" />
<img width="1712" height="1040" alt="Screenshot 2025-10-29 193901" src="https://github.com/user-attachments/assets/6cd9b763-1d59-45e7-8062-3b1225010a4f" />
<img width="1745" height="1034" alt="Screenshot 2025-10-29 213732" src="https://github.com/user-attachments/assets/ca2bf5c6-b583-4850-8946-a689db78d71c" />

characterisation flow (comes in the cell design)
<img width="1767" height="1068" alt="Screenshot 2025-10-29 214821" src="https://github.com/user-attachments/assets/dc5f8abf-da2a-45c9-91dc-b19d823b6102" />
<img width="1735" height="1076" alt="Screenshot 2025-10-29 214835" src="https://github.com/user-attachments/assets/508b6452-c67b-4899-a0c1-bda081f5a7a1" />






