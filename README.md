# Advanced-Physical-Design-Using-OpenLane--SKY130



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
</p>
<p align="center">
  <img src="![openlane](https://user-images.githubusercontent.com/88899069/216650468-e8a88814-f76c-46a3-86c1-3dae17077946.png)"></br>
   fig.
</p>





For checking the creation of that picorv32a file Just Go to design file under OpenLane directory and see there that a folder with name of RUN and  with date ---- is created .

just go to this directory to execute this step :-
Desktop/vsdflow/work/tools/openlane_working_dir/OpenLane/designs/picorv32a/runs

and execute this command

"ls -ltr"

You wil see a RUN folder inside this directory have created.

*Synthesis
 Now , we wil run the synthesis in the openlane interactive terminal

using this command:-

run_synthesis


Synthesis starts running and after the synthesis completed a netlist is generated in it's result 
To see the netlist it is available under the following directory 

Desktop/vsdflow/work/tools/openlane_working_dir/OpenLane/designs/picorv32a/runs/results/sysnthesis
To see this generated netlist just execute this command under the following directory which is mentioned above:-

ls -ltr


#Day 2
* Definition of core and Die,Preplaced Cells, placement of decoupling capacitors, pin placement and logical cell placement block.

And we will also explain its some factors like Utilization Factors And Aspect Ratio.



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

*Floorplanning

To run floorplan the following command is used:-

run_floorplan

</p>
<p align="center">
  <img src="https://user-images.githubusercontent.com/88899069/216446905-ebe967e3-826a-4c5d-a89b-40cc1a9e2ab6.png"></br>
   fig.
</p>

The floorplan can be run according to all those configurations which are mentioned in the config. TCL file.


The floorplan can be viewed in the magic using this command :-

 magic -T /home/vanshikatanwar/desktop/vsdflow/work/tools/openlane_working_dir/OpenLane/pdks/volare/sky130/versions/9f1c2b06d2b5a6708cfe0b55679c7e84d37220cc/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.max.lef lef read ../../tmp/merged.min.lef lef read ../../tmp/merged.nom.lef def read picorv32.def &

This is the image which shows the view after opening floorplan in magic tool .




Now, on  doing zoom it is observed that the respective input /output ports and standard cells are shown.


Placement

Placement can be done in 2 stages :-
These stages are
Global and local placement

Global placement does not focus on legalizing the cell while at the same time local placement focused on legalizing the cells.
In global placement the cells can be overlapped or it is possible in global placement that it can be placed even outside the boundary .

To execute the placement this command has to be use :-

run_placement

After placement the result can be shown like this as mentioned below :-

Picture of placement

Placement can be seen in magic tool also , and the command which is used to see the placement in magic tool is given below:-



# Day 3

16 Mask CMOS Process
The process is stated below:-

1.Selecting a substrate :
It's a base on which the whole chip is fabricated .

2.Creating active regions for transistors :

A photolithography process is used using which a small size of pockets are created for placing the PMOS and NMOS in this pocket . These small size pocket is known as active regions. We have make these pockets isolated so,that they don't interfere with each other .

3.N well and P well formartion :

2 wells are created  named as N well and P well and in which N well is used for PMOS fabrication and P well is used for NMOS fabrication.

4.Gate formation :

For controlling the threshold voltage there are 2 important factors which can be used for controlling this voltage. These, factors are doping concentration and the oxide capacitance.This threshold voltage further decided the functioning of the gate .

5.Formation of Lightly Doped Drain (LDD) :

To avoid short channel effect and the hot electron effect a doping profile is achieved for it .

6.Source and Drain formation :
Then, the Source and the Drain are formed

7.Contacts & local Interconnect Creation :

Removing the screen oxide and opens up the source drain and gate region to build the contacts.TiN layer is used for local communication.

8.Higher Level metal layer formation :


The upper layer of  metal and contact holes are drilled by the process of deposition .


Since, the standard cell of inverter will take time to build so, what we will do we will download it's .mag file from the github.

The post layout simulation of inverter has to be done in ngspice from this .mag file which will soon be interfaced with picorv32a design and then all the observations will be noted.



The github repository from which we need to download the .mag file is given below.
Use this command to clone the repository .

git clone https://github.com/nickson-jose/vsdstdcelldesign.git

The tech file of the magic is present in the pdk directory of the openlane and from there we need to copy that tech file to the vsdstdcelldesign directory.

To see the layout of inverter in magic execute this command on terminal which is given below:-

magic -T sky130A.tech sky130_inv.mag &

The layout of the magic can be seen like this :-




All these colour pallets which is showing in magic tool showing us all the layers of the layout. Also, the top right corner specifies the name of the layer according to the colour which is selected.


If you want to know about a particular layer ,then just select an area and write "what" command in tkcon window of magic . It gives you the information regarding the device.
To select a particular layer ,just press S twice there, where your cursor of the mouse is , a white highlighted region come over that layer .


*Invoking OpenLane






For extracting the layout of spice file of inverter ,use the command "extract all" in tkcon window.

Now,after executing this command,when we check the vsdstdcelldesign directory a new file of .ext extension is created inside it which is the extracted file .
This extracted file is further use to create the spice file of inverter for using the ngspice tool .

All the parasitics capacitance are removed and the file is extracted to spice file using the following commands:-



This creates a .spice file in the vsdstdcelldesign directory inside the openlane directory .


We need to edit the netlist for running the transient analysis of inverter :-
The edited netlist is given below:-



To open ngspice just execute this command :-

ngspice sky130Ainv.spice


Four timing parameters are used which we need to calculate from the waveform.These parameters are:

Rise transition delay = it is defined as the  Time taken for the output signal to reach from 20% to 80% of maximum value.

Fall transition delat = it is defined as the Time taken for the output signal to reach from 80% to 20% of max value.

Cell rise delay = it is defined as the Time difference between 50% of rising output and 50% of falling output.

Cell fall delay =it is defined as the Time difference between 50% of falling output and 50% of rising output.



# Day 4

## To Extract LEF files

Now , we need to extract the LEF File of inverter and interfaced it with the picorv32a design . 

LEF file consist of all the layer information, via informations and DRC rule check and error  while the Cell LEF contains all the information of standard cells.

There are 2 conditions given:-

1) It is necessary for the port that it should be placed on the intersection on the horizontal and vertical track. 

2) The width of the standard cell should be in placed in odd multiples of x .

Tracks are used at the time of routing.And the information of track is present in the PDK files with the name tracks.info .

Open this tracks.info file for all the information .

It shows the name li1,met1,and so on. and with this it is also showing some numbers. 

Let's take one example and understand what this number states /what's the meaning of this number and what this number is showing.

"li1 X 0.23 0.46"

This li1 states the layer 1 and this 0.23 is showing the horizontal offset and 0.46 is showing the pitch for the X.Similarly for Y and other metal layers track information is present .


o check and verify that the port A and Y of standard cel Inverter is also a part of intersection or not , for that we need to use the grid option by press "g" from the keyboard on the layout of inverter so, that the grid is activated.

And , also we can activate the grid from mouse by going into the windows option and then click on grid on .

Then, you will be able to see the grid of the layout of inverter .


The grid of inverter wil be seen like this:-

We can also make the grids from that github of vsdstdcelldesign  which are provided there also and also provided in the tracks.info file as :-

command for set the grid is given below:--

The look after the grid size changed accordingly in the image given below:-

Now the command which is used for extracting the LEF file is 

"LEF write " 
write this above command in tkcon window


After executing this command a LEF file is created in the same directory of vsdstdcelldesigns:-
