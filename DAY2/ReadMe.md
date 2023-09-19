# Day 2: Floorplanning, Libraries, and Cell Design

Welcome to Day 2 of the PD Openlane workshop. Today, we'll dive deep into key aspects of the design process, including floorplanning, library usage, and standard cell characterization. These foundational principles are critical for creating efficient and reliable semiconductor devices.

## Chip Floorplanning
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/ef583074-b184-4270-aed7-e9e017e0e372)


### Understanding Chip Floorplan

Chip floorplanning is the initial step in designing integrated circuits (ICs). It involves defining the arrangement and placement of various components, such as transistors, logic gates, memory cells, and interconnects, on a silicon wafer. Here are some important considerations:

1. **Size and Location of Pre-Placed Cells**: Pre-placed cells, including memories, clock gating cells, comparators, and muxes, are manually positioned within the chip's layout. Two key factors come into play:
   - **Utilization Factor**: This represents the ratio of the area utilized by the netlist to the total core area (typically aiming for 50%-70%).
   - **Aspect Ratio**: Aspect ratio is the ratio of height to width, and it affects the overall chip shape (e.g., square or rectangle).

2. **De-coupling Capacitors**: In larger circuits with numerous resistors, voltage drops can impact the charging of capacitors. The solution is to employ de-coupling capacitors, which ensure stable voltage levels, even in the presence of voltage fluctuations due to wire resistance.

3. **Power Planning**: Power planning during floorplanning is crucial for reducing noise in digital circuits caused by voltage droop and ground bounce. This phase includes the creation of a robust Power Distribution Network (PDN) to maintain a stable power supply.

4. **Pin Placement**: Efficient pin placement is essential to minimize buffering, enhance power efficiency, and reduce timing delays. Typically, input pins are placed on the left, output pins on the right, and clock pins with a larger size to minimize resistance.

### Floorplanning in OpenLane

After the synthesis phase, we proceed to floorplanning using the OpenLane toolchain. To initiate floorplanning, use the `run_floorplan` command. This process results in a floorplan layout that can be visualized using the Magic tool.

Before running floorplan, lets look into the switches available for the floorplan stage

## Changes made in the config.tcl for floorplan purpose:
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/35925fd5-acec-47f3-9ded-5c2c6bcbb8e7)

## Now in openlane, enter run_floorplan and the results will be updated at the runs folder
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/9c3ac2c1-2ac4-462d-a6d2-66020954233e)


## (0 0) in DIE AREA Indicates top-left corner co-ordinates and (660.685 671.405) indicates bottom-right corner of the die in micro-meters
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/ffed12de-c9c6-4cbf-bc15-79f7e404fe01)

To view the layout of the floorplan, use the command 

```bash
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

![image](https://github.com/aaronghosh/pes_pd/assets/124378527/ffed12de-c9c6-4cbf-bc15-79f7e404fe01)

![image](https://github.com/aaronghosh/pes_pd/assets/124378527/bd20f182-a675-4d4f-9e45-2d174a802f58)

## Library Binding and Placement
### 1.### Library Binding and Initial Placement

    Library consists of cells, sizes of cells, various flavours and shapes of the cells, Timing, Power and delay information.
    Now, we have the floorplan, netlist and representation of components of netlist in library
    place all the components such that the timing is not disturbed and distribute them properly.

    Library binding and initial placement are integral parts of translating logical descriptions of a design into predefined standard cells. Here's what these steps entail:

- **Netlist Binding**: This process involves matching each component within the design to specific shapes defined within the library. It's about finding the best fit for each component.
- **Initial Placement**: During this phase, the components, along with their respective functionalities, are arranged efficiently on the floorplan. The goal is to minimize delays strategically.
### 2. Placement Optimization

    Some components may be located very far to their inputs which can disturb signal integrity (as wire length increases, RC value increases). Therefore we use repeaters(may be series of buffers) inorder to avoid signal loss but area loss comes into picture.
    Assuming that all the clock signals are working at ideal rate, we do the timing analysis if the current placement works good.

 Placement optimization focuses on refining the physical arrangement of components within an integrated circuit. This phase assumes ideal clock signals, allowing designers to concentrate on enhancing the physical layout without dealing with timing issues related to the clock signal.


### 3. Placement
Use the `run_placement` command to execute placement and visualize the layout in the Magic tool.
```
run_placement
```
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/05225ee3-b985-4dbf-8825-ff1ed73c7710)


## Cell Design and Standard Characterization Flow

Cell design is a critical part of creating standard cell libraries, which consist of reusable components like logic gates and flip-flops. These libraries play a fundamental role in integrated circuit design.

### Standard Cell Characterization

![Cell Design Flow](insert_image_url_here)

 ## Cell design is done in 3 parts:

 1.   **Inputs**- PDKs (Process design kits), DRC & LVS rules, SPICE models, library & user-defined specs.
 2.   **Design Steps** - Design steps of cell design involves Circuit Design, Layout Design, Characterization. The software GUNA used for characterization. The characterization can be classified as Timing characterization, Power characterization and Noise characterization.
 3.   **Outputs** - Outputs of the Design are CDL (Circuit Description Language), GDSII, LEF, extracted Spice netlist (.cir), timing, noise, power.libs, function.

Standard cell characterization involves the following steps:

1. **Associating a Model File**: CMOS model files containing property definitions are linked to the cells.

2. **Defining Process Corners**: Different process corners are defined for the target cell's characterization to account for variations.

3. **Thresholds for Cell Delay and Slew**: Thresholds such as slew_low_rise_thr, slew_high_rise_thr, slew_low_fall_thr, and slew_high_fall_thr are set to define the timing characteristics of the cell.

4. **Timing and Power Tables**: Tables for timing and power are specified to characterize the cell's behavior under various conditions.

5. **Incorporating Parasitic-Extracted Netlist**: The netlist with parasitic elements is incorporated to simulate the cell's performance accurately.

6. **Applying Stimuli and Simulations**: Input signals are applied to the cell, and simulations are performed to characterize its behavior.

### Key Timing Thresholds

- **Propagation Delay**: Calculated as time(out_thr) - time(in_thr), propagation delay is a critical parameter for circuit performance.
- **Transition Time**: The difference between time(slew_high_rise_thr) and time(slew_low_rise_thr) is known as transition time.

## Conclusion

Day 2 has been a deep dive into the world of integrated circuit design. We've explored the intricacies of chip floorplanning, library usage, and standard cell characterization. These concepts form the foundation of semiconductor device design, enabling the creation of efficient and reliable ICs.

