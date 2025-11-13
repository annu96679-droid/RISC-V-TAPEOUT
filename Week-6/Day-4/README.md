
# Day 4

## Pre-layout timing analysis and importance of good clock tree

## Lab steps to convert grid info to track info

**Now, we need to extract the '.lef' file from the '.mag' file.**

```bash
#open the layout
magic -T sky130A.tech sky130_inv.mag

#Open the track info
cd pdks/sky130A/livs.tech/openlane
less track.info
```
<img width="1145" height="904" alt="Screenshot 2025-11-13 094436" src="https://github.com/user-attachments/assets/d38d7575-3c3b-489c-bce9-463ef9bb17f4" />
<img width="1290" height="916" alt="Screenshot 2025-11-13 094454" src="https://github.com/user-attachments/assets/b88178e8-c1ef-4e8d-bf17-2f56db7d8002" />

The track is used during the routing stage and is essentially a trace of metal layers such as metal 1, metal 2, etc.

PNR is automated, so we need to specify where we want the routes to go. This specification is given by tracks. Each of the tracks is placed at (0.23, 0.46)um horizontally and (0.17, 0.34)um vertically for li1, metal 1, and metal 2 layers.

**for having the grid , press g on layout environment**

<img width="1287" height="909" alt="Screenshot 2025-11-13 094556" src="https://github.com/user-attachments/assets/dc80ac60-1edf-406d-8ab5-6532d8158f12" />


we can first open the tracks file and then open the tkcon window and type the help grid command.

<img width="1291" height="910" alt="Screenshot 2025-11-13 094742" src="https://github.com/user-attachments/assets/bd4db66b-b90d-4649-a1b6-e44aaba86cb7" />

There are certain guidelines to follow while making standard cells:

* The input and output ports must lie on the intersection of the vertical and horizontal tracks.

* The width of the standard cell should be an odd multiple of the track pitch, and the height should be an odd multiple of the track vertical pitch.

<img width="1287" height="916" alt="Screenshot 2025-11-13 094802" src="https://github.com/user-attachments/assets/5c6f047b-705f-4f08-98a9-1c4c386d15d6" />

## Lab steps to convert magic layout to std cell LEF

Now, we will need to decide on the port name and its values. we can set the values for different ports, and for the power and ground port, we will need to make changes in the 'attach to layer' as Metal1.

<img width="1287" height="912" alt="Screenshot 2025-11-13 095111" src="https://github.com/user-attachments/assets/f832e916-a6ea-4fac-add3-f2edfd866c39" />

## set ports class and port use attributes

```bash
#select the port (A,Y,VPWR, VGND)

#in tckon
what
port class output
port use signal

```
<img width="1284" height="908" alt="Screenshot 2025-11-13 095351" src="https://github.com/user-attachments/assets/dbc50e3e-45bd-45d4-90b0-2580ad104685" />
<img width="1147" height="913" alt="Screenshot 2025-11-13 103158" src="https://github.com/user-attachments/assets/92541793-230d-488e-b8dd-2bd8bb9e0d20" />
<img width="1150" height="909" alt="Screenshot 2025-11-13 103420" src="https://github.com/user-attachments/assets/c60dc568-805c-45d5-8b66-b80dcac1d34f" />
<img width="1144" height="909" alt="Screenshot 2025-11-13 103545" src="https://github.com/user-attachments/assets/a6a50dd6-4955-4305-9eda-ba0249401b50" />

**Extract the lef file**

```bash
#in tkcon
save sky130_vsdinv.mag

#in the directory vsdcellstddesign
magic -T sky130A.tech sky130_vsdinv.mag

#in tckon
lef write

#open the lef file
less

```
<img width="970" height="909" alt="Screenshot 2025-11-13 110206" src="https://github.com/user-attachments/assets/3ef781cd-eecc-4f9c-b196-bbc0625b064e" />
<img width="1147" height="910" alt="Screenshot 2025-11-13 103850" src="https://github.com/user-attachments/assets/2a073185-c008-462e-b91f-8c40588877a2" />
<img width="1146" height="910" alt="Screenshot 2025-11-13 103955" src="https://github.com/user-attachments/assets/c16a1eb5-64cc-4c48-b7f7-2b545ce617d3" />
<img width="1146" height="910" alt="Screenshot 2025-11-13 103955" src="https://github.com/user-attachments/assets/d2264af5-db9c-4a77-bb85-1a385c2e8154" />

**We need to modify the config.tcl file of picorv32a directory**

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3be46ffb-6ca5-403c-a5d8-f552ac5b2e74" />

**OPENLANE :- Now we will go to the open lane directory and execute the docker command.**

Will Execute the following commands in a line

```bash
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]      
add_lefs -src $lefs
run_synthesis

```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f990eb85-da49-459a-a2c2-908a0b4a1aef" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/82d70c6f-c067-4322-80e1-54c207f2ccdc" />

## Lab steps to configure synthesis settings to fix slack and include vsdinv

**We will try to modify the parameters of our cell by referring the README.md file in the configuration folder in openlane directory**

```bash
#open the readme.md file
cd openlane/configuration/
less README.md

```
<img width="1005" height="911" alt="Screenshot 2025-11-13 155957" src="https://github.com/user-attachments/assets/90f21e87-b1d6-483e-a07b-662842150740" />

**commmands in the terminal in openlane directory**

```bash
#open the OPENLANE
docker

#Initializes the OpenLane design environment for the picorv32a design with a specific tag, replacing any existing run
prep -design picorv32a -tag 01-04_12-54 -overwrite

#Collects all .lef files in the design's src folder into the variable lefs
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]

#Adds the collected LEF files to the OpenLane flow for use during synthesis and PnR
add_lefs -src $lefs

#Prints the current synthesis strategy being used
echo $::env(SYNTH_STRATEGY)

#Changes the synthesis strategy to prioritize “DELAY 3” for timing optimization
set ::env(SYNTH_STRATEGY) "DELAY 3"

#Displays whether buffering is enabled during synthesis
echo $::env(SYNTH_BUFFERING)

#Shows the current setting for cell sizing during synthesis
echo $::env(SYNTH_SIZING)

#Enables gate sizing to improve timing results
set ::env(SYNTH_SIZING) 1

#Prints the driving cell being assumed at the input during synthesis
echo $::env(SYNTH_DRIVING_CELL)

#Starts the logic synthesis flow for the design using the updated configuration
run_synthesis


```
<img width="1140" height="910" alt="Screenshot 2025-11-13 161315" src="https://github.com/user-attachments/assets/b76b2963-e33e-48b9-80c2-c8e7d44b489a" />
<img width="1141" height="922" alt="Screenshot 2025-11-13 161328" src="https://github.com/user-attachments/assets/dd5d7c85-4712-4e1a-a028-01154066c8b9" />
<img width="1142" height="909" alt="Screenshot 2025-11-13 161356" src="https://github.com/user-attachments/assets/5536edba-24f0-4c06-9264-e1d55d2a1898" />
<img width="1140" height="911" alt="Screenshot 2025-11-13 161415" src="https://github.com/user-attachments/assets/6d1aa325-698d-458e-9bb9-d387a8dcdc83" />
<img width="1138" height="908" alt="Screenshot 2025-11-13 161432" src="https://github.com/user-attachments/assets/16746482-18f8-47ac-a5bd-8cf3a13687c7" />
<img width="1144" height="912" alt="Screenshot 2025-11-13 161457" src="https://github.com/user-attachments/assets/3e569c62-41a8-442e-8e27-64dcf3c5f7c5" />

