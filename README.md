# **Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK**

<img width="420" alt="Screenshot 2025-02-03 at 11 55 14 AM" src="https://github.com/user-attachments/assets/a35ad278-8cfa-4a84-91ad-30cc284e57ea" />

                  SILICON WAFER

Various components in chip:

a. PADS:
       It is a component through which you can send and receive signals inside and outside a chip.
       
b. CORE: 
       A 'core' is the section of the chip where the fundamental logic of the design is placed.
       
c. DIE:
       A die, which consists of core, is small semiconductor material specimen on which the fundamental circuit is fabricated.

d. Foundry IP's: 
            Pre-designed, verified circuit elements provided by semiconductor foundries.
            
e. Macros:
      Predefined, reusable digital circuit blocks, like standard cells, simplifying complex chip design.

**Introduction to RISC-V**

<img width="1470" alt="Screenshot 2025-02-03 at 3 09 11 PM" src="https://github.com/user-attachments/assets/f1e108c4-df64-4684-bb52-a94e734ac498" />

**OpenLANE**

OpenLane is an open-source automated toolchain for designing Application-Specific Integrated Circuits (ASICs) from RTL (Register-Transfer Level) to GDSII (Graphic Data System II) layout. It streamlines the ASIC design process by integrating various open-source tools, allowing for efficient chip development and tape-out without human intervention.

All the designs, we are going to use in this lab are present under the design directory :

![WhatsApp Image 2025-02-03 at 16 08 24](https://github.com/user-attachments/assets/e44ee0a3-ee3c-4434-b18c-430b5a9ace0c)

OpenLANE in interactive mode:

![WhatsApp Image 2025-02-03 at 22 50 35](https://github.com/user-attachments/assets/8958cc99-c60d-4481-9255-389180479330)

Comparsion between config.tcl and sky130A_sky130_fd_sc_hd_congfig.tcl

![WhatsApp Image 2025-02-03 at 22 58 54 (1)](https://github.com/user-attachments/assets/d8824e3d-e079-4555-901b-ae88709116bc)
![WhatsApp Image 2025-02-03 at 22 58 54](https://github.com/user-attachments/assets/d65bf9dc-4e9a-468a-ba6e-2ac18e2d4e19)
 
In OpenLane, the "prep" step is crucial for setting up the file structure and merging essential technology and cell information.

![WhatsApp Image 2025-02-03 at 23 05 30](https://github.com/user-attachments/assets/af1b1e48-1db4-48e0-af82-ac7197243304)

**Synthesis:**

Synthesis is the process of converting RTL (Register Transfer Level) code (ie., Verilog format) to optimized Gate level netlist.

In OpenLANE command to run synthesis is `run_synthesis`
![WhatsApp Image 2025-02-03 at 23 24 08](https://github.com/user-attachments/assets/f9bdd8ac-27cc-47c2-a67e-0745f514e7b1)

**TASK-1**
#### clock ratio = no.of d-ff/no.of cells * 100
clock ratio = 1613/14876 * 100 = 10.843

![WhatsApp Image 2025-02-03 at 23 43 16](https://github.com/user-attachments/assets/577109fe-b382-489e-a56d-d813622044b6)

Results at the end of the synthesis process:

1. Optimized RTL
2. Technology mapped netlist
3. Estimated approx core area/gate count
4. Operating frequency and respective timing reports



# **Day 2 - Floorplanning**

Floorplanning is the art of any physical design. A well and perfect floorplan leads to an design with higher performance and optimum area.
Floorplanning can be challenging in that, it deals with the placement of I/O pads and macros as well as power and ground structure.
Before we are going for the floor planning to make sure that inputs are used for floorplan is prepared properly.

**floorplan parameters**:

core area depends upon : 

**Aspect Ratio:**  Aspect ratio will decide the size and shape of the chip. It is the ratio between horizontal routing resources to vertical routing resources (or) ratio of height and width.    
#### aspect ratio = width/height 

**Core Utilization:** Utilization will define the area occupied by the standard cells, macros, and other cells.If core utilization is 0.8 (80%) that means 80% of the core area is used for placing the standard cells, macros, and other cells, and the remaining 20% is used for routing purposes. 
#### core utilization = area occupied by netlist/ total core area

**Preplaced Cells:** Preplaced cells, often referred to as MACROs, play a crucial role in enabling hierarchical chip design. They allow VLSI engineers to modularize larger designs. In floorplanning, preplaced cells are assigned specific locations within the core area. Blockages are also defined to ensure that standard cells are not placed in the preplaced cell regions.

**Decoupling Capacitors:**
Decoupling capacitors are strategically placed near preplaced cells during Floorplanning. They address voltage drops caused by interconnecting wires, which can disrupt noise margins or induce an indeterminate state in circuits. These capacitors charge up to the power supply voltage over time and act as reservoirs of charge. When the circuit requires a transition, they supply the needed charge, effectively decoupling the circuit from the main power supply and stabilizing operation.

**Power Planning:**
Power planning is a vital aspect of Floorplanning aimed at reducing noise in digital circuits due to voltage droop and ground bounce. Coupling capacitance forms between interconnect wires and the substrate. During transitions on a net, the charge associated with coupling capacitors may be dumped to the ground. Sufficient ground taps and a robust power distribution network (PDN) with multiple power strap taps are essential to lower resistance, maintain ground voltage stability, and enhance noise margins.

**Pin Placement:**
Pin placement optimization is crucial for minimizing buffering, improving power efficiency, and managing timing delays. It involves determining the specific locations along the I/O ring where pins should be placed, guided by the connectivity information of the HDL netlist. Well-optimized pin placement can reduce buffering requirements and subsequently lower power consumption. Blockages are often introduced to distinguish between the core and I/O areas, ensuring proper isolation.

### Floorplan using OpenLANE

![WhatsApp Image 2025-02-04 at 00 29 46](https://github.com/user-attachments/assets/c0d21489-35fe-4d27-8c2f-57417bcf24f4)

In OpenLANE command to run floorplan is `run_floorplan`


![WhatsApp Image 2025-02-04 at 00 39 20](https://github.com/user-attachments/assets/b0475309-7536-4ce6-af33-95202cb0d23e)

Output of floorplan in Magic Layout:

![WhatsApp Image 2025-02-04 at 00 50 49](https://github.com/user-attachments/assets/ec490607-bb50-4043-8bdf-7faeaae28adc)

If we zoom in a little we can see that some of the micro, IO pad, and tap-cells.

<img width="601" alt="Screenshot 2025-02-04 at 1 28 55 AM" src="https://github.com/user-attachments/assets/e3374ddd-80d6-4326-90a9-edfc2ecfe5a7" />

**Placement:**

Placement is a very important stage of physical design where all the standard cells get placed inside the core boundary. 
Placement is performed in two stages

**1. Global Placement:** Optimized but not legal placement. Optimization works to reduce wirelength by reducing half parameter wirelength.

**2. Detailed Placement:** Legalizes placement of cells into standard cell rows while adhering to global placement.


Command to run placement `run_placement`
![WhatsApp Image 2025-02-04 at 12 42 39](https://github.com/user-attachments/assets/20cb634b-27c9-4737-b53f-34c62b229200)

Standard cells legally placed
![WhatsApp Image 2025-02-04 at 12 44 23](https://github.com/user-attachments/assets/7ced0562-ebc1-45b2-8693-b4f9ef9f2c78)


# **Day 3 - Design library cell using Magic Layout and ngspice**

**SPICE DECK:**

- Component connectivity
- Component values
- Identify nodes
- Name nodes

**Layout Design using Magic**
Clone the following repository in the openlane directory to build all the dependencies:

`git clone https://github.com/nickson-jose/vsdstdcelldesign.git`

To invoke the layout of the inverter, use the following command:

`magic -T sky130A.tech sky130_inv.mag &`


![WhatsApp Image 2025-02-04 at 13 58 15](https://github.com/user-attachments/assets/e439ccfe-cebb-48ed-8b43-f3286f911eab)

![WhatsApp Image 2025-02-04 at 14 12 59](https://github.com/user-attachments/assets/1d4752d4-8e93-4ebb-80e1-5531167efab4)


**Extracting Parasitics**

![WhatsApp Image 2025-02-04 at 14 21 26](https://github.com/user-attachments/assets/de9de15f-1019-4d0b-894b-e30a1acf1cd2)

![WhatsApp Image 2025-02-04 at 14 26 36](https://github.com/user-attachments/assets/09888463-5954-4aee-8f6e-29b5cc2ddb44)

![WhatsApp Image 2025-02-04 at 14 29 04](https://github.com/user-attachments/assets/19ce6b55-17c0-4b5d-a257-90649a198af9)


Spice netlist simulation

![WhatsApp Image 2025-02-04 at 16 04 59](https://github.com/user-attachments/assets/b6086c51-2391-4713-abe9-be30a9fd69b8)


# **Day 4 - Pre-layout timing analysis**

During the place and route (PnR) process, an abstract view of the GDS files generated by Magic is used. This abstract view contains crucial information such as metal and pin details. This information is formally defined as LEF (Library Exchange Format) and is utilized by the PnR tool for interconnect routing, in conjunction with routing guides generated during the PnR flow.

There are two main types of LEF files that are essential for the PnR process:

**Technology LEF:** This file contains information about layers, vias, and restricted Design Rule Check (DRC) rules. It specifies the characteristics of the fabrication process.

**Cell LEF:** This file provides an abstract representation of standard cells used in the design. It includes pin information and other essential details.

**Creating Standard Cells**

**1. Port Placement:** Input and output ports of standard cells must align with the intersection of vertical and horizontal tracks. This alignment ensures that signals can be routed efficiently.

**2. Cell Dimensions:** Standard cell width should be an odd multiple of the track pitch, and the height should be an odd multiple of the vertical track pitch. This adherence to odd multiples helps in grid alignment.

**3. Track Information:** Track information can be found in the `tracks.info`.

![WhatsApp Image 2025-02-04 at 16 33 16](https://github.com/user-attachments/assets/db40c6e2-5ddb-4b6a-8887-acc2d588a555)

**Before grid setup:**

![WhatsApp Image 2025-02-04 at 13 58 15](https://github.com/user-attachments/assets/21940375-6528-4e7b-b39c-689790b31cdf)

**After grid setup:**

![WhatsApp Image 2025-02-04 at 17 04 24](https://github.com/user-attachments/assets/0bd91ef2-6e7e-4dab-a3f8-6979337ca068)


![WhatsApp Image 2025-02-04 at 17 17 43](https://github.com/user-attachments/assets/ea10fdd4-0f94-44d3-88ca-f919facc2971)

Overwriting previous run

![WhatsApp Image 2025-02-04 at 17 46 52](https://github.com/user-attachments/assets/2f87b8eb-a31d-46f6-ac13-15e039b47ae1)
![WhatsApp Image 2025-02-04 at 18 09 33](https://github.com/user-attachments/assets/c3b5cc17-9176-49e2-adb8-50ef294fe703)

After sucessful running of synthesis again run floorplan

![WhatsApp Image 2025-02-04 at 18 22 05](https://github.com/user-attachments/assets/fea42f18-5b43-4485-b8c2-beb89cea093d)

![WhatsApp Image 2025-02-04 at 18 25 21](https://github.com/user-attachments/assets/472054b1-5cf7-4b1b-87df-c54052c0786d)

Re-run placement

![WhatsApp Image 2025-02-04 at 18 27 38](https://github.com/user-attachments/assets/4a6754e0-a087-4d28-bc4b-e1f37173300e)


![WhatsApp Image 2025-02-04 at 18 30 39](https://github.com/user-attachments/assets/a0ed9df7-cbcc-48f9-ba82-2f83d2f04980)


![WhatsApp Image 2025-02-04 at 18 34 56](https://github.com/user-attachments/assets/cf2982c7-3e16-4a7d-8fe0-f44569f2ac1f)


![WhatsApp Image 2025-02-04 at 19 59 43](https://github.com/user-attachments/assets/4805d766-4537-4d52-80c9-2ab05b865722)


**Clock Tree Synthesis(CTS):**

Clock Tree Synthesis is a technique for distributing the clock equally among all sequential parts of a VLSI design. The purpose of Clock Tree Synthesis is to reduce skew and delay. Clock Tree Synthesis is provided with the placement data as well as the clock tree limitations as input. Clock Tree Synthesis (CTS) is the technique of balancing the clock delay to all clock inputs by inserting buffers/inverters along the clock routes of an ASIC design. As a result, CTS is used to balance the skew and reduce insertion latency. Before Clock Tree Synthesis, all clock pins were driven by a single clock source. Clock tree synthesis includes both clock tree construction and clock tree balance.

command to run CTS `run_cts`

![WhatsApp Image 2025-02-04 at 20 44 27](https://github.com/user-attachments/assets/b0989b9a-1f20-4b54-9a2c-ffb968bf93ee)

![WhatsApp Image 2025-02-04 at 20 43 43](https://github.com/user-attachments/assets/dec07a66-92d3-43ca-818f-40fd477ec574)

Screenshots of runned commands

![WhatsApp Image 2025-02-04 at 20 57 20](https://github.com/user-attachments/assets/bc80adab-cd22-4b01-a2b7-479c31468ae6)

![WhatsApp Image 2025-02-04 at 21 00 39](https://github.com/user-attachments/assets/bb1dbab6-cd15-4c86-bd31-04432d5391b3)

**Post CTS Analysis**

commands 

![WhatsApp Image 2025-02-04 at 21 05 37](https://github.com/user-attachments/assets/2014ce6f-8202-417e-91b4-63f7b6c2e712)

![WhatsApp Image 2025-02-04 at 21 12 46](https://github.com/user-attachments/assets/78149ed7-8c3e-46ad-82e8-70bb0cc3bea4)

![WhatsApp Image 2025-02-04 at 21 17 46](https://github.com/user-attachments/assets/afd258a9-660e-496e-80b8-3bd45fa042cf)



# **Day 5 - RTL2GDS using tritonRoute**

**Power Distribution Network(PDN)**

The PDN acts as a network of traces and components responsible for distributing power (VDD) effectively and reliably across the integrated circuit (IC). OpenLANE simplifies this process with the following components:

**- Power Ring Global:**
      This is a continuous metal ring encircling the entire IC core, ensuring uniform distribution of power to the core logic and functional blocks. It minimizes voltage drops, guaranteeing power supply to all core regions.

**- Power Halo Local:**
      The power halo forms a localized power distribution network around specific preplaced cells or macroblocks. Preplaced cells remain in fixed positions, and the power halo ensures they receive the necessary power connections.

**- Power Straps:**
      These are metal traces or structures that transport power from the chip's periphery to central regions, reducing the distance power must travel. Power straps maintain consistent power distribution across the entire chip.

**- Power Rails:**
      Metal lines run vertically or horizontally across the chip, supplying power to standard cells. Power rails ensure that each standard cell receives the required power for proper operation.

command to run PDN generation `gen_pdn`

![WhatsApp Image 2025-02-04 at 21 34 47](https://github.com/user-attachments/assets/54c486a3-e035-4a02-9319-b9199dbb5f63)

![WhatsApp Image 2025-02-04 at 21 43 49](https://github.com/user-attachments/assets/aa14df47-96e7-48c7-9ce6-0d4aa277c2b4)

PDN def will displayed by running `magic` command

![WhatsApp Image 2025-02-04 at 21 56 11](https://github.com/user-attachments/assets/bdc767e9-f077-4ccf-9af1-be9a4885f42d)

![WhatsApp Image 2025-02-04 at 21 59 48](https://github.com/user-attachments/assets/43f5e782-1eea-4fd8-adf6-214d20c67406)

**Detailed Routing using TritonRoute**

![WhatsApp Image 2025-02-04 at 22 15 12](https://github.com/user-attachments/assets/c64786b2-35ff-4e0b-accf-ae0ede6c038b)


![WhatsApp Image 2025-02-04 at 22 16 22](https://github.com/user-attachments/assets/d49b3bc8-e6f0-4bbf-a63b-9f0a98528a07)


![WhatsApp Image 2025-02-04 at 22 17 18](https://github.com/user-attachments/assets/c0f910b8-1c5e-48b4-9014-a8e8ea119e88)

![WhatsApp Image 2025-02-04 at 22 18 21](https://github.com/user-attachments/assets/9e422366-fd1c-47ae-9381-0259da5ee030)

**Routed def in magic**

![WhatsApp Image 2025-02-04 at 22 22 59](https://github.com/user-attachments/assets/089f4449-dd1b-4d61-a84f-369cae7f575f)

![WhatsApp Image 2025-02-04 at 22 26 13](https://github.com/user-attachments/assets/c37a3bb7-81a9-46dc-b522-d283274a17fd)

![WhatsApp Image 2025-02-04 at 22 27 53](https://github.com/user-attachments/assets/cceae3f0-653f-4622-aa25-bfeaf509e567)

**Post-Route STA Analysis**

![WhatsApp Image 2025-02-04 at 22 43 28](https://github.com/user-attachments/assets/e727345a-bf94-4dda-b843-73e47459155b)

![WhatsApp Image 2025-02-04 at 22 47 19](https://github.com/user-attachments/assets/ad1d8213-e6b5-4bd9-bcac-1337cb6df4e0)


