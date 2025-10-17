# DAY2

## LAB1

```bash
#open the file
vim day2_nfet_idvds_L015_W039.spice

```
<img width="1257" height="803" alt="image" src="https://github.com/user-attachments/assets/9618e0c8-bd6e-4673-af35-b30f68469a06" />

```bash
#plot the characteristics [Id vs Vds]
ngspice day2_nfet_idvds_L015_W039.spice

plot -vdd#branch
```
<img width="1258" height="798" alt="image" src="https://github.com/user-attachments/assets/f27e482e-94b2-40d3-ba6e-7f06246e60ff" />
<img width="1261" height="803" alt="image" src="https://github.com/user-attachments/assets/9ef8e274-b43c-4a5f-9c87-d164b04de5bd" />

```bash
#open the file [Id vs Vgs]
vim day2_nfet_idvgs_L015_W039.spice

```
<img width="1225" height="808" alt="image" src="https://github.com/user-attachments/assets/9b87cd81-b760-4285-9f61-f2fa116e3a5b" />


```bash
#plot the characteristics [Id vs Vds]
ngspice day2_nfet_idvgs_L015_W039.spice

plot -vdd#branch
```
<img width="1228" height="803" alt="image" src="https://github.com/user-attachments/assets/96ecad05-d642-45a5-a4d8-17c5528eca6f" />
<img width="1227" height="802" alt="image" src="https://github.com/user-attachments/assets/478184d9-c76a-469b-ab1e-d7928ef8b199" />
<img width="1253" height="807" alt="image" src="https://github.com/user-attachments/assets/4f8c8cd7-6798-4185-a912-f545018c6a01" />

|  # | Tangent point used (x₀, y₀) | Tangent slope (dy/dx) (A/V) | Extrapolated intercept (V) → Estimated V<sub>th</sub> | Comment                                               |
| -: | --------------------------- | --------------------------: | ----------------------------------------------------: | ----------------------------------------------------- |
|  1 | (1.40000 V, 1.1871e-04 A)   |                 1.98879e-04 |                                         **0.80310 V** | Tangent taken around 1.2–1.4 V region                 |
|  2 | (1.20374 V, 7.96774e-05 A)  |                 1.92793e-04 |                                         **0.79046 V** | Tangent spanning ~1.20 → 0.99 region                  |
|  3 | (1.60374 V, 1.58065e-04 A)  |                 1.94952e-04 |                                         **0.79295 V** | Tangent from higher-V region                          |

