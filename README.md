# __NASSCOM-VSD-SoC-Design-and-Planning__
## __What is an RTL to GDSII flow?__
Register Transfer Language (RTL) to Graphic Data System II (GDSII) is a comprehensive design flow in integrated circuit (IC) development that transforms a high-level hardware description into a physical layout ready for fabrication in the foundry. The RTL to GDSII process flows through several steps starting from the RTL design, where the circuit's functionality is coded into the hardware description languages using Verilog or VHDL. This RTL code is then converted into a gate-level netlist through a process called synthesis. Once the netlist is created, the first step in the physical design process is floorplanning followed by placement, clock-tree synthesis (CTS), and routing. After placement and routing, signoff checks including Design Rule Checking (DRC), Layout Versus Schematic (LVS) checks, and Static Timing Analysis (STA) are performed. The whole process is iterative until power, performance, and area targets are not met. This process is called Place and Route (PnR).
Finally, the design is exported as a GDSII file, which is used by semiconductor foundries to fabricate the physical ICs from the GDSII layout. A simplified RTL to GDSII process flow is shown below:
<img src="https://private-user-images.githubusercontent.com/58273760/356202255-2091ddf7-e211-408a-9cc8-ffef69762a18.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjQxNzAwNDYsIm5iZiI6MTcyNDE2OTc0NiwicGF0aCI6Ii81ODI3Mzc2MC8zNTYyMDIyNTUtMjA5MWRkZjctZTIxMS00MDhhLTljYzgtZmZlZjY5NzYyYTE4LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDA4MjAlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwODIwVDE2MDIyNlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTUyOTViZDRkODIwNzU5ZjFmZTk1OTBiOTEyZTc2YzY0YTRkZGI5Mjc4NTdiODczZDU4YWZhYTZkN2UyZjY3Y2MmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.MTuVPQ2VAoezy3zhFTZLqi95Wp7ImV9JSdwcYIEIeJc">
__Introduction to Openlane flow__
OpenLANE is a completely automated RTL to GDSII design flow that includes open-source tools and custom scripts for design optimization. Openlane is built around Skywater 130nm process node and is capable of performing full ASIC implementation steps from RTL down to GDSII. The flow-chart below gives a complete openlane design flow from RTL to GDSII
<img src="https://private-user-images.githubusercontent.com/58273760/356255543-503414c9-f122-41d9-a178-b9063fdfde19.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjQxNzAwNDYsIm5iZiI6MTcyNDE2OTc0NiwicGF0aCI6Ii81ODI3Mzc2MC8zNTYyNTU1NDMtNTAzNDE0YzktZjEyMi00MWQ5LWExNzgtYjkwNjNmZGZkZTE5LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDA4MjAlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwODIwVDE2MDIyNlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTMyOGQ3ZGE3MTMzMTYwYjgxMGIzNTY2NzQzNjM0MjliZTBiOTZkYmQ5MzgwNGFmZjNjODIwZTg2ODliMThmZTgmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.XG9Oky-v6EnXjsuqF565A2aEEYOxTXA-smu2H3vF5s4">
__Overview of Physical Design flow__
In ASIC design flow, PnR is the core which consists of several steps. Below are the stages and the respective tools used by OpenLANE:
### 
* Synthesis: Generates gate-level netlist, performs cell mapping, and pre-layout STA.
* Floorplanning: During this step some major decisions are taken like how to partition the system into the subsystems and blocks, how to arrange the blocks on the chip, where to allocate the stdcells, macros, memory, etc. During floorplanning the IO cell and power planning takes place.
* Placement: Placement steps decide the location of stdcells in the design. In this step, the wire length is estimated and therefore placement takes place considering the estimated wire lengths.
* Clock-tree synthesis (CTS): In this step clock tree netlist is implemented, which includes buffers, and the wiring of the clock network is performed. The objective of this step is to minimize the skew and minimize power dissipation.
* Routing (Global and Detailed): Routing creates the wiring layout for all nets other than the clock and power supply. The routing is divided into GLOBAL ROUTING and DETAILED ROUTING. Global routing is the planning stage, where a routing plan for a given net is created by dividing the entire routing region into rectangular tiles or bins. The detailed router decides the actual routing of each pre-assigned globals bins, where the actual wires and vias are created
### __LAB 1:GETTING FAMILIAR WITH OPENLANE EDA TOOLS__
__Design Preparation Step:
### 
* docker
* ./flow.tcl -interactive
* package require openlane 0.9
* prep -design picorv32a
* To view the latest directory created : /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs
  
`command to run synthesis: run_synthesis`

__Review files after design prep and run synthesis__

__Synthesis Report__
![synReport](https://github.com/user-attachments/assets/289fa619-18e7-411d-a5c8-2797fc066904)

__Characterization of Synthesized Results__
![synCalculator](https://github.com/user-attachments/assets/da54ea38-f6ce-475c-8f28-d3d3b3fe2e5d)

#### __LAB 2: FLOORPLANNING & PLACEMENT__

`command to run floorplan : run_floorplan`

__1. def file of floorplan__
![fpDefFile](https://github.com/user-attachments/assets/d649eab8-9b55-477c-8404-c1d508c3c49e)

Command to run floorplan in magic -T
```
# Change directory to path containing generated floorplan def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/29-07_10-25/results/floorplan/

# Command to load the floorplan def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
__2. View of floorplan in magic -T__

![fpMagicT](https://github.com/user-attachments/assets/2117a90a-6fa6-44f2-9063-274a1cd31940)
![fpRealFinalOut](https://github.com/user-attachments/assets/b11ec435-8355-4829-a6b3-558b8520887a)

##### __LAB 3: Placement in openlane__

__Run 'picorv32a' design congestion aware placement using OpenLANE flow and generate necessary outputs__
`run_placement`
![PlacementAnalysis](https://github.com/user-attachments/assets/9bac712b-6c33-4579-89a6-52abb6ef92bf)

__Load placement.def in magic layout__
```
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/29-07_10-25/results/placement/
# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```
![placementOutput](https://github.com/user-attachments/assets/79efcc57-23f4-40e6-a4a1-f7d711187c52)

__HOW TO MAKE CHANGES WHILE BEING IN THE FLOW?__

One can change the floorplan variables like core utilization and IO mode Example : TO CHANGE THE IO pins alignment in the layout, first we can verify the current configuration of the Pins, Go to the following directory as shown in the image below:
`/home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/26-07_10-33/results/floorplan`
Then use the command to open the '.def' file in magic:
` magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def

![day3IOPlacer](https://github.com/user-attachments/assets/ac27eb00-de50-4c64-ad87-4bd941f444bf)

__Steps to get clone of git "vsdstdcelldesign" repo__
The repository "vsdstdcelldesign" contains the .mag file for the inverter and spice models for sky130 nmos/pmos transistors.

Got to the openlane directory and run the following command to clone the git repository:

`git clone https://github.com/nickson-jose/vsdstdcelldesign.git`

Now, we will open the .mag file and to do that we require the sky130A.tech file from the following directory:

`/home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic`

we copy the file using the following command

`cp sky130A.tech /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign`

Now, open the sky130_inv.mag file in magic:

`magic -T sky130A.tech sky130_inv.mag &`

![gitCloneVsdStdCellDesign](https://github.com/user-attachments/assets/43255814-77b1-4e10-8d71-127f7d9507dd)

__EXTRACT THE SPICE NETLIST IN MAGIC__

![ext2spice_inverter](https://github.com/user-attachments/assets/d071a4fd-aa4d-472f-b21c-b4c98b9afa5a)
Spice file created:
![spice file created](https://github.com/user-attachments/assets/51de1851-77af-4f23-8f0e-b577515c834f)

__CREATING FINAL SPICE DECK USING SKY130 TECH__

Now to simulate in ngspice, use the following command while in the 'vsdstdcelldesign' directory:

`ngspice sky130_inv.spice`

Now, to open the plot use plot y vs time a in the ngspice terminal

![ngspice output](https://github.com/user-attachments/assets/962546ca-e9c6-4548-9015-5e929dda3e03)
Inverter Output Values:
![ngspiceInverterplotValues](https://github.com/user-attachments/assets/418ed179-9813-4aec-8c4b-7def15726c8f)

__CHARACTERIZATION OF INVERTER USING SKY130 TECH FILES__

CHARACTERIZE INVERTER USING SKY130 TECH FILES To characterize the inverter, we analyze the ngspice plot and determined the following parameters:

Rise Time: The time for the output waveform to transition from 20% to 80% of its maximum value.
From plot points: (x0 = 2.18192ns, y0 = 0.66049) to (x0 = 2.24571ns, y0 = 2.64018). Calculated Rise Time = 0.0634 ns

Fall Time: The time for the output waveform to transition from 80% to 20% of its maximum value.
From plot points: (x0 = 4.0525ns, y0 = 2.63976) to (x0 = 4.09516ns, y0 = 0.659249). Calculated Fall Time = 0.0422 ns

Propagation Delay(Cell Rise Delay): The time for the output to transition 50% in response to a 50% change at the input.
From plot points: Input(x0 = 2.15018ns, y0 = 1.65018) to Output(x0 = 2.21088ns, y0 = 1.65). Calculated Propagation Delay = 0.064 ns

Cell Fall Delay: The delay for the output to transition 50% due to a 50% change at the input.
From plot points: (x0 = 4.04997ns, y0 = 1.65) to (x0 = 4.07748ns, y0 = 1.65). Calculated Cell Fall Delay = 0.0277 ns

We have now characterized the inverter cell for a room temperature of 27 degC. Similarly, this cell can be characterized for different process, voltage, and temperature (PVT) corners to fully characterize this cell for different PVT corners.

With these parameters successfully characterized, the next step is to create a LEF file from this cell, which will be plugged into openlane picorv32a design flow.

__INTRODUCTION TO MAGIC TOOL AND DRC RULES__

`sudo wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz`

Once downloaded the zip file we extract it using the command
`sudo tar xfz drc_tests.tgz`

__FIXING POLY.9 ERROR IN SKY120 TECH FILE__
![drc rule check](https://github.com/user-attachments/assets/bdf63ee4-4891-4d39-8cae-a77aace9e344)

__FIXING NWELL CHALLENGES__
![nwell](https://github.com/user-attachments/assets/05e4e4db-5959-4e37-9732-38e42a7e26a1)

![nwell challenge last part](https://github.com/user-attachments/assets/10765735-a2ea-4d07-8620-72c7a07fb7cd)

###### __LAB 4: PRE-LAYOUT TIMING ANALYSIS & IMPORTANCE OF GOOD CLOCK TREE__

Open the file name tracks.info. This file specifies pitch, spacing, and other relevant details necessary for efficient routing. Each metal layer has an X and Y direction.

`/home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd`
`less tracks.info`

![tracs file to increase grid](https://github.com/user-attachments/assets/f24e4c84-b1d4-4049-a025-e29c8558c91d)

Increased  Grid size:

![inv grid size increased](https://github.com/user-attachments/assets/9bed2d26-62b7-4b3d-987b-76e95f7b7930)

__Converting magic layout to standard cell LEF__

Port class and port use attributes for a layout: After port definition, the next step is setting port class and port use attributes. These attributes are used to define the purpose of the port. Class and use properties are used by the LEF format for read-and-write routines. Press the button "s" to select the right port layer, then use the following commands:

Creating lef file:

![creating lef file](https://github.com/user-attachments/assets/8c4664bd-3080-4412-8b0b-969fa74d1e1f)

Generated lef file in vsdstdcelldesign:

![generated lef file in vsdstdcelldesign](https://github.com/user-attachments/assets/dd5fb3c3-da32-4794-ab3a-6620dd18d48d)

__Introduction to timing libs and steps to include new cell in the synthesis__

`/home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign`
`cp sky130_vsdinv.lef /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src`
For the synthesis step, we need the std cell library files. Therefore we copy the .lib files from the directory vsdstdcelldesign/libs using the following commands:
`cp sky130_fd_sc_hd__* /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src`

Now, we need to modify the config.tcl file: Go to the picorv32a directory and open the file using vim and we make the following modifications:
<img src="https://private-user-images.githubusercontent.com/58273760/354219563-eb1b1e65-9f32-49d4-a8ce-bde6e48fc988.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjQxNzQyODMsIm5iZiI6MTcyNDE3Mzk4MywicGF0aCI6Ii81ODI3Mzc2MC8zNTQyMTk1NjMtZWIxYjFlNjUtOWYzMi00OWQ0LWE4Y2UtYmRlNmU0OGZjOTg4LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDA4MjAlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwODIwVDE3MTMwM1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTZjOWQ2N2U2ZGQ0YWI2NmMzNzk5Y2Q0ZGVjN2Y5OTg2MTk5MzU2MTM1YjMxMmZlNTkxYTAwZmU2Y2NlMTcxMzAmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.sh_TAJSMWYj3R2aHAYVt0sa5HitTolE2bGGo7ybAPoU">

Now, invoke the docker and perform the regular steps
```
./flow.tcl -interactive
package require openlane 0.9

#to continue the work in the already made directory in the runs folder
prep -design picorv32a -tag 14-08_14-23 -overwrite
```

Synthesis result for custom cell:
![synthesis for custom cell](https://github.com/user-attachments/assets/b5d6b16a-2f54-449d-8be7-d7248bda9914)
![reduced slack time for synthesis](https://github.com/user-attachments/assets/1d3a8f26-6876-4007-b3e1-6b4719e10925)

Reduced Slack time for synthesis:
![reduced slack time for synthesis](https://github.com/user-attachments/assets/166d7f17-4c5d-4adf-bd7c-bb5e2992c35b)

Now, to check whether the std cell we have created has been included in the design or not. Go to the following directory:
`/home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/26-07_10-33/results/placement`
then
`magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &`

![skyvsd_inv in placement](https://github.com/user-attachments/assets/9cdcbfd6-03c0-4633-a13e-57bcff001754)

![expanded custom inv in placement](https://github.com/user-attachments/assets/dcfe104a-e38a-4acb-a876-98a9aa13289f)

__Timing analysis with ideal clocks using openSTA__

Configure OpenSTA for post-synth timing analysis
![pre STA conf file](https://github.com/user-attachments/assets/b463d793-b01b-478e-96e2-d1a1c350a61e)

my_base file for STA:
![my BASE file for STA](https://github.com/user-attachments/assets/695e7541-fa4d-4906-99d5-7f32632ac9ea)

Slack met:
![slack met](https://github.com/user-attachments/assets/6ac197ae-6f5d-4c37-89b3-e2db225a70c0)

__Clock tree synthesis TritonCTS and signal integrity__
```
write_verilog //path of the previous design//
#In our case:
write_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/20-07_16-44/results/synthesis/picorv32a.synthesis.v
```
Synthesis file overwritten by verilog command:
![synthesis file overwritten by write verilog command](https://github.com/user-attachments/assets/58b7444a-485f-4ee9-9d58-40b984ff33b5)

Run floorplan, placement and CTS
```
init_floorplan
place_io
tap_decap_or
```
`run_cts`

CTS result:

![cts sucessful](https://github.com/user-attachments/assets/3d99910e-b2b4-4aa4-af57-10f47e5dee2f)

Generated cts file:
![generated CTS file](https://github.com/user-attachments/assets/ad975164-ca06-4d03-842d-0ee4cdfdae7a)

Post cts Result:
```
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane
docker

# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
![post CTS](https://github.com/user-attachments/assets/43650b5a-2002-419e-9fb3-2e4d16ffcdfe)
```

```
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Command to invoke OpenSTA tool with script
sta pre_sta.conf
```
Include new lef
```
# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a -tag 29-07_10-25 -overwrite

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Command to set new value for SYNTH_MAX_FANOUT
set ::env(SYNTH_MAX_FANOUT) 4

# Command to display current value of variable SYNTH_DRIVING_CELL to check whether it's the proper cell or not
echo $::env(SYNTH_DRIVING_CELL)

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Command to invoke OpenSTA tool with script
sta pre_sta.conf
```
__Timing analysis with real clocks using openSTA__

Post cts OPENROAD TIMING analyis

```
# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/29-07_10-25/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/29-07_10-25/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/29-07_10-25/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Check syntax of 'report_checks' command
help report_checks

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Exit to OpenLANE flow
exit
```
opening openroad:
![opening openroad](https://github.com/user-attachments/assets/b9af5ca5-4fdd-4ece-a9c6-f593b177ba45)

Slack hold time satisfied:
![slack in the hold time is satisfied](https://github.com/user-attachments/assets/80ffeda9-d729-48ed-b1d2-5bb4e2398a64)

Slack setup time satisfied:
![salck in the setup time is satisfied](https://github.com/user-attachments/assets/7f0de065-f0dc-4520-9acc-d23a5de4d7a0)

Adding back the sky file:
![adding back the originall file](https://github.com/user-attachments/assets/b239e6bd-46f2-4ff2-82c6-4655d67e9b77)

####### __LAB 5: FINAL STEPS FOR RTL2GDS USING TRITONROUTE & OPENSTA__
Once CTS is ready we can generate a power distribution network (PDN) before routing. Let's check our current DEF file, using the following commands:

`echo $::env(CURRENT_DEF)`

use the PDN command:
`gen_pdn`

![gen pdn](https://github.com/user-attachments/assets/5d5b61fb-fcc7-4ba9-b52f-8c4bb86a69ee)

The PDN output above shows that PDN writes the LEF file, reads the CTS DEF and creates the grid and straps for the power and ground. As we know STDcells are placed in the std rows, therefore STDcell power rails are placed along the stdcell rows. The stdcell rails have a pitch of 2.720, equivalent to the height of the stdcell inverter. Thus the power and ground stdcell rails match with the GND and PWR ports of stdcell inverter.

The diagram below shows power planning.
<img src="https://private-user-images.githubusercontent.com/58273760/355400335-647fa47a-14af-49ac-b0ab-476a05bb59fe.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjQxNzQyODMsIm5iZiI6MTcyNDE3Mzk4MywicGF0aCI6Ii81ODI3Mzc2MC8zNTU0MDAzMzUtNjQ3ZmE0N2EtMTRhZi00OWFjLWIwYWItNDc2YTA1YmI1OWZlLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDA4MjAlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwODIwVDE3MTMwM1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTU2NjJmYzE4MzBjNDdmMjIyNDIwYjJjNTc0OTlhOTY2YjJjODViYWQ4YWYwNjc4ODJhMmQ4NTJjMmJhYjYzOGUmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.TR4Ttfi-q_OhUAsho4F2XQKHP1TVvDLRPzXhAae5bCE">

In the figure above, the green area corresponds to the picorv32a design. The red pads are for power, while the blue pads provide the ground connection.

From the pads, power is supplied to the rectangular close-loop rings. The vertical lines connected to the rings are power straps. The stdcells power and ground rails are attached to vertical straps. The height of the std cells must be multiple of the rail pitch to ensure proper power and ground connection. This image shows how the power comes from the outside to the pads, pads to the rings, rings to strap/stripe, and strap/stripe to stdcell rows.

__Global and detail routing and configure TritonRoute__
```
# Check value of 'CURRENT_DEF'
echo $::env(CURRENT_DEF)

# Check value of 'ROUTING_STRATEGY'
echo $::env(ROUTING_STRATEGY)

# Command for detailed route using TritonRoute
run_routing
Screenshots of routing run
```
![routing result](https://github.com/user-attachments/assets/03c783ee-318e-4cbc-8af6-25407826a2cc)

![final routing result](https://github.com/user-attachments/assets/885322d9-372e-4ceb-817d-34cee60cfb3c)

![routing done](https://github.com/user-attachments/assets/cff7e8d9-722b-46fd-87ee-d98473581867)

Commands to load routed def in magic in another terminal

```
# Change directory to path containing routed def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/05-08_13-22/results/routing/

# Command to load the routed def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &
```
![routing magic1](https://github.com/user-attachments/assets/78ed25be-81a3-4c69-b908-a9c61c8e1b71)

![routing magic 2](https://github.com/user-attachments/assets/ab68ff38-878e-4f3c-b519-f51f1a928925)

__References__

https://github.com/nickson-jose/vsdstdcelldesign

https://skywater-pdk.readthedocs.io/en/main/index.html


























