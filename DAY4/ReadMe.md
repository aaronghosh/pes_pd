
## Timing Modelling using Delay Tables

Place and routing (PnR) is performed using an abstract view of the GDS files generated by Magic. The abstract information will include metal and pin information. The PnR tool will use the abstract view information, formally defined as LEF information, to perform interconnect routing in conjunction to routing guides generated from the PnR flow.

    Technology LEF - Contains layer information, via information, and restricted DRC rules
    Cell LEF - Abstract information of standard cells

From PnR POV, We have to follow certain guidelines to get standard cell set

    Input and output ports must lie on the intersection of vertical and horizontal tracks
    Width of the standard cell should be odd multiples of the track pitch and height should be odd multiple of vertical track pitch

Track info can be found at :
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/9bb75840-0940-4938-8ca9-adfb184faef7)
he 'tracks.info' file is used during the routing stage.
Routes are the metal traces.
Since the PNR is an automated flow, we need to specify where all we want the routes to go.

![image](https://github.com/aaronghosh/pes_pd/assets/124378527/f5601853-ae76-4db8-b94d-6e6fdb8836a2)

![image](https://github.com/aaronghosh/pes_pd/assets/124378527/e9936e0d-b02f-4336-8ecb-a3a0851473c8)
LEF Generation

Since the layout is perfect, we can generate the lef file
1. save the modified layout (with new grid)

    In console, type save sky130_vsdinv.mag
    This saves the modified layout in current working directory

2. Open the file and extract LEF

    Open using  magic -T sky130A.tch sky130_vsdinv.mag
    in the console opened, type lef write and a lef file will be generated
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/18fe5c7e-ee0e-48d0-a286-5bfbdf6ace6f)

3. Plug the generated lef file into PICORV32a

To do this, we need the lef file, library file that has cells 
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/7ebd657a-2a6e-4280-a494-30bcf21ac1ec)


    Type the following

prep -design picorv32a -tag 16-09_19-58 -overwrite
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs 

    Next we type run_synthesis.
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/eadb6265-3d6e-4aa3-a90f-e53ecae5a898)



    The following results are displayed.

    To run floorplan and placement we type

init_floorplan
run_placement

    Now to view the design we type the command

magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &

![image](https://github.com/aaronghosh/pes_pd/assets/124378527/a74134d9-6e9f-4451-ba64-2987631a7d91)
Timing Analysis with Ideal Clocks using OpenSTA

Configure OpenSTA for Post-Synth Timing Analysis

    We must create two files


![image](https://github.com/aaronghosh/pes_pd/assets/124378527/97232bef-95c9-4d95-8546-18eb18e76a56)



