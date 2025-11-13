
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

After synthesis, we have observed that the slack is nagative.

wns(worst negative slack)= -23.89

tns(total negative slack)= -711.59.

**run the floorplan using command run_floorplan**
```bash
run_floorplan
```

<img width="1143" height="911" alt="Screenshot 2025-11-13 161612" src="https://github.com/user-attachments/assets/0d81b719-9397-4631-9c8f-6601c2bd3e22" />

Since, we are getting the error so first again we have to do the synthesis using the commands mentioned earlier and then we will use following commands to do the floorplan.

**Commands to remove the error**
#Creates the initial chip floorplan by defining core area, die size, utilization, and placing power/ground rails.
init_floorplan

#Automatically places all I/O pads/ports around the boundary of the chip according to the constraints.
place_io

#Inserts well-tap cells, decoupling capacitors, and end-cap cells to ensure proper power integrity and well-tie spacing before placement
tap_decap_or

<img width="1140" height="453" alt="Screenshot 2025-11-13 161636" src="https://github.com/user-attachments/assets/1c3a234f-6968-47c7-9ac7-a72f56e28883" />
<img width="1141" height="491" alt="Screenshot 2025-11-13 161705" src="https://github.com/user-attachments/assets/4fcb1212-f81b-4f8b-8cc6-559d5e5f6883" />
<img width="1145" height="572" alt="Screenshot 2025-11-13 161725" src="https://github.com/user-attachments/assets/829d799e-ce86-45ff-8233-9a37a4dff45c" />

**run the placement**  : Here we will get a successful placement
<img width="1141" height="912" alt="Screenshot 2025-11-13 161811" src="https://github.com/user-attachments/assets/7e9a69c4-8097-4ff0-b784-c43b3c69a2c0" />
<img width="1141" height="907" alt="Screenshot 2025-11-13 161836" src="https://github.com/user-attachments/assets/89b0ecaa-69d5-4f84-b688-477f8e39328c" />


**Commands to load placement def in magic in another terminal**

```bash
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/24-03_10-03/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

<img width="990" height="911" alt="image" src="https://github.com/user-attachments/assets/f00f5221-5acd-4ac6-8d0f-2c8c7d029530" />

** view of custom inverter inserted in placement def**

<img width="993" height="914" alt="Screenshot 2025-11-13 172803" src="https://github.com/user-attachments/assets/38f238bc-5060-4da3-9f1b-4ad20abcfe59" />

**Command for tkcon window to view internal layers of cell**


```bash
#Command to view internal connectivity layers
expand

```
<img width="990" height="913" alt="Screenshot 2025-11-13 172828" src="https://github.com/user-attachments/assets/2a8bdc2b-c6f5-4d86-a1b2-b943e5a5e41a" />
<img width="989" height="909" alt="Screenshot 2025-11-13 172904" src="https://github.com/user-attachments/assets/29c35528-89fc-49c7-b4e5-7a3872a6cfc1" />

## Lab steps to configure OpenSTA for post-synth timing analysis

Since we are having 0 wns after improved timing run we are going to do timing analysis on initial run of synthesis which has lots of violations and no parameters were added to improve timing

Commands to invoke the OpenLANE flow include new lef and perform synthesis

```bash
#Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

#alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'

#Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker

#Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

#Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

#Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

#Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

#Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

#Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```
<img width="991" height="918" alt="Screenshot 2025-11-13 173745" src="https://github.com/user-attachments/assets/922a2773-7f56-48ef-a7ab-c1a796b23765" />
<img width="993" height="910" alt="Screenshot 2025-11-13 173823" src="https://github.com/user-attachments/assets/259b1d55-fce5-4ce8-81bb-341a0c2df7f9" />
<img width="992" height="911" alt="Screenshot 2025-11-13 174341" src="https://github.com/user-attachments/assets/eb055970-8687-4cf7-90f9-f070deae0407" />

**A newfile is created in the openlane directory: pre_sta.conf**

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bd1c2827-1498-46fd-b3d7-5ba647e0ce1a" />

**Newly created my_base.sdc for STA analysis**

```bash
cd openlane/designs/picorv32a/src
openlane/scripts/base.sdc**

```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/94d63b96-a890-434a-bea3-0df665b2dbc8" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1447a10f-80dc-42ae-915e-3962e1c42448" />

```bash
**Commands to run STA in another terminal**

#Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

#Command to invoke OpenSTA tool with script
sta pre_sta.conf
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e39484ce-34e6-4bae-b248-27f166aa5b35" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b0d7c468-de93-4122-ab6e-f1a222596f7a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a839f84b-3579-456e-9ff0-506a2a17a403" />

## Lab steps to optimize synthesis to reduce setup violations

**As we can see in the enviornment, the fanout is causing more delay so now we have to reduce the fanout but adding some parameters**

```bash
# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a -tag 25-03_18-52 -overwrite

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Command to set new value for SYNTH_MAX_FANOUT
set ::env(SYNTH_MAX_FANOUT) 4

# Command to display current value of variable SYNTH_DRIVING_CELL to check whether it's the proper cell or not
echo $::env(SYNTH_DRIVING_CELL)

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/58b42eb4-a749-4f58-b33e-23fba6c057f4" />

**Commands to run STA in another terminal**

```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Command to invoke OpenSTA tool with script
sta pre_sta.conf
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/234ce0a8-08cb-4522-8731-e302bcbc49b7" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4b06fb1f-c0c9-469b-8f6b-6051117b8efe" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/da343273-5ecd-48b5-80bb-cdc7cacc752b" />

## Lab steps to do basic timing ECO

**Here we are focusing on OR gate which have driving strenth of 2 and driving the 4 fanouts**

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/caca7e21-b307-429e-94d3-eddc28457e70" />

**Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4**

```bash
# Reports all the connections to a net
report_net -connections _11672_

# Checking command syntax
help replace_cell

# Replacing cell
replace_cell _14510_ sky130_fd_sc_hd__or3_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/48c414c6-4511-44e9-b3ae-3f416a3917a7" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e99adddd-e13a-4ca1-895e-6c0ba9af2f05" />

Hence slack reduced from -23.89 to -23.51

<img width="906" height="133" alt="image" src="https://github.com/user-attachments/assets/53e9b0bc-f853-4b74-bf09-26f7a40aa0c6" />

**Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4**

```bash
# Reports all the connections to a net
report_net -connections _11675_

# Replacing cell
replace_cell _14514_ sky130_fd_sc_hd__or3_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6cfeef90-7fa6-4ed4-807c-f685d871b044" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0f3d820e-03a7-4d0e-8496-9fb19f7deb55" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b688142d-bac3-406f-bce6-f69cd6785f60" />

**OR gate of drive strength 2 driving OA gate has more delay**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1e6ec409-93cf-4769-8fd5-36b93174a4f7" />

**Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4**

```bash
# Reports all the connections to a net
report_net -connections _11643_

# Replacing cell
replace_cell _14481_ sky130_fd_sc_hd__or4_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/74a928a2-039d-4a74-ac91-9638f60c6a55" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/92023f6d-5a0b-4302-b19f-e52b48ba16dd" />

**Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4**

```bash
# Reports all the connections to a net
report_net -connections _11668_

# Replacing cell
replace_cell _14506_ sky130_fd_sc_hd__or4_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4e4342ff-ec09-4cf8-aea7-09c6ade1dd9a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5b9c6166-329a-4e00-8b2e-a1b809dec79e" />

**Commands to verify instance _14506_ is replaced with sky130_fd_sc_hd__or4_4**

```bash
# Generating custom timing report
report_checks -from _29043_ -to _30440_ -through _14506_
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c1e61505-adf9-408d-8dc2-64a263f84e31" />

We started ECO fixes at wns -23.9000 and now we stand at wns -22.6173 we reduced around 1.2827 ns of violation

**Replace the old netlist with the new netlist generated after timing ECO fix and implement the floorplan, placement and cts.**

To bring the modified netlist back into the PnR flow, we will regenerate the Verilog file using write_verilog and replace the existing synthesis netlist. However, before overwriting anything, we’ll first create a backup of the current netlist.

**Commands to back up the netlist:**

```bash
# Navigate from the home directory to the synthesis output folder
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/30-10_06_16/results/synthesis/

# Display the files in this directory
ls

# Create a copy of the original netlist with a new name
cp picorv32a.synthesis.v picorv32a.synthesis_old.v

# Verify the copied file by listing the directory contents again
ls
```

**Commands for generating the Verilog file**

```bash
# View the syntax and usage information for the write_verilog command
help write_verilog

# Replace the existing synthesized netlist with the newly generated one
write_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/25-03_18-52/results/synthesis/picorv32a.synthesis.v

# Leave the OpenSTA environment after completing the timing checks
exit
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7bc88699-8563-4e22-b7a9-0c29191eaa19" />

**Confirmed that the updated netlist was successfully written by verifying that the instance _14506_ has now been substituted with sky130_fd_sc_hd__or4_4**

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2c186b9e-201b-4529-bb80-c5894f3cec35" />

Since we have validated that the netlist was successfully updated and will be used in the PnR flow, but we want to continue from the previously clean, zero-violation version, we will proceed with the original stable design for the remaining steps.

**Commands to reload the design and execute the required stages**

```bash
# Re-prepare the design so all environment variables get refreshed
prep -design picorv32a -tag 24-03_10-03 -overwrite

# Add the newly introduced merged LEF file into the OpenLane setup
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Update the synthesis strategy to a new setting
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Enable gate sizing for synthesis
set ::env(SYNTH_SIZING) 1

# With the preparation complete, execute synthesis
run_synthesis

# The following three commands are included automatically when run_floorplan is executed
init_floorplan
place_io
tap_decap_or

# Proceed to the placement stage
run_placement

# If a CTS-related library mismatch error appears, reset the variable
unset ::env(LIB_CTS)

# Once placement is complete, continue with clock tree synthesis
run_cts
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c0811ded-c131-4323-9d58-3cf08e4310d2" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/abc5b1b5-37ff-4fc7-8889-9caa97902dbb" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e8d1f7f3-6dc0-47e7-aac6-ace7e2fe753b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bd1b2340-89e7-4cae-9b16-ef61892bf389" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c372fb22-6de9-4d8f-b088-3544ae93f50f" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8280ebf5-5a84-4755-9573-dc4eb9dc725f" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/14c023b6-6cb7-43c7-ba93-98f7f0db47e0" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/113abf2f-ecfe-492f-a217-ab5eea55a5a8" />

**Post-CTS Timing Analysis Using OpenROAD**

Below are the commands used inside the OpenLANE flow to perform timing analysis through OpenROAD’s integrated OpenSTA engine:

```bash
# Launch the OpenROAD environment
openroad

# Load the merged LEF file
read_lef /openLANE_flow/designs/picorv32a/runs/24-03_10-03/tmp/merged.lef

# Import the DEF generated after CTS
read_def /openLANE_flow/designs/picorv32a/runs/24-03_10-03/results/cts/picorv32a.cts.def

# Save the current OpenROAD session into a database file
write_db pico_cts.db

# Reload the stored database for further processing
read_db pico_cts.db

# Load the post-CTS synthesized netlist
read_verilog /openLANE_flow/designs/picorv32a/runs/24-03_10-03/results/synthesis/picorv32a.synthesis_cts.v

# Provide the timing library required for analysis
read_liberty $::env(LIB_SYNFF_COMPLETE)

# Bind the design with the associated libraries
link_design picorv32a

# Bring in the custom SDC constraint file
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Mark all clock networks as propagated
set_propagated_clock [all_clocks]

# View the usage details of the 'report_checks' command
help report_checks

# Produce a detailed timing report with extended fields
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Exit OpenROAD and return to the OpenLANE shell
exit
```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/75eba4e2-41e5-4b0f-845a-8a8978ccaa4e" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/942c687e-891e-4ca7-8d65-1fdbd5ac3a9b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ea0d281b-2602-45e4-a23b-206e8604c216" />

**Post-CTS Timing Exploration After Modifying CTS_CLK_BUFFER_LIST**

Below are the commands used in the OpenLANE flow to perform timing analysis in OpenROAD after removing the sky130_fd_sc_hd__clkbuf_1 entry from the clock buffer list.

```bash
# Display the current contents of CTS clock buffer list
echo $::env(CTS_CLK_BUFFER_LIST)

# Delete 'sky130_fd_sc_hd__clkbuf_1' from the first position in the buffer list
set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]

# Verify the updated list
echo $::env(CTS_CLK_BUFFER_LIST)

# Show the DEF file currently in use
echo $::env(CURRENT_DEF)

# Point CURRENT_DEF variable to the placement DEF
set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/24-03_10-03/results/placement/picorv32a.placement.def

# Execute CTS again with the modified buffer list
run_cts

# Confirm the buffer list after rerunning CTS
echo $::env(CTS_CLK_BUFFER_LIST)

# Launch OpenROAD for detailed timing analysis
openroad

# Import merged LEF file
read_lef /openLANE_flow/designs/picorv32a/runs/24-03_10-03/tmp/merged.lef

# Load the DEF produced after CTS
read_def /openLANE_flow/designs/picorv32a/runs/24-03_10-03/results/cts/picorv32a.cts.def

# Save the current OpenROAD session
write_db pico_cts1.db

# Reload the previously saved database
read_db pico_cts.db

# Bring in the CTS-updated netlist
read_verilog /openLANE_flow/designs/picorv32a/runs/24-03_10-03/results/synthesis/picorv32a.synthesis_cts.v

# Import the timing library
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Bind the netlist and library
link_design picorv32a

# Load the constraint file created earlier
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Mark all clock domains as propagated
set_propagated_clock [all_clocks]

# Generate a detailed timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Display hold-related clock skew
report_clock_skew -hold

# Display setup-related clock skew
report_clock_skew -setup

# Exit back to the OpenLANE flow
exit

# Print the current CTS buffer list again
echo $::env(CTS_CLK_BUFFER_LIST)

# Reinsert 'sky130_fd_sc_hd__clkbuf_1' at index 0 of the buffer list
set ::env(CTS_CLK_BUFFER_LIST) [linsert $::env(CTS_CLK_BUFFER_LIST) 0 sky130_fd_sc_hd__clkbuf_1]

# Confirm the updated list
echo $::env(CTS_CLK_BUFFER_LIST)
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0e8512b4-4a2e-4f0a-8f45-344ceca476b2" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8370dfe7-2e42-4526-8bcb-1a7b13330961" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a8271f73-a882-4d57-89f4-9b7d60e1365b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/043eac71-0290-48f9-9b94-68add3151d84" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ac1a6add-2e8a-4523-b291-f93d0026afa6" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4b07f05e-6ad9-4ae2-974b-9487134a5a16" />
