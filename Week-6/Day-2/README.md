# DAY2

## Chip floorplan consideration
## floorplan view of an integrated circuit (IC)

<img width="797" height="596" alt="image" src="https://github.com/user-attachments/assets/47c6f7c3-702e-4552-871c-dd29ddad2a18" />

**1. Overall Structure**

* The outer orange box labeled “Die” represents the entire chip area — the physical boundary of the silicon die that will be packaged.

* Inside it, the inner orange box labeled “Core” represents the core area — the actual region where the logic of your design (standard cells, macros, blocks, etc.) will be placed.

* The space between die and core is typically reserved for I/O pads, power rings, or bonding connections.

**2. Blue and Gray Grid Lines**

* The horizontal and vertical blue/gray stripes represent metal layers (routing tracks) used for power distribution and signal routing.

* This grid pattern shows power rails (VDD and GND) and routing channels.

* The yellow crosses (X) mark intersections where vias (vertical connections between metal layers) are placed to connect one layer to another.

**3. Grey Blocks (DEC_P1, DEC_P2, DEC_P3, Block a, b, c)**

* These are placed design blocks or standard cell clusters.

* Each block (like DEC_P1, Block a, etc.) represents a functional module of the circuit.

  * For example:

     * DEC_P1, DEC_P2, DEC_P3 → could be decoder modules.

     * Block a, Block b, Block c → might be logic blocks connected to those decoders.

* These are placed within the core area, aligned to the standard cell rows (horizontal gray stripes).

