# SPICE Deck creation for CMOS Inverter

SPICE deck contains the information of netlist such as:

    Connectivity Information
    Component values
    'Nodes' identified
    'Node' names
## Inverter Layout using Magic

cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
magic -T sky130A.tech sky130_inv.mag

## Exploring the Layout displayed by MAGIC

Select the specific layer/device by hovering over the object and pressing, s, iteratively, until you traverse the hierarchy to the specified object:
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/4df359cf-3b01-4bc0-b1a7-b264fe8a8976)

    select a region from the layout, go to the console and type what to display the information of selected area
    To select a region, place cursor on that point and presss. More the number of times you press s, higher the abstraction selected.

![image](https://github.com/aaronghosh/pes_pd/assets/124378527/d945cce5-70ce-4926-ad71-123f8a8b3188)
## Extracting PEX to SPICE with MAGIC
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/3e0f13c0-f305-4673-9342-e14125a6500e)
## Modified Spice netlist
o run the spice netlist, run ngspice sky130_inv.spice and plot y vs time a
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/4dc08f92-9ea8-4594-ae4e-7426bcb3836e)

![image](https://github.com/aaronghosh/pes_pd/assets/124378527/e8dc64d5-fded-4801-933c-b706d342cc6c)

    Rise Transition : 0.0395ns
    Fall transition : 0.0282ns
    Cell Rise delay : 0.03598ns
    Cell fall delay : 0.0483ns

![image](https://github.com/aaronghosh/pes_pd/assets/124378527/aa0fd384-2063-424e-a8c2-ce2bdb1b22ad)

![image](https://github.com/aaronghosh/pes_pd/assets/124378527/d523c825-9c70-4a01-b406-992ad9c821fd)
