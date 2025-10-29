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

**Switches for floorplan**

```bash
#open the file
less floorplan.tcl (containing the default parameter for the floorplan stage
```

<img width="1292" height="909" alt="Screenshot 2025-10-28 205628" src="https://github.com/user-attachments/assets/ab56a751-a8be-45f4-9650-31e7db755ad7" />

**Run 'picorv32a' design floorplan using OpenLANE flow and generate necessary outputs**

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

**Review floorplan files and steps to view floorplan**

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
**In this image :

                      Die Area : (0  0)(660685  671405)

Here, (0  0) : denotes the lower left x value and lower left y value
      (660685  671405) : denotes upper right x - value and upper right y - value

Units Distance micron 1000 : means 1 micron = 1000 database units

so , To find the dimension of the die : divide the upper right and upper left values by 1000

Now,         Area of die in microns = Die width in microns * Die hieght in microns

                                   = (660685/1000) * (671405/1000)
                                   = 443587.212425 square microns
```





















