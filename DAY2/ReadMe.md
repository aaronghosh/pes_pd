# Good Floorplan vs Bad Floorplan and Introduction to library cells
## Chip Floorplanning Considerations
1. Define Width and height of core and die

    Die : Structure that consists of core which is a small semiconductor material on which the fundamental circuit is fabricated.
    core : Structire that contains primary logic and functional components.

Whenever we come across the concepts of core and die, Utilisation factor plays an important role. UTILISATION FACTOR = Area Occupied by the Netlist / Area of the core (usually 50%-70%) ASPECT RATIO = Height / Width (1 = square, others = rectangle)
2. Define Location of Pre-Placed cells

pre-placed cells : memories, clock gating cells, comparator, mux etc

    The arrangement of these IPs on chip is called FLOORPLANNING
    These IPs have user defined locations and hence are placed in chip before automated placement and routing. Therefore called pre-placed cells.
    Automated PnR tool places the remaining logical cells in design onto chip.

3. De-coupling capacitors

Problem We know that all the combinational blocks are connected to Vdd and Vss for their operation. But when there is a large circuit with many resistors, then The capacitors in the logic might not get fully charged as there occurs voltage deop due to wire metal and the resistors present along the path. So after voltage drop, if the voltage obtained by the logic is within noise margin, then it works well but what if it doesn't?

Solution We use De-Coupling capacitors (A huge capacitance with voltage equal to that of supply voltage) that is placed close to the combinational logic. When the switching activity takes place, it detatches the circuit from main supply and this capacitor acts as power supply.

The local communication has been successfully eshtablished with the solution mentioned above. The global communication is taken care by power planning.
4. Power Planning

    Power planning during the Floorplanning phase is essential to lower noise in digital circuits attributed to voltage droop and ground bounce. Coupling capacitance is formed between interconnect wires and the substrate which needs to be charged or discharged to represent either logic 1 or logic 0.
    When a transition occurs on a net, charge associated with coupling capacitors may be dumped to ground. If there are not enough ground taps charge will accumulate at the tap and the ground line will act like a large resistor, raising the ground voltage and lowering our noise margin. To bypass this problem a robust PDN with many power strap taps are needed to lower the resistance associated with the PDN.

5. Pin Placement

    Pin placement is an essential part of floorplanning to minimize buffering and improve power consumption and timing delays.
    We usually place input pins on the left and output pins on the right
    for primary inputs and outputs, pin size may be small and for clock, the pin size would be large because clock should drive many cells so we need to make sure that the resistance is less.
    larger the area, lesser the resistance.
    Placement blockage is done inorder to makesure that no logic is placed along the area where the pin placement is carried out.

## Floorplan

run_floorplan

Before running floorplan, lets look into the switches available for the floorplan stage

## Changes made in the config.tcl for floorplan purpose:
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/35925fd5-acec-47f3-9ded-5c2c6bcbb8e7)

## Now in openlane, enter run_floorplan and the results will be updated at the runs folder
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/9c3ac2c1-2ac4-462d-a6d2-66020954233e)

(0 0) in DIE AREA Indicates top-left corner co-ordinates and (660.685 671.405) indicates bottom-right corner of the die in micro-meters
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/ffed12de-c9c6-4cbf-bc15-79f7e404fe01)
To view the layout of the floorplan, use the command 


magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &


![image](https://github.com/aaronghosh/pes_pd/assets/124378527/ffed12de-c9c6-4cbf-bc15-79f7e404fe01)

![image](https://github.com/aaronghosh/pes_pd/assets/124378527/bd20f182-a675-4d4f-9e45-2d174a802f58)

## Library Binding and Placement
### 1. Bind the netlist with physical cells

    Library consists of cells, sizes of cells, various flavours and shapes of the cells, Timing, Power and delay information.
    Now, we have the floorplan, netlist and representation of components of netlist in library
    place all the components such that the timing is not disturbed and distribute them properly.

### 2. Optimize Placement

    Some components may be located very far to their inputs which can disturb signal integrity (as wire length increases, RC value increases). Therefore we use repeaters(may be series of buffers) inorder to avoid signal loss but area loss comes into picture.
    Assuming that all the clock signals are working at ideal rate, we do the timing analysis if the current placement works good.

### 3. Placement

run_placement

![image](https://github.com/aaronghosh/pes_pd/assets/124378527/05225ee3-b985-4dbf-8825-ff1ed73c7710)

## Cell Design Flow

Cell design is done in 3 parts:

    Inputs - PDKs (Process design kits), DRC & LVS rules, SPICE models, library & user-defined specs.
    Design Steps - Design steps of cell design involves Circuit Design, Layout Design, Characterization. The software GUNA used for characterization. The characterization can be classified as Timing characterization, Power characterization and Noise characterization.
    Outputs - Outputs of the Design are CDL (Circuit Description Language), GDSII, LEF, extracted Spice netlist (.cir), timing, noise, power.libs, function.

Standard cell Charachterization Flow

Standard Cell Libraries consist of cells with different functionality/drive strengths. These cells need to be characterized by liberty files to be used by synthesis tools to determine optimal circuit arrangement. The open-source software GUNA is used for characterization. Characterization is a well-defined flow consisting of the following steps:

    Link Model File of CMOS containing property definitions
    Specify process corner(s) for the cell to be characterized
    Specify cell delay and slew thresholds percentages
    Specify timing and power tables
    Read the parasitic extracted netlist
    Apply input or stimulus
    Provide necessary simulation commands

General Timing characterization parameters
Timing threshold definitions

    slew_low_rise_thr - 20% from bottom power supply when the signal is rising
    slew_high_rise_thr - 20% from top power supply when the signal is rising
    slew_low_fall_thr - 20% from bottom power supply when the signal is falling
    slew_high_fall_thr - 20% from top power supply when the signal is falling
    in_rise_thr - 50% point on the rising edge of input
    in_fall_thr - 50% point on the falling edge of input
    out_rise_thr - 50% point on the rising edge of ouput
    out_fall_thr - 50% point on the falling edge of ouput

These are the main parameters that we use to calculate factors such as propogation delay and transition time

    propogation delay - time(out_thr) - time(in_thr)
    Transition time - time(slew_high_rise_thr) - time(slew_low_rise_thr)


