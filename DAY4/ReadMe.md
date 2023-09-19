
# LABS

Go to the correct directory 
```
/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd
```
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/7228b14e-3d96-40e5-ada5-8e9d3628223a)

type the command
```bash
gedit tracks.info
```
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/7ace2c22-f8d6-4b2e-9c92-1f9dfbdc0597)

Now we converge the grid definition in the layout to track definition, by typing the following command
```
% grid 0.46um 0.34um 0.23um 0.17um
```
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/a28ef575-910e-49e5-ba34-2ea73a413df8)

Here is what the layout looks like after the grid change
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/d5929e81-a85c-44e2-b386-12a83a0b30f6)

    Its confirmed that the pins A and Y are at the intersection of X and Y tracks. So the first condition is met.
    The PR boundary is taking 3 grids on width and 9 grids on height which says that the 2nd condition is also met
# LEF Generation

Since the layout is perfect, we can generate the lef file
1. save the modified layout (with new grid)

    In console, type ```save sky130_vsdinv.mag```
    This saves the modified layout in current working directory

2. Open the file and extract LEF

    Open using  magic -T sky130A.tch sky130_vsdinv.mag
    in the console opened, type ```lef write```
   ![image](https://github.com/aaronghosh/pes_pd/assets/124378527/d33bebaa-28f3-44eb-b975-1fc474e1ff1e)
   a lef file will be generated
   open the lef file
    ![image](https://github.com/aaronghosh/pes_pd/assets/124378527/8ca17e2c-3053-4f50-b840-f82213ff2993)

    We copy the .mag file that we created to the 'src' folder of picorv32a folder.
    We then perform this copy command
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/729b921a-1aa8-415d-86b7-20689081c9fe)

make changes to the config.tcl file. Modified config.tcl file is given below
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/f7fc7312-9cdc-46ed-a422-e93c4f5a15f7)

    Type the following commands in the interactive Openlane window
```
prep -design picorv32a -tag 16-09_19-58 -overwrite
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs 
```
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/7dd62469-0853-41c9-ad2b-1ac5c0fe3fed)

then run the synthesis in this window using the command ```run_synthesis```

![image](https://github.com/aaronghosh/pes_pd/assets/124378527/5d4e98e6-f93d-4c07-ba12-0b7e75339b57)
some of the results are given below
![image](https://github.com/aaronghosh/pes_pd/assets/124378527/76e02c23-470e-4969-b553-13d694e2963c)

**for running the floorplan and placement we type**
```
init_floorplan
run_placement
```

![image](https://github.com/aaronghosh/pes_pd/assets/124378527/7a062bed-6bb1-41ce-a150-f007b2bd4f95)


