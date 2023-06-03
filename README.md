# PD-OpenLANE-Workshop
A detailed summary of Advanced-Physical-Design-using-OpenLane-SKY130 workshop organised by VSD  hands on performed by Archita Malgaonkar 

## Table of Contents 
  
 * [Day 1: Inception of open-source EDA, OpenLANE and Sky130 PDK](#day-1) 
     + [How to talk to computers](#how-to-talk-to-computers) 
     + [SoC Design and OpenLANE](#soc-design-and-openlane) 
     + [ASIC Design Flow](#ASIC-Design-Flow) 
     + [Getting Familiar to EDA tools](#getting-familiar-to-eda-tools) 
     + [Starting with OpenLANE and Synthesis](#starting-with-openLANE-and-synthesis) 
     + [Run Logic Synthesis in OpenLANE](#Run-Logic-Synthesis-in-OpenLANE) 
 * [Day 2: Good Floorplan vs bad Floorplan and Introduction to Library Cells](#day-2) 
     + [Stages of Floorplanning](#Stages-of-floorplanning) 
     + [Steps to run and view floorplan using OpenLANE](#steps-to-run-and-view-floorplan-using-openlane) 
     + [Placement in OpenLANE](#Placement-in-OpenLANE) 
     + [Cell Design Flow](#Cell-Design-Flow) 
     + [Characterization](#Characterization)  
 * [Day 3 - Design library cell using Magic Layout and ngspice characterization](#day-3) 
     + [Labs for CMOS inverter ngspice simulations](#Labs-for-CMOS-inverter-ngspice-simulations) 
     + [CMOS Inverter Design using Magic](#CMOS-Inverter-Design-using-Magic) 
     + [Characterizing the cell's(CMOS Inverter) slew rate and propagation delay](#Characterizing-the-cell's(CMOS-Inverter)-slew-rate-and-propagation-delay) 
 * [Day 4 - Pre-layout timing analysis and importance of good clock tree](#day-4) 
     + [Plug-in the Customized Inverter Cell(lif file) to OpenLane](#Plug-in-the-Customized-Inverter-Cell(lif-file)-to-OpenLane) 
     + [Delay Table](#Delay-Table) 
     + [Fix Negative Slack](Fix-Negative-Slack) 
     + [Floorplanning and Placement](#Floorplanning-and-Placement) 
     + [Pre-Layout Setup Timing Analysis](#Pre-Layout-Setup-Timing-Analysis) 
     + [Pre-Clock Tree Synthesis using TritonCTS](#Pre-Clock-Tree-Synthesis-using-TritonCTS) 
     + [Timing Analysis with Real Clocks](#Timing-Analysis-with-Real-Clocks) 
     + [Multi-corner STA for Post-CTS](#Multi-corner-STA-for-Post-CTS) 
 * [DAY 5: Final Steps for RTL2GDS using TritonRoute and OpenSTA](#Day-5) 
     + [Maze Routing](#Maze-Routing) 
     + [DRC Cleaning](#DRC-Cleaning) 
     + [Power Distribution Network (review)](#Power-Distribution-Network-(review)) 
     + [Routing Stage and TritonRoute](#Routing-Stage-and-TritonRoute) 
     + [Run PDN(Power Distribution Network)](#Run-PDN(Power-Distribution-Network)) 
     + [Routing Stage](#Routing-Stage) 
     + [Extraction of GRSII file](#Extraction-of-GRSII-file) 
 * [All commands to run in openlane](#All-commands-to-run-in-openlane) 
 * [Appendix](#Appendix) 
 * [References](#references) 
 * [Acknowledgement](#acknowledgement) 
 * [Accreditation](#Accreditation) 
 * [Inquiries](#inquiries) 
  
  
 ## Day 1: 
 ## Inception of open-source EDA, OpenLANE and Sky130 PDK 
  
 --- 
 ### How to talk to computers 
 --- 
 #### Introduction to QFN-48 package , cjip , pads,core,dir and IPs  
  
 This workshop we will be trying to talk more about the industry begind the chip highlighted which is the processor
 
 
 <p align="center"> 
     ![image](https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/73242877-3e8c-41a4-bf6c-0c99df7c021b)


  Along with the processors there are many other interfaces on the board like FLASH , EEPROM’s , GND and VCC pins,etc. Below is a typical image of a board. We are more focused about the chip design
  
  <p align="center"> 
     <img src="https://user-images.githubusercontent.com/120498080/214492904-1273ccaa-8c55-4fb5-9367-4b97f6a42b78.png"> 
    
    A typical IC can be visualized as the image shown below 
    
     <p align="center"> 
     <img src="https://user-images.githubusercontent.com/120498080/214492904-1273ccaa-8c55-4fb5-9367-4b97f6a42b78.png"> 
 - The exposed area of metal on a circuit board known as a *Pad* is where the component lead is soldered. Pads are used to send the signal inside the chip. 
 - A *Core* is a compact processor or CPU placed into a bigger processor or CPU socket. It is capable of handling all computing operations on its own. All the digital logic of the chip are placed in Core. 
 - The square of silicon that has been sliced off the wafer and contains an integrated circuit on the silicon wafer is called to as a *Die*. 
  
 <p align="center"> 
     <img src="https://user-images.githubusercontent.com/120498080/214492926-4321ab2c-5583-4c7d-9189-f7f1d4f447af.png"> 
  
 - *Foundry IP's* refers to all intellectual property (IP), whether Background IP or Foreground IP, developed for genetic components, pathways, and strains as well as methods and tools for design, genetic engineering, testing, and/or small-scale fermentation of microbial strains, regardless of when or for what purpose. 
 - *Macro-cells* are substantial building pieces that might be thought of as "black boxes." These macro-cells logic and electronic activity are described, but the internal structure may or may not be known. 
 - We can understand that *Macros* are like pure digital logic whereas *IP's* dose the intelegent work. 
 - Wo communicate to foundries through some interface files(PDKs) provided be the foundries. 
  
 #### RISC-V Instruction Set Architecture(ISA) 
 Along with the introduction of RISC-V, other well-known architectures like ARM and x86 are discussed. This is necessary since the architecture on which we will be working is a picorv32a CPU core based on RISC-V processor. 
 - For example if a C program is to be run on the hardware of given layout then the given C program is first compiled in its assembly language program (here it is the RISC-V assembly level program). Then this assemble level program is converted to machine level program which is the binary level program which is understood by the hardware of the computer and finally the bits get executed in the particular layout. 
 - There is another interface which need to present between RISC-V Architecture and layout is the *HARDWARE DESCRIPTION LANGUAGE (HDL)*. This HDL basically describe the harware and understabd the machine code(binary input). We need to implement this RISC-V specification using some RTL. So this RTL implement the RISC-V Architecture specification using the standard RTL to GDS flow. 
 - So the entire flow starts from the RISC-V Architecture, it has been implemented using an RTL and then finally implemented in layout. 
  
     <p align="center"> 
     <img src="https://user-images.githubusercontent.com/120498080/214389623-05de9e7c-75a3-45c4-aef8-0c50a7c2bc95.png"> 
  
 - The `.exe` file generated be the compiler contain the instruction whose syntax depend on the type of hardware we are using. This instruction set basically acts as an interface between the C level program and harware and this instruction set is called as *Instruction Set Architecture(ISA)* of the Architecture of the Computer(here a RISC-V Architecture). It is the language though which user speaks to the computer 
 - The assembler take this instructions and convert it into its respective machine language programe(binary language). 
 - We get the specifications of the instruction set and write a HDL of this instructions and synthesis it to gate level and then this is converted into its respective layout using the general RTL to GDSII flow. 
  
 ### SoC design and openLANE 
 --- 
 The 3 important factors in Digital ASIC Design process are,  
 1. *RTL Designs(Register Transfer Level) {*many RTL design is available in opensource like `librecores.org`,`openecores.org`,`github.com`} 
 2. *EDA Tools(Electronic Design Automation) {*many EDA tools are available like spice simulator, sis, magic, etc but now a days Qflow, openROAD, openLANE are used} 
 3. *PDKs Data*(Process Design Kits) {*Google + 130nm PDK is provided by skywater *} 
  
 The openLANE is a OPENSOURCE platform 
  
 ### ASIC Design Flow 
 The main objective of *ASIC Design Flow* is to take the design from RTL to GDSII tool which is the final format used for the final layouts. It as also called as *Automated PnR* or *Physical Implemantation* 
  
 #### RTL to GDSII flow 
 <p align="center"> 
     <img src="https://user-images.githubusercontent.com/120498080/214480028-4e5a5326-4998-4a2f-9248-f3337c3c562d.png"> 
  
 1.*Synthesis* 
 - Designed RTL is transilated into a circuit made out of components from SCL(Standard Cell Library). The resulatant curcuit is described in HDL and usually refered to as a gate level netlist. The gate level netlist is functionally equivalent to the RTL. 
 - The Standard Cells have regular layouts. Cell layouts are enclosed by fixed height rectangle and cell width is variable, its an integer multiple of the units called the site width. 
 - Each cells comes under differnet models/views utilized by different EDA tools like - Libraty View(includes electrical modles for cells such as delay and power modles), HDL View of the Cells,SPICE View of the Cells,Layout View of the Cells(GDSII view which is the detailed view and Delev view which is the abstract view) 
  
 2.*Floor and Power Planning* 
 - The objective here is the plan the silicon area and create the robous power distribution network to power the circuits. 
 - In Chip Floor-Planning the chip die is partition between different system building blocks and place the I/O Pads. 
 - In Macro Floor-Planning we define the micro dimensions and its pin locations and rows definitions. 
 - In power planning the power network is constructed using horizontal and vertical parallel metal straps(parallel structures are ment to reduce resistance and to address electromagnetic radiation problem) 
  
 3.*Placements* 
 - For macros we place the gate level netlist cells on wafer rows. 
 - Cell placements are done 2 steps:  
    + *Global Placement* (Global Placement tries to find the optimal positions for all the cells, such positions are not necessarly leagel, so cells may overlap of go off rows). 
    + *Detailed placements* (In Detailed Placement the positions obtained from global placements are minimally altered to be leagel). 
  
 4.*Clock Tree Synthesis (CTS)* 
 - It is the clock distribution network to deliver the clock to all sequential elements with minimum skew(the arrival of the clock at different component at differnet time) and minimum latency. The clock network looks like a tree where the clock source are the roots and the clock elements are the entities. 
  
 5.*Routing* 
 - After routing the clock comes signal routing. In given placement and fixed number of metal layers is require to find a valid pattern of horizontal and veritical wires to implement the nets that connects the cells together. The router uses the available metal layers as directed be the pdk, for each metal layer the pdk define the thickness, the pitch, the tracks and the minimum width. It also defines the wires that can be used to put nets, wire segments on different metal layers together. 
 - The skywater pdk defines extra routing layers, the lowest layer is called the local interconnect layer(its titenum nitride layer) and the following five layer on the local interconnect layer are all aluminum layers. 
 - Most of the routers are the grid routers, they construct the routing grids out of metal layer tracks. As the routing grids is hudge it use divide and conquer aproach is used for routing.  
    + First *Global Routing* is performed to generate the routing guids,  
    + then *Detail Routing* uses the fine grid and the routing guides to implement the actual wiring. 
  
 5.*Sign-off* 
 - Here we construct the final layout which undergo verification, which includes  
 *Physical Verification*  
     + *Design Rule Checking(DRC)* where we make sure that the final layout follows all design rules. 
     + *Layout vs Schematic(LVS)* which make sure that the final layout matches the gate level netlist then we started with  
 *Timing Verification*  
     + *Static Time Analysis(STA)* to make sure that all timing constrains are met and circiut will run at designated clock frequency. 
  
 #### Introduction to OpenLANE 
 For Open Source ASIC Flow we need to ba aware about the following in Open Source EDA
