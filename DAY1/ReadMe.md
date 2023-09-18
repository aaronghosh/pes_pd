# Open Source EDA Workshop - Day 1

Welcome to the VLSI PHYSICAL DESIGN COURSE. This is the openlane workshop - Day 1. In this workshop, we will explore the inception of open-source EDA, Opennlane, and Skywater 130.

## Skywater-130 PDK

![Skywater-130 PDK](https://github.com/aaronghosh/pes_pd/assets/124378527/d75817ab-4b2d-47e8-b37e-5f8fc7f830cf)

The Skywater PDK (Process Design Kit) files we are working with are described under `$PDK_ROOT`. Here's an overview of its components:

- **Skywater-pdk:** Contains all the foundry provided PDK related files.
- **Open_pdks:** Contains scripts that bridge the gap between closed-source and open-source PDKs, ensuring compatibility with EDA tools.
- **Sky130A:** Houses open-source compatible PDK files.

## Invoking OpenLane

![Importing Package](https://github.com/aaronghosh/pes_pd/assets/124378527/cdf510d1-a636-47dd-af52-56a4356c30cf)

Before we can dive into the workshop, let's make sure we have all the necessary software dependencies for OpenLane. Run the following command:

```bash
package require openlane 0.9
```
## Design Hierarchy

![Design Hierarchy](https://github.com/aaronghosh/pes_pd/assets/124378527/c6478112-e0f5-457e-a42e-61eb1f701b20)

Understanding the hierarchy of designs is fundamental to our work with OpenLane. It allows us to navigate and manipulate the various components effectively.

## Config File Example Content

![Config File Example](https://github.com/aaronghosh/pes_pd/assets/124378527/b5d6da85-197d-4758-85b6-71d9e3177dce)

The `config.tcl` files play a vital role in configuring OpenLane for specific designs. Here's an example of its contents:

## Prepare the Design for the Flow

![Prepare Design](https://github.com/aaronghosh/pes_pd/assets/124378527/5477cf35-9d64-4abf-93f0-4f4690019108)

Once you're ready to begin the workflow, use the `prep` command to prepare the design for processing.

## Synthesis

![Synthesis](https://github.com/aaronghosh/pes_pd/assets/124378527/b86c07f1-03a0-4b74-b15c-e2893828df9a)

To synthesize the design, use the `run_synthesis` command. This command invokes Yosys and other tools necessary for the synthesis process. Be patient, as this can be a time-consuming step.

The flop ratio, which can be calculated as the number of flip-flops divided by the total number of cells, is a crucial metric in this phase.

For example, if we have:
No. of flops: 1613
Total number of cells: 14876

The flop ratio is: 1613 / 14876 = 0.108 (or 10.8% of the total number of cells are flip-flops).

## Conclusion

Under the `runs` folder, you can explore the netlist files generated after synthesis.

That's it for Day 1 of our Open Source EDA Workshop. Stay tuned for more exciting insights and hands-on experiences in the upcoming sessions!

