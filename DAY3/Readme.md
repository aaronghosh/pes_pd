# SPICE Deck Creation for CMOS Inverter

In this section, we will create a SPICE deck for simulating a CMOS inverter. A SPICE deck contains essential information, including:

- Connectivity Information
- Component Values
- Node Identification
- Node Names
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/4cf26a69-7b0f-4966-b335-2c2faf9c4c79)
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/00665371-6769-4151-8ae7-ae34afdbf73a)

# LAB

Installation
```bash
git clone https://github.com/nickson-jose/vsdstdcelldesign.git
```
## Inverter Layout using Magic

To visualize the layout of a CMOS inverter using Magic, follow these steps:

1. Navigate to the directory containing your Magic installation:

```bash
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
```


2. Launch Magic with the appropriate technology file and layout file:

```bash
magic -T sky130A.tech sky130_inv.mag
```

![Selecting Objects](https://github.com/aaronghosh/pes_pd/assets/124378527/4df359cf-3b01-4bc0-b1a7-b264fe8a8976)


## Exploring the Layout in Magic

In Magic, you can explore the layout by selecting specific layers and devices. Use the 's' key to select objects iteratively until you reach the desired object. Here's how you can do it:

To display information about the selected area, follow these steps:

1. Select a region in the layout.
2. Go to the console window and type `what` to display information about the selected area.

**Checking for DRC errors**
To check drc we should click DRC update and it show the errors in tkon window. 
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/fb9e1c77-b401-4905-b176-8b08ab85e17d)

## Extracting PEX to SPICE with Magic

To extract PEX (parasitic extraction) data to a SPICE netlist in Magic, use the appropriate commands within Magic.
We use the commands
```bash
ext2spice cthresh 0 rthresh 0
```
After this we run another command
```
ext2spice
```

![Displaying Information](https://github.com/aaronghosh/pes_pd/assets/124378527/d945cce5-70ce-4926-ad71-123f8a8b3188)

U will see the .ext and .spice file

Here is the .spice file after our modifications

![Extracting PEX](https://github.com/aaronghosh/pes_pd/assets/124378527/3e0f13c0-f305-4673-9342-e14125a6500e)

## Modified Spice Netlist

To run the SPICE netlist and analyze the results, follow these steps:

1. Run ngspice with the SPICE netlist file (e.g., `sky130_inv.spice`):

```bash
ngspice sky130_inv.spice
```


2. Plot the desired waveforms. For example, to plot `y` vs. `time` for signal `a`, use the following command within the ngspice shell:

```bash
plot y vs time a
```

![image](https://github.com/aaronghosh/pes_pd/assets/124378527/4dc08f92-9ea8-4594-ae4e-7426bcb3836e)

![image](https://github.com/aaronghosh/pes_pd/assets/124378527/e8dc64d5-fded-4801-933c-b706d342cc6c)

Here are some results obtained from the simulation:

- Rise Transition: 0.0395 ns
- Fall Transition: 0.0282 ns

Feel free to explore and analyze the simulation results for your CMOS inverter design.
