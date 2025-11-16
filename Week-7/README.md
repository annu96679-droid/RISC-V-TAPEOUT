# Route SPEF Generation

Automated RTL2GDS Flow for VSDBabySoC:

Initial Steps:

* We need to create a directory vsdbabysoc inside OpenROAD-flow-scripts/flow/designs/sky130hd

<img width="1284" height="797" alt="Screenshot 2025-11-14 120633" src="https://github.com/user-attachments/assets/6295bea4-87ec-4fca-ad8d-7640ff8d5812" />

* Now create a directory vsdbabysoc inside OpenROAD-flow-scripts/flow/designs/src and include all the verilog files here.

<img width="1290" height="804" alt="Screenshot 2025-11-14 121943" src="https://github.com/user-attachments/assets/11efb7c9-8b5a-4bf6-8b22-150ade0043ae" />
<img width="1287" height="806" alt="Screenshot 2025-11-14 121954" src="https://github.com/user-attachments/assets/8f15ba3e-ca66-4802-a2da-ddbadae1fdcc" />

* Now copy the folders gds, include, lef and lib from the VSDBabySoC folder in your system into this directory.

<img width="1288" height="802" alt="Screenshot 2025-11-14 122442" src="https://github.com/user-attachments/assets/32f4027b-ddbe-469d-b10e-8280e9b46f43" />
<img width="1284" height="801" alt="Screenshot 2025-11-14 122741" src="https://github.com/user-attachments/assets/49b6b915-8fac-491e-9584-48b0d491c988" />

* **The gds folder would contain the files avsddac.gds and avsdpll.gds**

* The include folder would contain the files sandpiper.vh, sandpiper_gen.vh, sp_default.vh and sp_verilog.vh

<img width="1288" height="805" alt="Screenshot 2025-11-14 123008" src="https://github.com/user-attachments/assets/1888d81c-3ec3-4ef6-81ee-1567b1228def" />

* **The gds folder would contain the files avsddac.lef and avsdpll.lef**

* **The lib folder would contain the files avsddac.lib and avsdpll.lib** 

* **Now copy the constraints file(vsdbabysoc_synthesis.sdc) from the VSDBabySoC folder in your system into this directory.**

* **Now copy the files(macro.cfg and pin_order.cfg) from the VSDBabySoC folder in your system into this directory.**
