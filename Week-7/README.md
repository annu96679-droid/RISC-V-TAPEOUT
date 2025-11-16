# Week 7 - Tasks

Automated RTL2GDS Flow for VSDBabySoC:

Initial Steps:

* **We need to create a directory vsdbabysoc inside OpenROAD-flow-scripts/flow/designs/sky130hd**

<img width="1284" height="797" alt="Screenshot 2025-11-14 120633" src="https://github.com/user-attachments/assets/0675d118-a586-41b6-ad18-ee750d95212a" />

* **Now create a directory vsdbabysoc inside OpenROAD-flow-scripts/flow/designs/src and include all the verilog files here.**

<img width="1290" height="804" alt="Screenshot 2025-11-14 121943" src="https://github.com/user-attachments/assets/ee33145b-834c-4393-b4fb-08b35e0280ea" />
<img width="1287" height="806" alt="Screenshot 2025-11-14 121954" src="https://github.com/user-attachments/assets/f46796c6-06e6-4a81-b244-8a9c605e4cbd" />

* **Now copy the folders gds, include, lef and lib from the VSDBabySoC folder in your system into this directory.**

<img width="1288" height="802" alt="Screenshot 2025-11-14 122442" src="https://github.com/user-attachments/assets/44808df6-fb12-40ce-b646-deaf6ea423e6" />
<img width="1284" height="801" alt="Screenshot 2025-11-14 122741" src="https://github.com/user-attachments/assets/6c5d36f0-7094-4300-b9e5-d5acb6089813" />


* **The gds folder would contain the files avsddac.gds and avsdpll.gds**

* **The include folder would contain the files sandpiper.vh, sandpiper_gen.vh, sp_default.vh and sp_verilog.vh**

<img width="1288" height="805" alt="Screenshot 2025-11-14 123008" src="https://github.com/user-attachments/assets/25169069-366f-45d4-b98f-4a09163a5609" />



* **The gds folder would contain the files avsddac.lef and avsdpll.lef**

* **The lib folder would contain the files avsddac.lib and avsdpll.lib**

* **Now copy the constraints file(vsdbabysoc_synthesis.sdc) from the VSDBabySoC folder in your system into this directory.**

* **Now copy the files(macro.cfg and pin_order.cfg) from the VSDBabySoC folder in your system into this directory.**

<img width="1285" height="804" alt="Screenshot 2025-11-14 123144" src="https://github.com/user-attachments/assets/4cd30d4a-c977-4817-a821-7fb6516de586" />

* **Now, create a config.mk file whose contents are shown below:**

<img width="1286" height="807" alt="Screenshot 2025-11-14 161801" src="https://github.com/user-attachments/assets/824cbbc5-8d99-4bcc-803e-8a5338b6ac44" />

**This is the updated config.mk**

<img width="1282" height="825" alt="Screenshot 2025-11-14 130933" src="https://github.com/user-attachments/assets/637aadbc-333d-4dec-b4f3-9fe9c5c820a3" />
<img width="1287" height="805" alt="Screenshot 2025-11-14 161744" src="https://github.com/user-attachments/assets/7673c91d-9116-4817-96ae-d40023dca0c8" />

* **Now go to terminal and run the following commands:**

```bash
cd OpenROAD-flow-scripts
source env.sh
cd flow
```

## 1. Commands for synthesis:

```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk synth

```
<img width="1288" height="808" alt="Screenshot 2025-11-14 161902" src="https://github.com/user-attachments/assets/4f8e303e-f7e3-4395-8c83-63b2ff5b95af" />
<img width="1286" height="807" alt="Screenshot 2025-11-14 161801" src="https://github.com/user-attachments/assets/09f90b64-3a5c-43de-9c62-60224c334890" />

**Synthesis netlist:**

<img width="1292" height="807" alt="Screenshot 2025-11-14 162447" src="https://github.com/user-attachments/assets/d0f081c0-cc67-440f-bea6-71bf5ce3755c" />

**Synthesis log:**

<img width="1285" height="808" alt="Screenshot 2025-11-14 162614" src="https://github.com/user-attachments/assets/822547b5-0610-43ce-89bc-83272ae76d79" />
<img width="1291" height="803" alt="Screenshot 2025-11-14 162638" src="https://github.com/user-attachments/assets/4648ec96-ce3a-41b4-b5f0-ebae97fb8965" />

**Synthesis check:**

<img width="1285" height="802" alt="Screenshot 2025-11-14 162959" src="https://github.com/user-attachments/assets/15e2448c-03ac-42ef-ae53-a8da9a4fe09a" />

**Synthesis Stats:**

<img width="1285" height="806" alt="Screenshot 2025-11-14 163036" src="https://github.com/user-attachments/assets/58ccbb66-ae72-45b3-bc54-ec78c4189f30" />
<img width="1284" height="808" alt="Screenshot 2025-11-14 163047" src="https://github.com/user-attachments/assets/fe157656-6bd1-4a6a-8151-36724b35114a" />
<img width="1291" height="809" alt="Screenshot 2025-11-14 163058" src="https://github.com/user-attachments/assets/a0bd3830-3a1e-40eb-83e0-3af15726b488" />

## 2. Commands for floorplan:

before running floorplan we have to make some changes:  we have to delete these 3 blocks entirely from the file:

* Block 1(GND#2)

* Block 2(VDD#2)

* Block #(VDD#3)

<img width="1285" height="808" alt="Screenshot 2025-11-14 183505" src="https://github.com/user-attachments/assets/98fb8345-b563-430d-8a25-93561d3da0da" />
<img width="1289" height="807" alt="Screenshot 2025-11-14 183453" src="https://github.com/user-attachments/assets/6ef6761e-2f81-46fd-8adc-869d26610745" />

```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk floorplan
```
<img width="1285" height="806" alt="Screenshot 2025-11-14 183546" src="https://github.com/user-attachments/assets/74b7df58-67dc-488b-961d-b3b8054dbf7a" />

**Both tapcell insertion and power delivery network (PDN) generation executed successfully for the vsdbabysoc design on the Sky130HD PDK. Tapcells were correctly inserted across the placement rows, and the PDN grid structure was created with appropriate power rails and macro grids. Although some supply pin warnings were reported, these are typical for certain analog macros and do not affect the PDN build. The floorplan is now ready to proceed to the placement stage.**

**NOW, Open the GUI (OpenROAD GUI) during the floorplan stage**

```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_floorplan
```

<img width="1288" height="805" alt="Screenshot 2025-11-14 184221" src="https://github.com/user-attachments/assets/d2eb6f30-ecd5-42b6-88ec-9e6cb3504498" />
<img width="1301" height="807" alt="Screenshot 2025-11-14 184010" src="https://github.com/user-attachments/assets/7eb0d75c-f3a8-4606-bd82-94a49830e55c" />

After importing the synthesized BabySoC netlist into OpenROAD, the floorplanning stage was completed successfully. The OpenROAD GUI view clearly shows a well-defined core boundary with the placement rows, macros, and blockages properly arranged inside the defined die area.

**✔ 1. Floorplan Dimensions and Core Area Defined Correctly**

The core boundary is visible in the GUI, indicating that:

* The die size,

* core area, and

* aspect ratio

were successfully generated according to the values specified in the floorplan configuration.
The grid of evenly spaced horizontal and vertical rows confirms that the placement rows were created properly.

**✔ 2. Core Utilization and Density Constraints Applied**

The Inspector window shows a region marked as a blockage, with:

* Max density = 0.000000%


This indicates that the blockage area has been defined correctly to prevent cell placement in specific regions (for example, under macros).

* The low global utilization ensures:

   * lower routing congestion,

   * easier CTS and routing,

   * better timing closure.

**✔ 3. Macro Blockages and Placement Regions Visible**

* The large rectangular dark region visible in the layout is a soft blockage, which ensures that:

   * standard cells do not overlap the macro region

   * macros have dedicated space inside the floorplan

   * routing channels around macros are preserved

This confirms that macro-level planning (SRAM/PLL/DAC/ADC, etc.) is done correctly.

**✔ 4. All Standard Cells Fit Within the Floorplan Boundary**

* The visible red/blue vertical placement sites represent standard-cell rows.

* Their alignment inside the core boundary confirms that:

* all rows are properly created,

* standard cells will be placed within legal locations,

* cell orientation and site configurations are valid.

* OpenROAD reports no violations related to cell rows or row cuts.

## 3. Runs the entire placement stage of OpenLane

```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk place
```

<img width="1293" height="807" alt="Screenshot 2025-11-14 184456" src="https://github.com/user-attachments/assets/dd9077d1-c064-44b9-adc6-4e18aaeb1304" />

<img width="1288" height="804" alt="Screenshot 2025-11-14 184518" src="https://github.com/user-attachments/assets/37d9e3b6-089b-48aa-b4cd-bb3fd9973e20" />
<img width="1291" height="807" alt="Screenshot 2025-11-14 184545" src="https://github.com/user-attachments/assets/39db9844-5d98-4b3e-8c37-9423edd9d096" />
<img width="1287" height="802" alt="Screenshot 2025-11-14 184558" src="https://github.com/user-attachments/assets/6be2f6be-84d5-4ea5-a346-b02d0410ded7" />
<img width="1291" height="806" alt="Screenshot 2025-11-14 184629" src="https://github.com/user-attachments/assets/beca739e-130d-460f-a5dc-3f08b9555b9b" />

**From the OpenROAD placement logs, we can conclude the following:**

**1. Detailed Placement Completed Successfully**

* The detailed placement engine (DPL) finished without reporting:

  * ❌ no cell overlap

  * ❌ no site alignment issues

  * ❌ no edge spacing violations

  * ❌ no padding violations

This indicates a legally placed and DRC-clean cell arrangement.

**2. HPWL Improved (Better Wirelength Optimization)**

* HPWL = Half-Perimeter Wire Length (a metric to estimate routing complexity)

* Original HPWL: 206,070.3 units

* Final HPWL: 193,204.1 units

* Improvement (Delta): –6.2%

* This means:
   * ✔ The placer reduced the expected wire length
   * ✔ Routing will be easier and timing will improve

**3. Cell Flipping & Mirroring Performed**

* 1670 cell flips performed

* 267 instances mirrored

* This helps:
   * ✔ reduce wirelength
   * ✔ improve routability
   * ✔ keep cells aligned with power/ground rails

**4. Placement Utilization is Good**

* Design area: 724,772 μm²
* Utilization: 29%


* This means:
   * ✔ The design is not congested
   * ✔ There is enough free space for routing
   * ✔ No density problems

* 29% is a healthy utilization for successful routing in Sky130.

**5. No Placement Violations**

* INFO FLW-0012 Placement violations .

* This message means 0 violations were found—placement is clean and ready for CTS.

**6. Output Files Generated Successfully**

The flow generated the final placement database:

* 3_5_place_dp.odb

* Updated floorplan.sdc

* 3_place.sdc

* These files will be used for the next stages (CTS, routing).

**Opens the OpenROAD GUI at the placement stage**

```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_place
```

<img width="1262" height="802" alt="Screenshot 2025-11-15 093808" src="https://github.com/user-attachments/assets/ccc47dbe-2be3-4319-b724-a6f11052045c" />
<img width="1263" height="805" alt="Screenshot 2025-11-15 093734" src="https://github.com/user-attachments/assets/f6a64c80-8293-4b42-b2b9-65d0f8bff132" />
<img width="1262" height="822" alt="Screenshot 2025-11-15 093752" src="https://github.com/user-attachments/assets/b4c25b49-3cae-4f02-a4fe-bd754cadbef2" />
<img width="1258" height="824" alt="Screenshot 2025-11-15 093937" src="https://github.com/user-attachments/assets/1b313513-9661-45c0-9163-20b3458a73c4" />
<img width="1266" height="809" alt="Screenshot 2025-11-15 094238" src="https://github.com/user-attachments/assets/5b23fa9d-6e09-4332-9842-5d7fecf06840" />
<img width="1286" height="807" alt="Screenshot 2025-11-15 095954" src="https://github.com/user-attachments/assets/4544c926-39f2-47fe-a5ef-a6780cc30b02" />

**The placement stage for the BabySoC design using the Sky130HD PDK was completed successfully in OpenROAD. Both global placement and detailed placement were performed, followed by verification checks and visual inspection using the gui_place interface.**

**1. Global and Detailed Placement Completed Successfully**

* During placement:

   * Standard cells were distributed across the available placement rows.

   * Global placement optimized the approximate positions of cells to reduce overall wire length (HPWL).

   * Detailed placement refined the placement to ensure:

   * legal cell positions,

   * alignment with rows,

   * no cell overlap,

   * proper cell orientation.

*Inspector data confirms several cells have:

   * Placement status: PLACED
   
   * Orientation: R180 / MX / MY (depending on rails)


This indicates that orientation and legality rules are being respected.

** 2. Placement Density Visualization**

We have heatmap density view, where colors represent placement congestion:

* Green/Yellow zones → higher density (clustered logic)

* Blue/Purple zones → lower density

* Distribution is balanced, meaning global placement found good spreading.

* There are no red (highly congested) zones, indicating the design is routable with minimal congestion issues.

* The presence of defined blockages ensures that the design respects macro boundaries.

**3. Verification of Legality**

* The placement results show:

   * No overlaps between adjacent cells

   * No row alignment issues

   * Proper well-tap cell alignment (verified earlier)

   * All cells placed inside legal placement rows

   * Macro blockages and routing channels remain intact

   * This confirms that detailed placement cleaned up any violations.

**4. Instance-Level Inspection**

* Standard cells (inverters, buffers, flip-flops) properly aligned on rows.

* Inspector reports:

   * Master: sky130_fd_sc_hd__inv_1
   
   * Description: Inverter
   
   * Placement status: PLACED


* Flip-flops (e.g., dfxtp) also show correct placement with matching orientation.

* This verifies the correct use of Sky130HD library cells.

5. Heavy Zoom & Track Alignment Check

* At high zoom levels:

   * Routing tracks (horizontal + vertical) are aligned with cell pins.

   * Metal layers (met1–met5) and via layers are correctly aligned.

   * No off-grid placement, confirming that the floorplan grid settings are correct.

   * This ensures clean routing and reduced risk of DRC violations later.

**6. Timing Evaluation After Placement**

* Timing report shows:

   * Positive slack on datapaths

   * No setup or hold violations after placement

   * TNS (Total Negative Slack):

   * ns max = 0.00


* This means:

  * ✔ Placement has not introduced timing violations

  * ✔ The design is ready for clock-tree synthesis (CTS)
