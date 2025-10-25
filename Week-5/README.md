# OpenROAD Flow Setup and Floorplan + Placement

## 1. Install OpenROAD Flow Scripts

**OpenROAD (Open Rapid Object-Oriented Design)** is an open-source EDA (Electronic Design Automation) tool that aims to provide a fully autonomous RTL-to-GDSII flow for digital ASIC design.  It supports synthesis, floorplanning, placement, clock tree synthesis, routing, and final layout generation. OpenROAD enables rapid design iterations, making it ideal for academic research and industry prototyping.

## Steps:

```bash
#Clone the OpenROAD Repository
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts

#Open the directory
cd OpenROAD-flow-scripts
```
<img width="1285" height="802" alt="Screenshot 2025-10-21 191140" src="https://github.com/user-attachments/assets/0a94a7fe-ab67-4bf9-be3b-94d71092d706" />
<img width="1281" height="801" alt="Screenshot 2025-10-21 191149" src="https://github.com/user-attachments/assets/4710803b-73ff-4f9d-a99a-8f2833aacdf6" />
<img width="1205" height="145" alt="Screenshot 2025-10-21 191242" src="https://github.com/user-attachments/assets/e9074a47-f100-4dc5-860a-5e2c74fb92f7" />

```bash
#Run the Setup Script
sudo ./setup.sh

```

<img width="1286" height="804" alt="Screenshot 2025-10-21 195213" src="https://github.com/user-attachments/assets/bf9289af-9b74-44a3-a0ad-66c9837a688e" />
<img width="1290" height="800" alt="Screenshot 2025-10-21 195254" src="https://github.com/user-attachments/assets/0028f98d-032e-4a15-9393-fd5e4e130fac" />

```bash
#Build OpenROAD
./build_openroad.sh --local

```

<img width="1274" height="809" alt="Screenshot 2025-10-21 211327" src="https://github.com/user-attachments/assets/4833e7de-c58d-4448-b9d3-c2c401dd08a3" />
