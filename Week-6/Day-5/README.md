# Day 5

## Final steps for RTL2GDS using tritonRoute and openSTA

**Running the Power Distribution Network (PDN) Generation and Reviewing the Layout**

Below are the commands used to execute all stages up to this point before generating the PDN.

```bash
# Move into the main OpenLANE flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# We previously created an alias named 'docker' for the full Docker command,
# so launching the OpenLANE Docker environment now only requires this:
docker

# Start OpenLANE in interactive mode from inside the container
./flow.tcl -interactive

# Load the OpenLANE package required for the flow
package require openlane 0.9

# Initialize the design setup by creating the necessary folders and configs
prep -design picorv32a

# Add all LEF files, including the merged one, into the OpenLANE environment
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Select a timing-centric synthesis strategy
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Enable sizing during synthesis
set ::env(SYNTH_SIZING) 1

# Run the synthesis stage after preparation
run_synthesis

# These commands are internally executed when 'run_floorplan' is called
init_floorplan
place_io
tap_decap_or

# Proceed with placement
run_placement

# If a CTS-related library issue appears, clear the variable
unset ::env(LIB_CTS)

# Generate the clock tree for the design
run_cts

# After CTS is completed, create the power distribution network
gen_pdn
```

<img width="988" height="911" alt="Screenshot 2025-11-13 184431" src="https://github.com/user-attachments/assets/2b802dd7-a47a-47fb-b827-105f24cd251b" />
<img width="994" height="902" alt="Screenshot 2025-11-13 184444" src="https://github.com/user-attachments/assets/0e237137-5da7-4295-9f43-ef4d67e0b626" />
<img width="991" height="910" alt="Screenshot 2025-11-13 184502" src="https://github.com/user-attachments/assets/7ffc191a-7336-46f0-b540-6cdf132ea349" />
<img width="992" height="907" alt="Screenshot 2025-11-13 184535" src="https://github.com/user-attachments/assets/35dc73cc-63ad-4118-9594-dd09c438480c" />
<img width="992" height="909" alt="Screenshot 2025-11-13 184624" src="https://github.com/user-attachments/assets/f031aa33-ee5f-4216-b1fe-5941cd5bd19b" />
<img width="994" height="913" alt="Screenshot 2025-11-13 184703" src="https://github.com/user-attachments/assets/63e68408-e7f6-42ac-b267-c9c050c12e5e" />
<img width="990" height="910" alt="Screenshot 2025-11-13 184826" src="https://github.com/user-attachments/assets/d80c4aa7-0e59-4b2b-844a-22555abe5995" />
<img width="983" height="909" alt="Screenshot 2025-11-13 184845" src="https://github.com/user-attachments/assets/68508f06-5730-451a-83e7-c9844caf845e" />
<img width="995" height="915" alt="Screenshot 2025-11-13 185015" src="https://github.com/user-attachments/assets/1ef59679-3bab-4b97-a15a-02fb4686cf0d" />


**Commands for Opening the PDN DEF in Magic (in a separate terminal)**


```bash
# Navigate to the directory where the PDN DEF file was generated
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_13_13/tmp/floorplan/

# Launch Magic and load the PDN DEF along with the technology and LEF files
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech \
      lef read ../../tmp/merged.lef \
      def read 14-pdn.def &

```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b8152a04-d2d4-44d2-9c28-8aaa9a4a5e75" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ae63c5d6-da07-40f8-9167-ab5251a289c3" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4968f330-a9eb-4b79-87ae-c50a533252bc" />

**Carrying Out Detailed Routing with TritonRoute and Inspecting the Routed Layout**

```bash
# Display the DEF file currently set in the environment
echo $::env(CURRENT_DEF)

# Show the routing strategy that is presently configured
echo $::env(ROUTING_STRATEGY)

# Execute the det
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3bb307aa-b70e-43
16-acba-3325f7206a20" />

**Commands for Opening the Routed DEF File in Magic (in a separate terminal)**

```bash
# Move to the directory where the routed DEF file is stored
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_13_13results/routing/

# Launch Magic and load the routed DEF along with the technology and LEF files
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech \
      lef read ../../tmp/merged.lef \
      def read picorv32a.def &
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9554d507-d09c-4d8e-8034-ea2dc3e36689" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fd832311-2057-4f54-b81a-a5c21e9f3b17" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6563a6e1-8d0e-41ec-8992-06f45da91c72" />

**Screenshot of fast route guide present in openlane/designs/picorv32a/runs/26-03_08-45/tmp/routing directory**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c7591ae7-796b-4d15-8048-c4d2060e7959" />

**Post-Route Parasitic Extraction Using the SPEF Generator**

Below are the commands used to extract parasitics after routing through an external SPEF extraction utility.

```bash
# Move into the SPEF extractor tool directory
cd Desktop/work/tools/SPEF_EXTRACTOR

# Run the SPEF extraction script using the merged LEF and final routed DEF
python3 main.py \
/home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_13_13/tmp/merged.lef \
/home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_13_13results/routing/picorv32a.def
```

**Post-Route Timing Analysis in OpenROAD Using Extracted SPEF Data**

The following commands perform timing analysis in OpenROAD (with integrated OpenSTA) after incorporating the post-route parasitics.
```bash
# Launch OpenROAD
openroad

# Load the merged LEF file
read_lef /openLANE_flow/designs/picorv32a/runs/13-11_13_13/tmp/merged.lef

# Load the DEF generated from detailed routing
read_def /openLANE_flow/designs/picorv32a/runs/13-11_13_13/results/routing/picorv32a.def

# Save the current OpenROAD project as a database
write_db pico_route.db

# Reload the database for further processing
read_db pico_route.db

# Bring in the post-route synthesized netlist
read_verilog /openLANE_flow/designs/picorv32a/runs/26-03_08-45/results/synthesis/picorv32a.synthesis_preroute.v

# Import the technology library needed for timing
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Bind the design with its libraries
link_design picorv32a

# Load the custom timing constraints
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Specify that all clocks use propagated delays
set_propagated_clock [all_clocks]

# Read the SPEF parasitics extracted from routing
read_spef /openLANE_flow/designs/picorv32a/runs/13-11_13_13/results/routing/picorv32a.spef

# Generate the detailed timing report
report_checks -path_delay min_max \
              -fields {slew trans net cap input_pins} \
              -format full_clock_expanded \
              -digits 4

# Close OpenROAD and return to the OpenLANE shell
exit
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1221e14f-c514-4f2f-a5d6-c318ee4d6b98" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e4b36b47-88b2-4c69-a4bb-86a32c40066f" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6ae9cb6b-2666-48c5-b5b1-c43fd94a7ed2" />

## Conclusion

Through this workshop, I gained complete hands-on experience with the full RTL-to-GDSII physical design flow using OpenLANE and OpenROAD on the Sky130 PDK. I worked through every stage of the ASIC implementation process—starting from synthesis and floorplanning, all the way to placement, CTS, PDN generation, routing, parasitic extraction, and final timing analysis.
