# Advanced-Physical-Design-Using-OpenLane--SKY130

# Advanced-Physical-Design-Using-OpenLane-SKY130

# Table of Contents

- [Design](#Design)
- [Open Source Digital ASIC Design](#Open-Source-Digital-ASIC-Design)
- [Openlane](#Openlane)
- [OpenLANE ASIC Flow](#OpenLANE-ASIC-Flow)
- [OpenLane ](#OpenLane)
- [OpenLane design stages](#OpenLane-design-stages)
- [Day 1](#Day-1)
	- [Design Preparation Steps](#Design-Preparation-Steps)
- [Day 2](#Day-2)
	- [FloorPlan and Library Cells](#FloorPlan-and-Library-Cells)
	- [Timing characterization](#Timing-characterization)
- [Day 3](#Day-3)
	- [Inverter standard cell characterization](#Inverter-standard-cell-characterization)
	- [MAGIC FEATURES & DRC Rules](#-MAGIC-FEATURES-&-DRC-Rules)
- [References](#references)
- [Acknowledgement](#acknowledgement)
- [Author](#author)

## OpenLane 

OpenLane is an automated RTL to GDSII flow based on several components including OpenROAD, Yosys, Magic, Netgen, Fault and custom methodology scripts for design exploration and optimization.
For more information check [here](https://openlane.readthedocs.io/)

![openlane flow 1](https://user-images.githubusercontent.com/80625515/130246106-18f73ccc-e8e1-4061-a1b0-8c14bdf711f1.png)

### OpenLane design stages

1. Synthesis
	- `yosys` - Performs RTL synthesis
	- `abc` - Performs technology mapping
	- `OpenSTA` - Performs static timing analysis on the resulting netlist to generate timing reports
2. Floorplan and PDN
	- `init_fp` - Defines the core area for the macro as well as the rows (used for placement) and the tracks (used for routing)
	- `ioplacer` - Places the macro input and output ports
	- `pdn` - Generates the power distribution network
	- `tapcell` - Inserts welltap and decap cells in the floorplan
3. Placement
	- `RePLace` - Performs global placement
	- `Resizer` - Performs optional optimizations on the design
	- `OpenDP` - Perfroms detailed placement to legalize the globally placed components
4. CTS
	- `TritonCTS` - Synthesizes the clock distribution network (the clock tree)
5. Routing
	- `FastRoute` - Performs global routing to generate a guide file for the detailed router
	- `CU-GR` - Another option for performing global routing.
	- `TritonRoute` - Performs detailed routing
	- `SPEF-Extractor` - Performs SPEF extraction
6. GDSII Generation
	- `Magic` - Streams out the final GDSII layout file from the routed def
	- `Klayout` - Streams out the final GDSII layout file from the routed def as a back-up
7. Checks
	- `Magic` - Performs DRC Checks & Antenna Checks
	- `Klayout` - Performs DRC Checks
	- `Netgen` - Performs LVS Checks
	- `CVC` - Performs Circuit Validity Checks

#Day 1

##Invoking Openlane 

OPENLANE can be used in 2 ways 
- Automatic and Interactive 

here, in this Github repo we are going to do Labs of OpenLane in interactive mode.

To start the openLane first, we need to go the OpenLane Directory 

vsdflow/work/tools/openlane_working_dir/OpenLane
then, do 
1) "make mount"

The command which is going to use For executing OpenLane is given below:-
2) ./flow.tcl -interactive

This command is showing that openlane is running in interactive mode. Now, our next step is to invoke the required openlane package.

For that execute the below command:-

3) package require openlane 0.9

Now, our next step is to prepare a design. As , we know that openlane has numerous number od design example available inside the /Desktop/work/tools/openlane_working_dir/OpenLane/designs

Here, in this github we will prepare the design of picorv32a. For , preparing it's design just execute the below command :- 

4) prep -design picorv32a

after the execution of last step it , will be shown like this:





For checking the creation of that picorv32a file Just Go to design file under OpenLane directory and see there that a folder with name of RUN and  with date ---- is created .

just go to this directory to execute this step :-
Desktop/vsdflow/work/tools/openlane_working_dir/OpenLane/designs/picorv32a/runs

and execute this command

"ls -ltr"

You wil see a RUN folder inside this directory have created.

* Now , we wil run the synthesis in the openlane interactive terminal

using this command:-

run_synthesis


Synthesis starts running and after the synthesis completed a netlist is generated in it's result 
To see the netlist it is available under the following directory 

Desktop/vsdflow/work/tools/openlane_working_dir/OpenLane/designs/picorv32a/runs/results/sysnthesis
To see this generated netlist just execute this command under the following directory which is mentioned above:-

ls -ltr


#Day 2
* Definition of core and Die,Preplaced Cells, placement of decoupling capacitors, pin placement and logical cell placement block.

And we will also explain its some factors like Utilization Fcators And Aspect Ratio.

</p>
<p align="center">
  <img src="https://user-images.githubusercontent.com/90523478/215344362-2b689123-76e6-4dd2-9769-8514387247c3.png"></br>
   fig. : 
</p>

**Core and Die
The core and Die area depends on the dimensions of Logic Gates and due to which it also depends on the dimension of chip. As, whatever dimension of chip will come it will calculate the area accordingly. The core and die area are also depends on the dimensions of Standard Cells.

The wires did not contribute to these dimensions.
**Utilization Factor

Utilization Factor is defined as the ratio of area occupied by the netlist to the total area of the core.

**Aspect ratio

Aspect ratio is defined as the ratio of height to the width.

If aspect ratio is 1 then, it describes the chip as square otherwise it is rectangle. 


**Preplaced cells

A large circuit can be converted into smaller one and it can be called as black box.

**Decoupling capacitors 

These capacitors are placed local to preplaced cells during Floorplanning.

Whenevr any oggling will happen in circuit the decoupling capacitors wil send the current to the circuit and there will be no voltage drop.

The capacitor will refresh itself whenever there is no switching.

**Power planning
The power planning is done on global level which is very important for digital circuits which are prone to voltage drop and ground bounce.

**Pin Placement
The placement of the input /output ports of various circuits vary from designer to designer .

All the input output ports can be placed near to the cells which make use of them. A proper design should be known to arrange the pins.

**Logical cell placement block 

These blocks are placed to differentiate the core area and the input /output area .

Cells should be placed in the area where the pins are placed.


