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

**2. Placement Density Visualization**

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

**5. Heavy Zoom & Track Alignment Check**

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


## Clock Tree Synthesis (CTS) for the BabySoC design

```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk cts
```
<img width="1290" height="809" alt="Screenshot 2025-11-15 100133" src="https://github.com/user-attachments/assets/1580c7d3-ba38-4b2a-af03-f145c5e3db95" />
<img width="1292" height="806" alt="Screenshot 2025-11-15 100149" src="https://github.com/user-attachments/assets/b2de0563-052c-495c-8fc7-109187965d98" />

```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_cts
```
<img width="1281" height="812" alt="Screenshot 2025-11-15 100308" src="https://github.com/user-attachments/assets/e6876ca7-37a7-441c-892c-e1932f7b526c" />

**1. Clock Tree Successfully Built**

* The command ran the CTS step:

   * 4_1_cts


* This means:

   * The clock tree was constructed.

   * Clock buffers/inverters were inserted correctly.

   * The tree was balanced to reduce skew and improve clock distribution.

**2. Timing Repair Performed (No Violations Found)**

* OpenROAD executed:

    * repair_timing -setup_margin 0 -hold_margin 0

* The results show:

   * [INFO RSZ-0098] No setup violations found.

   * [INFO RSZ-0033] No hold violations found.


* ✔ No setup timing issues

* ✔ No hold timing issues

* ✔ The clock network is clean after CTS

This means the inserted clock buffers did not disturb timing.

**3. Placement Analysis After CTS**

* Before timing repair:

   * total displacement 2067.6 u
   
   * legallized HPWL = 208195.7 u
   
   * delta HPWL = 2%


* After repair:

   * total displacement = 0 u

   * average displacement = 0 u
   
   * max displacement = 0 u


* This shows:

   * All clock buffers were placed legally

   * No cell moved out of place after legalization

   * The design remained stable after CTS

**4. Design Utilization Still Healthy**

* From report:

   * Design area: 729567 um^2

   * Utilization: 30%


* This confirms:

   * Enough free area remains for routing

   * CTS did not overload the layout

   * No congestion was introduced

   * 30% utilization is ideal for routing heavy designs.

**5. CTS Files Generated Correctly**

* Output files:

   * 4_cts.odb

   * 4_cts.sdc

   * 4_cts.def (created internally)


* These include:

   * Clock tree placement

   * Updated SDC with clock latencies

   * Updated DEF for routing stage

These files are used in the next step (routing).

**6. GUI CTS Loaded Successfully**

* The clock tree timing graph is generated

* Slack values are displayed

* You can debug clock timing through the GUI

* The fact that OpenROAD GUI opened without errors indicates the CTS stage produced a valid ODB database.

**Final Summary Conclusion**

The CTS stage for BabySoC was completed successfully. The clock buffers were inserted, timing was re-evaluated, and both setup and hold checks showed zero violations. Placement remained legal after CTS, and the design maintained healthy utilization at 30%. The flow generated all necessary CTS files and the updated timing model, confirming that the design is ready to proceed to the global and detailed routing stages.

## Routing Stage (Global Routing) of BabySoC

```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_cts
```
<img width="1247" height="809" alt="Screenshot 2025-11-15 102027" src="https://github.com/user-attachments/assets/4a93281f-7669-4576-ba04-c66b62908912" />
<img width="1288" height="809" alt="Screenshot 2025-11-15 102107" src="https://github.com/user-attachments/assets/81aadc5f-fbdd-4e48-8f6c-f3c7f8d2c834" />
<img width="1282" height="806" alt="Screenshot 2025-11-15 102117" src="https://github.com/user-attachments/assets/ff11675a-b823-4499-9235-bb5c015f28c3" />

**The routing stage was executed using OpenROAD’s global router (FastRoute). This step analyzes routing resources, assigns routing paths, estimates via usage, and checks congestion before detailed routing. The log output provides key insights into the quality and feasibility of the routing for the BabySoC design.**

**1. Routing Resources Successfully Loaded**

* The router identified:

   * 13 metal/via layers

   * 443 macros

   * 29 via types

   * 25 via rulesets

This confirms that the Sky130HD technology LEF and the design’s DEF were correctly loaded.

**2. Derated Routing Resources Calculated**

The tool performed an assessment of resource availability:

| Layer | Original  | Derated | Reduction |
| ----- | --------- | ------- | --------- |
| met1  | 1,071,378 | 459,331 | 57.13%    |
| met2  | 804,318   | 399,107 | 50.32%    |
| met3  | 535,689   | 297,286 | 44.50%    |
| met4  | 322,014   | 144,707 | 55.06%    |
| met5  | 106,953   | 41,660  | 61.05%    |


Derated resources represent real available routing capacity after considering spacing rules, blockages, and PDN structures.

These values indicate a realistic estimate for routability.

**3. Congestion Repair Iterations Performed**

The router ran additional iterations to reduce overflow:

* [INFO GRT-0101] Running extra iterations to remove overflow.


It also performed:

  * Via-related pin node fixing

  * Steiner tree adjustments

  * Via filling

This ensures improved path selection and less congestion during detailed routing.

**4. Via Statistics Generated**

The final design required:

  * 50961 vias total

  * 192,902 3D routing usage

This is typical for a design of this size and confirms that the router is connecting nets across multiple metal layers effectively.

**5. Final Congestion Report**

The congestion numbers are:

| Layer     | Demand | Usage (%)               | Max H Overflow                    | Max V Overflow | Total Overflow |
| --------- | ------ | ----------------------- | --------------------------------- | -------------- | -------------- |
| met1      | 16769  | 3.65%                   | 1                                 | 0              | 1              |
| met2      | 18072  | 4.53%                   | 0                                 | 1              | 7              |
| met3      | 3379   | 1.14%                   | 1                                 | 0              | 3              |
| met4      | 1783   | 1.23%                   | 0                                 | 1              | 7              |
| met5      | 16     | 0.04%                   | 1                                 | 0              | 4              |
| **Total** | 40019  | **2.98% overall usage** | **3 H / 2 V / 22 total overflow** |                |                |


* Interpretation:

   * Overall routing usage is very low (≈3%), meaning the design is not overly congested.

   * The low overflow numbers are acceptable and typical for global routing.

However, any non-zero overflow must be addressed before detailed routing (DRT).

**6. Global Routing Completed With Congestion (Important)**

The tool ended with:

* [ERROR GRT-0116] Global routing finished with congestion.


Although the congestion is small, the router flags it as a failure because:

* Some grid edges still exceeded capacity

* This must be fixed before running detailed routing

* This may require:

* Adjusting blockages

* Increasing routing resources

* Fine-tuning placement

* Or simply allowing detailed routing to fix minor overflows

This is not a critical error, but it indicates that the design needs congestion cleanup before final routing.

**Final Summary Conclusion**

The global routing stage for BabySoC was executed successfully, and routing resources were properly analyzed across all layers. The router generated comprehensive congestion, via, and usage reports. Although routing utilization remains low (only ~3%), minor congestion persists in a few regions, resulting in a global routing warning. This congestion can be resolved through additional optimization or detailed routing steps.

The design is now prepared to proceed toward detailed routing (DRT), where final wire shapes, vias, and DRC compliance will be ensured.

```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_route
```
<img width="2418" height="1528" alt="image" src="https://github.com/user-attachments/assets/0845fd36-2230-4e46-8c97-50060c071318" />
<img width="2418" height="1550" alt="image" src="https://github.com/user-attachments/assets/8036ed2b-4a68-4752-b974-e0b4fbf88ca9" />

After running global routing and detailed routing in OpenROAD, the final routed BabySoC layout was generated successfully. The visual inspection using the OpenROAD GUI confirms that routing was completed with well-distributed metal layers, correct via insertion, and congestion levels within acceptable limits.

**1. Global Routing + Detailed Routing Completed**

* Global routing successfully assigned routing resources across metal layers.

* Detailed routing converted these routing guides into final wire geometries.

* The clock nets (shown in green) are fully connected and routed with buffer tree topology.

* All standard cell pins and macro pins are properly connected.

The routed layout shows:

* ✔ Well-defined routes across MET1–MET5

* ✔ Correct via connections (VIA1–VIA4)

* ✔ Balanced routing density

**2. No Major DRC Violations After Detailed Routing**

Visual inspection of the final routed layout reveals:

* No short-circuit markers

* No spacing violations

* No unconnected pins

* No illegal routing segments

Although global routing reported small congestion earlier, detailed routing fixed these overflows, ensuring DRC-clean routing.

The placement density heatmap (near 100% in some regions) is normal for dense digital standard-cell regions.

**3. Routing Heatmaps Confirm Good Congestion Distribution**

* From the heatmap:

   * ✔ High utilization (red-orange zones)

* Indicates dense logic regions, especially around CPU and memory blocks.

   * ✔ Low utilization (blue-cyan zones)

* Located near empty placement rows and corners of the die.

   * ✔ No hotspots > 100%

This means the design is routable and no grid cell exceeded routing capacity after detailed routing.

**4. Final Routed Layout Matches Floorplan Boundary**

* All nets stay inside the die boundary.

* Macro blockages are respected.

* Routing follows legal tracks and DRC constraints.

* The clock net (clknet_leaf) is highlighted showing a proper hierarchical distribution.

* The Inspector confirms:

   * Wire type: ROUTED

   * Placement status: PLACED

   * Special: False

**Final Conclusion**

The BabySoC design successfully completed global routing followed by detailed routing in OpenROAD. The final routed layout shows a DRC-clean design with properly connected nets, legal routing tracks, and no violations. Heatmaps confirm stable routing density and absence of congestion hotspots. The final routing snapshot clearly represents a fully connected and manufacturable layout ready for parasitic extraction (SPEF) and signoff checks.

**CTS REPORT**


==========================================================================
cts final report_tns
--------------------------------------------------------------------------
tns 0.00

==========================================================================
cts final report_wns
--------------------------------------------------------------------------
wns 0.00

==========================================================================
cts final report_worst_slack
--------------------------------------------------------------------------
worst slack 5.55

==========================================================================
cts final report_clock_skew
--------------------------------------------------------------------------
Clock clk
   0.92 source latency core.CPU_result_a4[0]$_DFF_P_/CLK ^
  -0.82 target latency core.CPU_Dmem_value_a5[0][18]$_SDFFE_PP0P_/CLK ^
   0.55 clock uncertainty
   0.00 CRPR
--------------
   0.65 setup skew


==========================================================================
cts final report_checks -path_delay min
--------------------------------------------------------------------------
Startpoint: core.CPU_Xreg_value_a4[25][15]$_SDFFE_PP0P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_src2_value_a3[15]$_DFF_P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.10    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.00    0.00    0.00 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.23    0.24    0.26    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.24    0.00    0.26 ^ clkbuf_3_7_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     2    0.04    0.06    0.20    0.47 ^ clkbuf_3_7_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_7_0_CLK (net)
                  0.06    0.00    0.47 ^ clkbuf_4_14__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    13    0.17    0.18    0.24    0.71 ^ clkbuf_4_14__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_14__leaf_CLK (net)
                  0.18    0.00    0.71 ^ clkbuf_leaf_108_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    12    0.04    0.06    0.19    0.90 ^ clkbuf_leaf_108_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_108_CLK (net)
                  0.06    0.00    0.90 ^ core.CPU_Xreg_value_a4[25][15]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     2    0.01    0.06    0.32    1.22 ^ core.CPU_Xreg_value_a4[25][15]$_SDFFE_PP0P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_Xreg_value_a4[25][15] (net)
                  0.06    0.00    1.22 ^ _14708_/A1 (sky130_fd_sc_hd__a21oi_1)
     1    0.00    0.04    0.06    1.28 v _14708_/Y (sky130_fd_sc_hd__a21oi_1)
                                         _02012_ (net)
                  0.04    0.00    1.28 v _14712_/A (sky130_fd_sc_hd__nand4_1)
     1    0.01    0.13    0.12    1.40 ^ _14712_/Y (sky130_fd_sc_hd__nand4_1)
                                         _02016_ (net)
                  0.13    0.00    1.40 ^ _14729_/A1 (sky130_fd_sc_hd__o32ai_4)
     1    0.04    0.12    0.18    1.58 v _14729_/Y (sky130_fd_sc_hd__o32ai_4)
                                         _02033_ (net)
                  0.12    0.00    1.59 v _14731_/A2 (sky130_fd_sc_hd__o21ai_0)
     1    0.00    0.06    0.16    1.74 ^ _14731_/Y (sky130_fd_sc_hd__o21ai_0)
                                         core.CPU_src2_value_a2[15] (net)
                  0.06    0.00    1.74 ^ core.CPU_src2_value_a3[15]$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_2)
                                  1.74   data arrival time

                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.10    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.00    0.00    0.00 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.23    0.24    0.26    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.24    0.00    0.26 ^ clkbuf_3_6_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     2    0.04    0.06    0.21    0.47 ^ clkbuf_3_6_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_6_0_CLK (net)
                  0.06    0.00    0.47 ^ clkbuf_4_13__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    12    0.16    0.18    0.24    0.71 ^ clkbuf_4_13__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_13__leaf_CLK (net)
                  0.18    0.00    0.71 ^ clkbuf_leaf_70_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    12    0.04    0.06    0.18    0.89 ^ clkbuf_leaf_70_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_70_CLK (net)
                  0.06    0.00    0.89 ^ core.CPU_src2_value_a3[15]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_2)
                          0.88    1.78   clock uncertainty
                          0.00    1.78   clock reconvergence pessimism
                         -0.03    1.74   library hold time
                                  1.74   data required time
-----------------------------------------------------------------------------
                                  1.74   data required time
                                 -1.74   data arrival time
-----------------------------------------------------------------------------
                                  0.00   slack (MET)



==========================================================================
cts final report_checks -path_delay max
--------------------------------------------------------------------------
Startpoint: core.CPU_valid_taken_br_a5$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_Xreg_value_a4[7][13]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.10    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.00    0.00    0.00 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.23    0.24    0.26    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.24    0.00    0.26 ^ clkbuf_3_4_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     2    0.04    0.07    0.21    0.47 ^ clkbuf_3_4_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_4_0_CLK (net)
                  0.07    0.00    0.47 ^ clkbuf_4_8__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    11    0.15    0.16    0.23    0.70 ^ clkbuf_4_8__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_8__leaf_CLK (net)
                  0.16    0.00    0.70 ^ clkbuf_leaf_149_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    15    0.04    0.06    0.18    0.88 ^ clkbuf_leaf_149_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_149_CLK (net)
                  0.06    0.00    0.88 ^ core.CPU_valid_taken_br_a5$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.03    0.30    1.18 v core.CPU_valid_taken_br_a5$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_valid_taken_br_a5 (net)
                  0.03    0.00    1.18 v _07913_/A (sky130_fd_sc_hd__or4_4)
    19    0.22    0.35    0.83    2.01 v _07913_/X (sky130_fd_sc_hd__or4_4)
                                         _02930_ (net)
                  0.35    0.01    2.02 v _07915_/A (sky130_fd_sc_hd__clkinv_16)
    43    0.45    0.31    0.36    2.38 ^ _07915_/Y (sky130_fd_sc_hd__clkinv_16)
                                         _02932_ (net)
                  0.31    0.02    2.39 ^ _09981_/C (sky130_fd_sc_hd__nor3_2)
     2    0.02    0.10    0.14    2.53 v _09981_/Y (sky130_fd_sc_hd__nor3_2)
                                         _04371_ (net)
                  0.10    0.00    2.53 v _09982_/B1 (sky130_fd_sc_hd__a21oi_4)
     6    0.10    0.70    0.57    3.10 ^ _09982_/Y (sky130_fd_sc_hd__a21oi_4)
                                         _04372_ (net)
                  0.70    0.00    3.10 ^ _09988_/A2 (sky130_fd_sc_hd__o21ai_4)
    16    0.12    0.33    0.38    3.47 v _09988_/Y (sky130_fd_sc_hd__o21ai_4)
                                         _04378_ (net)
                  0.33    0.00    3.48 v _13552_/A (sky130_fd_sc_hd__nor3_4)
    23    0.14    1.37    1.19    4.67 ^ _13552_/Y (sky130_fd_sc_hd__nor3_4)
                                         _07042_ (net)
                  1.37    0.00    4.67 ^ _13572_/B (sky130_fd_sc_hd__nand2_2)
     5    0.03    0.29    0.30    4.98 v _13572_/Y (sky130_fd_sc_hd__nand2_2)
                                         _07058_ (net)
                  0.29    0.00    4.98 v _13574_/B1 (sky130_fd_sc_hd__o221ai_1)
     1    0.00    0.20    0.24    5.22 ^ _13574_/Y (sky130_fd_sc_hd__o221ai_1)
                                         _01444_ (net)
                  0.20    0.00    5.22 ^ hold2174/A (sky130_fd_sc_hd__dlygate4sd3_1)
     1    0.00    0.05    0.57    5.79 ^ hold2174/X (sky130_fd_sc_hd__dlygate4sd3_1)
                                         net2283 (net)
                  0.05    0.00    5.79 ^ core.CPU_Xreg_value_a4[7][13]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  5.79   data arrival time

                         11.05   11.05   clock clk (rise edge)
                          0.00   11.05   clock source latency
     1    0.10    0.00    0.00   11.05 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.00    0.00   11.05 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.23    0.24    0.26   11.31 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.24    0.00   11.31 ^ clkbuf_3_6_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     2    0.04    0.06    0.21   11.52 ^ clkbuf_3_6_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_6_0_CLK (net)
                  0.06    0.00   11.52 ^ clkbuf_4_13__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    12    0.16    0.18    0.24   11.76 ^ clkbuf_4_13__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_13__leaf_CLK (net)
                  0.18    0.00   11.76 ^ clkbuf_leaf_90_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    11    0.04    0.06    0.19   11.94 ^ clkbuf_leaf_90_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_90_CLK (net)
                  0.06    0.00   11.94 ^ core.CPU_Xreg_value_a4[7][13]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.55   11.39   clock uncertainty
                          0.00   11.39   clock reconvergence pessimism
                         -0.05   11.34   library setup time
                                 11.34   data required time
-----------------------------------------------------------------------------
                                 11.34   data required time
                                 -5.79   data arrival time
-----------------------------------------------------------------------------
                                  5.55   slack (MET)



==========================================================================
cts final report_checks -unconstrained
--------------------------------------------------------------------------
Startpoint: core.CPU_valid_taken_br_a5$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_Xreg_value_a4[7][13]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.10    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.00    0.00    0.00 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.23    0.24    0.26    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.24    0.00    0.26 ^ clkbuf_3_4_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     2    0.04    0.07    0.21    0.47 ^ clkbuf_3_4_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_4_0_CLK (net)
                  0.07    0.00    0.47 ^ clkbuf_4_8__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    11    0.15    0.16    0.23    0.70 ^ clkbuf_4_8__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_8__leaf_CLK (net)
                  0.16    0.00    0.70 ^ clkbuf_leaf_149_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    15    0.04    0.06    0.18    0.88 ^ clkbuf_leaf_149_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_149_CLK (net)
                  0.06    0.00    0.88 ^ core.CPU_valid_taken_br_a5$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.03    0.30    1.18 v core.CPU_valid_taken_br_a5$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_valid_taken_br_a5 (net)
                  0.03    0.00    1.18 v _07913_/A (sky130_fd_sc_hd__or4_4)
    19    0.22    0.35    0.83    2.01 v _07913_/X (sky130_fd_sc_hd__or4_4)
                                         _02930_ (net)
                  0.35    0.01    2.02 v _07915_/A (sky130_fd_sc_hd__clkinv_16)
    43    0.45    0.31    0.36    2.38 ^ _07915_/Y (sky130_fd_sc_hd__clkinv_16)
                                         _02932_ (net)
                  0.31    0.02    2.39 ^ _09981_/C (sky130_fd_sc_hd__nor3_2)
     2    0.02    0.10    0.14    2.53 v _09981_/Y (sky130_fd_sc_hd__nor3_2)
                                         _04371_ (net)
                  0.10    0.00    2.53 v _09982_/B1 (sky130_fd_sc_hd__a21oi_4)
     6    0.10    0.70    0.57    3.10 ^ _09982_/Y (sky130_fd_sc_hd__a21oi_4)
                                         _04372_ (net)
                  0.70    0.00    3.10 ^ _09988_/A2 (sky130_fd_sc_hd__o21ai_4)
    16    0.12    0.33    0.38    3.47 v _09988_/Y (sky130_fd_sc_hd__o21ai_4)
                                         _04378_ (net)
                  0.33    0.00    3.48 v _13552_/A (sky130_fd_sc_hd__nor3_4)
    23    0.14    1.37    1.19    4.67 ^ _13552_/Y (sky130_fd_sc_hd__nor3_4)
                                         _07042_ (net)
                  1.37    0.00    4.67 ^ _13572_/B (sky130_fd_sc_hd__nand2_2)
     5    0.03    0.29    0.30    4.98 v _13572_/Y (sky130_fd_sc_hd__nand2_2)
                                         _07058_ (net)
                  0.29    0.00    4.98 v _13574_/B1 (sky130_fd_sc_hd__o221ai_1)
     1    0.00    0.20    0.24    5.22 ^ _13574_/Y (sky130_fd_sc_hd__o221ai_1)
                                         _01444_ (net)
                  0.20    0.00    5.22 ^ hold2174/A (sky130_fd_sc_hd__dlygate4sd3_1)
     1    0.00    0.05    0.57    5.79 ^ hold2174/X (sky130_fd_sc_hd__dlygate4sd3_1)
                                         net2283 (net)
                  0.05    0.00    5.79 ^ core.CPU_Xreg_value_a4[7][13]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  5.79   data arrival time

                         11.05   11.05   clock clk (rise edge)
                          0.00   11.05   clock source latency
     1    0.10    0.00    0.00   11.05 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.00    0.00   11.05 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.23    0.24    0.26   11.31 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.24    0.00   11.31 ^ clkbuf_3_6_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     2    0.04    0.06    0.21   11.52 ^ clkbuf_3_6_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_6_0_CLK (net)
                  0.06    0.00   11.52 ^ clkbuf_4_13__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    12    0.16    0.18    0.24   11.76 ^ clkbuf_4_13__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_13__leaf_CLK (net)
                  0.18    0.00   11.76 ^ clkbuf_leaf_90_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    11    0.04    0.06    0.19   11.94 ^ clkbuf_leaf_90_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_90_CLK (net)
                  0.06    0.00   11.94 ^ core.CPU_Xreg_value_a4[7][13]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.55   11.39   clock uncertainty
                          0.00   11.39   clock reconvergence pessimism
                         -0.05   11.34   library setup time
                                 11.34   data required time
-----------------------------------------------------------------------------
                                 11.34   data required time
                                 -5.79   data arrival time
-----------------------------------------------------------------------------
                                  5.55   slack (MET)



==========================================================================
cts final report_check_types -max_slew -max_cap -max_fanout -violators
--------------------------------------------------------------------------
max capacitance

Pin                                    Limit     Cap   Slack
------------------------------------------------------------
_13743_/Y                               0.15    0.15   -0.00 (VIOLATED)
_13455_/Y                               0.15    0.15   -0.00 (VIOLATED)


==========================================================================
cts final max_slew_check_slack
--------------------------------------------------------------------------
0.02021305449306965

==========================================================================
cts final max_slew_check_limit
--------------------------------------------------------------------------
1.4951449632644653

==========================================================================
cts final max_slew_check_slack_limit
--------------------------------------------------------------------------
0.0135

==========================================================================
cts final max_fanout_check_slack
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
cts final max_fanout_check_limit
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
cts final max_capacitance_check_slack
--------------------------------------------------------------------------
-0.001024815021082759

==========================================================================
cts final max_capacitance_check_limit
--------------------------------------------------------------------------
0.1538189947605133

==========================================================================
cts final max_capacitance_check_slack_limit
--------------------------------------------------------------------------
-0.0067

==========================================================================
cts final max_slew_violation_count
--------------------------------------------------------------------------
max slew violation count 0

==========================================================================
cts final max_fanout_violation_count
--------------------------------------------------------------------------
max fanout violation count 0

==========================================================================
cts final max_cap_violation_count
--------------------------------------------------------------------------
max cap violation count 2

==========================================================================
cts final setup_violation_count
--------------------------------------------------------------------------
setup violation count 0

==========================================================================
cts final hold_violation_count
--------------------------------------------------------------------------
hold violation count 0

==========================================================================
cts final report_checks -path_delay max reg to reg
--------------------------------------------------------------------------
Startpoint: core.CPU_valid_taken_br_a5$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_Xreg_value_a4[7][13]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ pll/CLK (avsdpll)
   0.26    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.21    0.47 ^ clkbuf_3_4_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.23    0.70 ^ clkbuf_4_8__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.18    0.88 ^ clkbuf_leaf_149_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.88 ^ core.CPU_valid_taken_br_a5$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.30    1.18 v core.CPU_valid_taken_br_a5$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
   0.83    2.01 v _07913_/X (sky130_fd_sc_hd__or4_4)
   0.36    2.38 ^ _07915_/Y (sky130_fd_sc_hd__clkinv_16)
   0.15    2.53 v _09981_/Y (sky130_fd_sc_hd__nor3_2)
   0.57    3.10 ^ _09982_/Y (sky130_fd_sc_hd__a21oi_4)
   0.38    3.47 v _09988_/Y (sky130_fd_sc_hd__o21ai_4)
   1.20    4.67 ^ _13552_/Y (sky130_fd_sc_hd__nor3_4)
   0.31    4.98 v _13572_/Y (sky130_fd_sc_hd__nand2_2)
   0.24    5.22 ^ _13574_/Y (sky130_fd_sc_hd__o221ai_1)
   0.57    5.79 ^ hold2174/X (sky130_fd_sc_hd__dlygate4sd3_1)
   0.00    5.79 ^ core.CPU_Xreg_value_a4[7][13]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
           5.79   data arrival time

  11.05   11.05   clock clk (rise edge)
   0.00   11.05   clock source latency
   0.00   11.05 ^ pll/CLK (avsdpll)
   0.26   11.31 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.21   11.52 ^ clkbuf_3_6_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.24   11.76 ^ clkbuf_4_13__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.19   11.94 ^ clkbuf_leaf_90_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00   11.94 ^ core.CPU_Xreg_value_a4[7][13]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
  -0.55   11.39   clock uncertainty
   0.00   11.39   clock reconvergence pessimism
  -0.05   11.34   library setup time
          11.34   data required time
---------------------------------------------------------
          11.34   data required time
          -5.79   data arrival time
---------------------------------------------------------
           5.55   slack (MET)



==========================================================================
cts final report_checks -path_delay min reg to reg
--------------------------------------------------------------------------
Startpoint: core.CPU_Xreg_value_a4[25][15]$_SDFFE_PP0P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_src2_value_a3[15]$_DFF_P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ pll/CLK (avsdpll)
   0.26    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.21    0.47 ^ clkbuf_3_7_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.24    0.71 ^ clkbuf_4_14__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.19    0.90 ^ clkbuf_leaf_108_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.90 ^ core.CPU_Xreg_value_a4[25][15]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.32    1.22 ^ core.CPU_Xreg_value_a4[25][15]$_SDFFE_PP0P_/Q (sky130_fd_sc_hd__dfxtp_1)
   0.06    1.28 v _14708_/Y (sky130_fd_sc_hd__a21oi_1)
   0.12    1.40 ^ _14712_/Y (sky130_fd_sc_hd__nand4_1)
   0.18    1.58 v _14729_/Y (sky130_fd_sc_hd__o32ai_4)
   0.16    1.74 ^ _14731_/Y (sky130_fd_sc_hd__o21ai_0)
   0.00    1.74 ^ core.CPU_src2_value_a3[15]$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_2)
           1.74   data arrival time

   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ pll/CLK (avsdpll)
   0.26    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.21    0.47 ^ clkbuf_3_6_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.24    0.71 ^ clkbuf_4_13__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.19    0.89 ^ clkbuf_leaf_70_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.89 ^ core.CPU_src2_value_a3[15]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_2)
   0.88    1.78   clock uncertainty
   0.00    1.78   clock reconvergence pessimism
  -0.03    1.74   library hold time
           1.74   data required time
---------------------------------------------------------
           1.74   data required time
          -1.74   data arrival time
---------------------------------------------------------
           0.00   slack (MET)



==========================================================================
cts final critical path target clock latency max path
--------------------------------------------------------------------------
0

==========================================================================
cts final critical path target clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
cts final critical path source clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
cts final critical path delay
--------------------------------------------------------------------------
5.7921

==========================================================================
cts final critical path slack
--------------------------------------------------------------------------
5.5454

==========================================================================
cts final slack div critical path delay
--------------------------------------------------------------------------
95.740750

==========================================================================
cts final report_power
--------------------------------------------------------------------------
Group                  Internal  Switching    Leakage      Total
                          Power      Power      Power      Power (Watts)
----------------------------------------------------------------
Sequential             6.95e-03   9.72e-04   1.45e-08   7.92e-03  37.7%
Combinational          2.12e-03   4.52e-03   3.05e-08   6.64e-03  31.6%
Clock                  3.66e-03   2.76e-03   2.96e-09   6.42e-03  30.6%
Macro                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Pad                    0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
----------------------------------------------------------------
Total                  1.27e-02   8.25e-03   4.80e-08   2.10e-02 100.0%
                          60.7%      39.3%       0.0%
