
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
<img width="1146" height="910" alt="Screenshot 2025-11-13 103955" src="https://github.com/user-attachments/assets/d2264af5-db9c-4a77-bb85-1a385c2e8154" /># Day-4




