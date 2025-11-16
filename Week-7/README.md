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
<img width="1285" height="804" alt="Screenshot 2025-11-14 123144" src="https://github.com/user-attachments/assets/4cd30d4a-c977-4817-a821-7fb6516de586" />


* **The gds folder would contain the files avsddac.lef and avsdpll.lef**

* **The lib folder would contain the files avsddac.lib and avsdpll.lib**

* **Now copy the constraints file(vsdbabysoc_synthesis.sdc) from the VSDBabySoC folder in your system into this directory.**

* **Now copy the files(macro.cfg and pin_order.cfg) from the VSDBabySoC folder in your system into this directory.**
