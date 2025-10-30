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



