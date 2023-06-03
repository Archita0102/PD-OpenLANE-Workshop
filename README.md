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
 
 This workshop we will be trying to talk more about the industry behind the chip highlighted which is the processor
 
 <p align="center"> 
      <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/19af55b9-20b9-4721-93ca-926707d80d58"> 
 
  Along with the processors there are many other interfaces on the board like FLASH , EEPROM’s , GND and VCC pins,etc. Below is a typical image of a board. We are more focused about the chip design
  
  <p align="center"> 
     <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/a6806a98-28d5-487d-b11c-cd5f760ee250"> 
    
 
    
  
   
     
  A typical IC can be visaulized as shown in the figure. 
<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/fa5069d5-a1c9-4f28-a305-d02545cf0a51"> 
      
      
<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/429f301e-da75-42c8-ae45-69f5b2c95468"> 
      
     
   
 - The internal structure of the IC looks like shown in figure 1. This is package QFN-48. 
 - Chip is in the centre of the package and is connected to the other pins via wirebonds. This is how signal is tranferred from outside to the internal of the chip.
 - Figure 3 shows the chip internal circuit consisting of various components.
      
      Pads: send signal inside the chip
      
      Core: digital logic is placed here
      
      Die: size of the entire chip THAT GETS MANUFACTURED ON THE SILICON WAFER

<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/8b3b6139-2133-4ab8-a9aa-39bd9af9307d"> 
  

  
 - Typical core consists of SoC , SRAM , ADC,PLL, DAC, SPI. 
 - All of the above are called as foundry IPs.  All device performances are dependent on the foundry. IP is the intelligence needed to build up the blocks. 
 - Macros – SPI,SoC – it consists of pure digital logic.
  
  
 #### Introduction to RISC-V  
 Instruction Set Architecture – It is the language of the computer the way we talk with the computer. 
 <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/1ade684d-a2dd-4792-88e5-cf107c23ae44">     
         

 - Left image is a C program. To make the C program run on a particular layout it is first complied into an assembly language program , in this case it is RISC-V assembly language program 
 - This assembly language program is then converted into machine language program which is the binary language program consisting of 1s and 0s which is understood by hardware of the computer this is then executed in the layout and we get the required layout. 
 - Another interface that needs to be between the RISC V architecture and layout is the hardware description language. 
 - RTL implements the specifications of RISC-V architecture and from the RTL to layout is a standard RTL to GDSII flow. User only has to program in C other flow will be automatically done by the system
   
 #### From Software Applications to Hardware 
  
<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/0edc84e8-b0aa-4ec8-8d15-2175bb4da865"> 
       
     
 - Application software enters into a block of system software and the system software converts the application program into binary language.
 - The major components of system software are the operating system (OS) , the compiler and the assembler.
 - OS(Operating system) : Handle IO operations , allocate memory and low level system functions. Alongwith it also converts the application into its respective assembly language or binary language program.
 - The output of OS is C,C++ etc and then its complied and converted into instructions . The syntax of instructions depends upon the hardware language. Intructions language is into .exe.file.
 - Assembler converts the instructions into binary/machine language program. This is further fed to the hardware. 
 - Instructions act as an abstract interface between the C program and the hardware. This interface is the instruction set architecture.
  
 <p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/cfdd404a-c934-4444-897c-23dbe700da8b">
   
 - To reach from the abstract interface to the hardware the another interface os the hardware description language to implement the specification of the instructions which is synthesized into a netlist in form of gates and then we have a physical design of the netlist. 
   
 <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/d58a47c2-7d04-4dfb-9a82-f32c98301288">
   
 So the three main parts are:
   1.	RISC –V Instruction Set architecture
   2.	RTL and synthesis of RISC-V based CPU core picorv32a
   3.	Physical design implementation of picorv32a


 --- 
 ### SoC Design and OpenLANE
 --- 
   
 #### Introduction to all components of open source digital ASIC design
   
  
 Designing digital ASIC requires several elements and all of them are to be present always
   
 <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/73e31598-8ccd-488d-b035-518785df9c50">
   
  

   
 1. *RTL Designs(Register Transfer Level):* There are many open source RTL designs present some of them are librecores.org , opencores.org , github.com. 
 2. *EDA Tools(Electronic Design Automation):* Spicesimulatot, sis ,magic . Nowadays we have academic tools like Qflow , OpenROAD, OpenLANE. 
 3. *PDKs Data*(Process Design Kits):* It is a collection of files used to model a fabrication process for EDA tools to design an IC. Skywater 130 nm + Google pdk is opensource PDK.
   
 <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/36d0a908-dba8-4d41-a254-b29ee252377f">
   
 
#### Simplified RTL to GDSII flow
  
 The main objective of ASIC design is to take the design from RTL to GDSII
   
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/bdce5052-f645-4cab-9042-29b017caf8de">
  
 
  
 1.*Synthesis* 
 - The RTL design is converted into a circuit out of components from the standard cell library.
 - Result is the gate level netlist that is functional equivalent to an RTL.  
 - Standard cells have a fixed regular layout of fixed height and variable width. 
 - Each cells have different models  Libraty View(includes electrical modles for cells such as delay and power modles), HDL View of the Cells,SPICE View of the Cells,Layout View of the Cells(GDSII view which is the detailed view and Delev view which is the abstract view)

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/94f7de8a-a187-4fc6-b62e-a110f493c3e5">
  
  

 2.*Floor and Power Planning* 
 - Here the objective is to plan the silicon area and create robust power distribution. 
   1. Chip floor planning: Chip die is position between different chip positions.
   2. Macro floor planning: Defining the micro dimensions and its pin locations. Also the row definition
 
 - In power planning the power network is constructed typically a chip is powered by multiple Vdd , and power pins. The power pins are connected to horizontal and vertical metal straps parallelly . Parallel connection reduces the resistance and to address electromagnetic radiation. Power distribution network uses upper metal layers as they are thicker to lower metal layers and hence have low resistance.

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/91474b18-7b40-4695-8ec5-2c1849288173">
  
 
 3.*Placements* 
 - Placing the gate level netlist on the vertical rows and must be placed close to eachother to reduce the interconnect delay and to enable successful routing . 
 - Cell placements are done 2 steps:  
   1.	Global placement: Global placement have to find optimum positions that may not be legal and hence cells may overlap on eachother.
   2. Detailed placement: The positions obtained here from global placement are minimally altered to be legal.
  
 <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/0e76f216-3943-45ee-adb3-74fa8d77006c">
   
   
  

 4.*Clock Tree Synthesis (CTS)* 
 - Creating a clock distribution network before routing. Delivering clock to all sequential elements . 
 - Clock network is like tree where clock source is root and clock elements entity. Minimum skew and minimum latency . 
 - Skew means arrival of clock at different components at different times.
  
  <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/cd9c8f8b-736c-446a-89d9-fd6383891455">
    
    

 5.*Routing* 
 - In given placement and fixed number of metal layers is require to find a valid pattern of horizontal and veritical wires to implement the nets that connects the cells together. 
 - The router uses the metal layers as defined by the pdk . pdk defines the thickness,pitch , track and minimum width.
 - The skywater pdk defines 6 routing layers. Thw lowest one is called local interconnect layer which is titanium nitride layer.The following five are aluminium layers. 
 - Most of the routers are grid routers they construct the routing grid out of the metal layer track . As the roouting layer is huge we go for divide and conquer approach.
    1. Global Routing: generates routing grids
    2. Detailed routing:Uses routing grids to implement actual wiring
    
 <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/a4cc3d32-a78d-4eb2-9374-203ac10accf3">
   

  
 5.*Sign-off* 
 - Now we can construct a final layout after routing which undergoes  
     1.Physical verifications:
        a. 	Design Rules checking: It makes sure the final layout undergoes all design rules. 
        b.  Layout vs Schematic: Makes sure that final layout matches the gate level netlist.
     2.Timing Verifications:
        a.  Static Timing analysis: makes sure that all timing constraints are met at a designated clock frequency.

  
 #### Introduction to OpenLANE and strive objects 
 Using OenSOURCE tools the problem is tougher. We need to take care about
  a.	Tools Qualification
  b.	Tools Calibaration
  c.	Missing tools

OPENLANE is OpenSOURCE and free and can be used for our purpose. Openlane is started as a Open Source flow for a true Open Source tape out experiment 
Efabless has a family of SoCs striVe where there is openpdk,openeda,openrtl.

 <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/90544e14-46f6-42b0-8052-108b1a33a79e">
   
 This family has several members
 
<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/5fa2ec14-57d8-4fe5-b011-4ccad5101eb5">
    
- The main goal for Openlane is to produce a clean GDSII without human intervention. Clean means no LVS violations , no DRC violations , no timing violations.
- Openlane is tuned for Skywater 130nm open pdk also supports XFAB180 and GF130g
- It is containerized which means functional out of box ,and instructions to build and run natively.
- Openlane can be used to harden macros and chips in the final layout
    a.	Autonomous : Push button flow. Configure the flow , push button wait for some time based on the design size and we get GDSII TOOL. 
    b.	Interactive : We can run commands and steps one by one.
  
* Open lane has a nice feature of design space exploration to find best sets of flow configurations .
* OpenLane currently has 43 different designs will be using picorv32a 

 #### Introduction to OpenLANE detailes ASIC flow 
   
 <p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/5fa2ec14-57d8-4fe5-b011-4ccad5101eb5">
   
-	The flow is transferred from the design RTL to the final GDSII layout , to function it needs the pdk.
   
 <p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/5fa2ec14-57d8-4fe5-b011-4ccad5101eb5">
   
-	Openlane is based on several opensource projects.
-	The RTL is fed to Yosys with design constraints . Yosys translated the RTL into logic circuits and later the design is mapped into standard cell library using STL the abc.
-	Abc has to be guided during the optimization and this guidance comes in the format of abc script referred as synthesis strategis. We have strategis that target the least area and other strategis that target the best timing
-	Different designs using different to achieve the objectives for that we have the synthesis exploration utility that can be used to generate reports that shows how the design and area is affected by synthesis translation
-	Based on the exploration we can pick the best strategy to continue with.
-	Openlane also has design exploration utility which can be used to sweep design configurations  and generate a report.the report shows different design matrix also shows number of violations generated.
-	This is useful to find the best configurations for openlane for any given design.
-	Design Explorations can also be used for regression testing for continues integration{OpenLANE  currently run the exploration in 70 designs and compare the results to the best knonw one}
-	If we want our design to be ready of testing after fabrication we enable DFT(Design for Test) step(which is optional) and this used open source project "Fault" to perform scan insertion ,automatic test pattern generation , test patterns compaction , fault coverage , fault simulation.
   
-	DFT(Design for Test) also add the data controller which enables the external access to the internal structure.
-	Now comes the physical implementation that consists of several steps and all are done by openroad app. We used openroad app for
	 - Floor/Power Planning
   - End Decoupling Capacitors and Tap cells insertion
   - Placement: Global and Detailed
   - Post placement optimization
   - Clock Tree Synthesis (CTS)
   - Routing: Global and Detailed
  
-	To perform optimization which enforce subtransformation of the gate level netlist generated by synthesis step we need to perform logic equivalence checking which can be done using yosys.
-	We compare the netlist from optimisation to the gate level netlist from the synthesis to make sure  they are functionally equivalent.
-	If not equivalent then the desired result will not be obtained.
-	During the Physical Implemantation we have a special step Fake antenna diodes Insertion Script which is required to address the antenna rules violation.
-	When a metal wire is fabricated it can act as antenna. Reactive ion etching causes charge to accumulate on wire . Transistor gates can be damages during fabrication.
-	This is the job of the router . if the router fails there are 2 solutions
   -	Bridging attaches a high layer intermediary
   -	Add antenna  diode cell to leak away the charges.
-	With openlane we take a preventive approach 
   -	Created a Fake Antenna Diode Cell add to  SCL next to every cell input after placement.
   -	Run the magic antenna checker 
   -	If checker reports violation on the cell input pin, replace fake diode cell with real diode one
-	Openlane has a configuration that allows the designer to select one of the approaches to handle antenna violations i.e either the OpenLane approach or the openRoad approach.
-	Sign off involves static timing analysis , design rule checking and layout vs schematic 
-	Finally Sign-off of OpenLANE involves interconnects RC Extraction from the routed layout followed by STA(Static Timing Analysis) using "OpenSTA" which is on "OpenROAD". The result of this step is set of timing reports highiliting timing violations if any.
-	Physical Verification involves DRC performed using Magic VLSI layout tool and LVS involve using Magic for several extraction followed by Nitgen to perform the comparision.
-	Magic involves Design rule checking and spice extraction from layout. Magic and netgen are used for LVS.


 --- 
 ### Get Familiar to open source EDA tools
 --- 
   
   
   
 ## Day 2: 
 ## Good floorplan vs bad floorplan and introduction to library cells
  
 --- 
 ### Chip floor planning considerations 
 --- 
 #### Utilization factor and aspect ratio
   
<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/86a0b01a-6151-4ead-94f4-a7027eedf613">

Consider a basic netlist of 2 flip flops :- Launch flop(Left one) and the capture flop(right one) and a simple combinational logic between. 
  
<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/d74ca6af-9344-4741-ad37-e138f8b13684">
  
 - And,OR gates :- Standard Cells         
 - Flip Flops:- Latches/Registers
  
-	Netlist defines connectivitiy between all the components.
-	Dependent more on the dimensions of the logic gates/Standard cells: therefore try giving a proper length and breadth to the gates.

 <p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/ff5ab0c8-ff5f-4b94-bbc5-4e6b6126ac32">
 
- To identify the dimensions of the chips we should be knowing the dimensions of the gates excluding the wires.
-	Considering the Standard cells and Flip flops having dimensions of 1unit we get an area of 1sq unit.
-	Bring the gates and flip flops together : the total area we get is 4sq units. This is the minimum area of the netlist.
   
 <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/2b2fe937-f263-454a-b4d4-d6079e29662d">
 
Core:Section of the chip where the fundamental logic of design is placed
Die:Consists of core , a small semiconductor material specimen on which the fundamental circuit is fabricated
   
<p align="center"> 
    <img src="(https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/0fcd986c-3906-4164-9154-949e710604fa">


   
Placing all the logic cells inside the core . The logical cell occupies all the area of the core which specifies the utilization is 100%

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/ad29d0dc-386b-41b5-a5b9-5f29d3b06e47">

*Utilization factor = Area occupied by netlist / Total Area of core*  = (4 x 1sq unit)/ (2sq unit x 2sq unit)=1=100% i.e extra cells cannot be added in this core.
- Ideally the utilization factor is 50-60%
  
*Aspect ratio= Height/Width* = 2unit/2unit = 1 signifies  chip is square . If other than 1 specifies chip is rectangular

 #### Concept of pre placed cells
  
  
*Preplaced cells:* Combinational logic undergoes a simple digital logic .The output of combinational logic can consist of huge gates. We can divide the circuit logic for further simplification  and can be separated into two different blocks. The individual blocks can be made to work when needed. Both the blocks can be implemented seperately.
  
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/23634c20-9468-45fd-b0be-274f9f71aa6e">
  
When they are put into a black box and the wires are detached the circuitry inside the block in not visible for the user.

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/27030d52-d181-4b9a-a6ed-cd7897f49c15">

- Similarly, there are some other IP’s available like comparators, MUXs, etc
-	The arrangement of IPs in the chip is called floorplanning.
-	The IPs have user-defined positions and hence are placed in chip before automated placement and routing and are called as pre placed cells.
  
  #### Decoupling Capacitors
  
  The preplaced cells can be be placed into the chip area according to the design scenarios. These locations cannot be changed once placed.
  
  <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/6f96ada2-99bb-4339-b262-b72dc5e2ac86">
    
-	We have to surround the preplaced cells with decoupling capacitors.
-	Consider the circuit below to be a part of any block. Whenever a circuit switches there is a amount of current it needs. There is a small capacitor and when the transition happens the capacitor has to completely charge or discharge. The amount of charge will be sent by the supply voltage. 
-	When voltage flows through the wires there is a drop because of the resistance. Hence the voltage reaching the circuit is less than the supply.
-	The voltage reaching the circuit must be within the noise margin range.
-	If it's not within the noise margin range then unwanted transitions between logic 0 and logic 1 can take place.

 <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/34127557-f5a5-481f-b369-267a0f168836">
   
- This problem can be solved by decoupling capacitors. Huge capacitor filled with charge . the voltage across it is similar to that of power supply. The circuit gets the current from the decoupling capacitor.
-	The issue of voltage drop is resolved.
   
 <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/bc4601a9-d8e3-417d-83d2-4daa7adfd6c1">
   
  <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/bc4601a9-d8e3-417d-83d2-4daa7adfd6c1">
    
  
  #### Power planning
    
- Consider a circuit consisiting of driver and load connected with the power supplies, where we want that the signal coming from the driver should reach the load in the similar fashion without any transistion.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/bc4601a9-d8e3-417d-83d2-4daa7adfd6c1">
  
- Assume the red path is 16 bit bus and is charged from logic 0 to logic 1
- For load to retain the same signal it should get the necessary supply from the power because we don’t have any decoupling capacitor. Hence , power supply has to provide the voltage.
- It is not a good idea to attach decoupling capacitors to the entire chip. For some blocks its alright.
- As the supply line and the bus line are placed far away there will be voltage drop occurring. There cn be issues of ground bounce and voltage drop.
- Power was coming from single source instead if multiple power supplies can be given the issue can be solved as the circuit takes the current from the nearest power supply.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/bc4601a9-d8e3-417d-83d2-4daa7adfd6c1">

 #### Pin placement and logical cell placement blockage
 
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/bc4601a9-d8e3-417d-83d2-4daa7adfd6c1"> 
  
-	The connectivity information between the gates is coded using VHDL/Verilog language and is called a netlist.
-	Placing all of the circuits in the core and die.
-	All the i/p pins on right and o/p pins on left preferably
  
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/bc4601a9-d8e3-417d-83d2-4daa7adfd6c1">
  
-	Ordering of i/p and o/p pins is random.
-	Clock ports are bigger in size compared to data ports as they are driving all the cells continuously.
-	Bigger the size lower is the resistance.
-	Other area should be blocked for placement and routing.

 
--- 
 ### Library binding and placement 
 --- 
 #### Netlist binding and initial place design
  
- Bind netlist with physical cells : There are no shapes defined for gates . In reality they are contained in boxes. Giving the cells physical dimesnions. Each component in the netlist is been given a proper width and height.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/bc4601a9-d8e3-417d-83d2-4daa7adfd6c1">
  
- These blocks are present in library. Library also has all timing information . 
Library consists of also shapes and sizes. Bigger in size least resistance path and they can be faster.
- Placement : Placing the shapes and sized inside a floorplan. The physical views are to be placed on the floorplan  with the help of connectivity seen inside a netlist.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/bc4601a9-d8e3-417d-83d2-4daa7adfd6c1">
  
- Place the netlist close to the i/p blocks in the floorplan so that the logical connectivity is maintained.

 #### Optimize placement using estimated wire length and capacitance
  
-	Correctly optimizing the placement of the blocks inside the floorplan.
-	The problem of distance is called optimize placement : this  is the step where we estimate wire length and capacitance and based on that insert repeaters.
-	Repeaters are buffers that recondition the original signal make a new signal and replicate the original signal and send it again.
-	Hence signal integrity is maintained. But , in this there is loss of area.
-	Based on the estimation of wire length required there will be capacitance calculation and upon that waveform is created and the transistion of waveform should be in persimmable range.
-	Slew is dependent on the value of capacitor. Higher the value of capacitor the amount of charge required to charge the capacitor will be high and the skew will be high.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/bc4601a9-d8e3-417d-83d2-4daa7adfd6c1">
  
 
 #### Final Placement Optimization
  
-	Abatment reduce the time delay between the logic.
-	High speed logic requires such types of abatment so that there is no time delay.
-	Based on setup time analysis with ideal clocks will know the placement done is reasonable or not

   #### Need for libraries anc characterization
  
- PART1 : Concepts and Theory – NLDM , CCS Timing , power  and noise characterization: If we have a functionality that is coded in RTL the first step is converting it into logic synthesis. Output  of logic synthesis is gates that represents your original functionality. 
-	Next step is floorplanning. We import the netlist that we get out of logic synthesis and decide the size of core and die.
-	Next step is Placement: Logic cells are placed on the chip in a fashion that initial timing is met.
-	Next is CTS : if you want to have 0 skew and clock spread across the logic cells at equal time.
-	Next Routing :  Routing cells dependent on the characteristics of the digital cells used within.
-	Static Timing Analysis: Try to see what is setup, hold times an max achievable frequency of the circuit. Last step of IC design flow.
-	Common among all the steps is the gates or cells where library characterization is important , the collection of gates is referred to as library. 
-	To make understand the EDA tool what the gate is and timing characteristics modelling of gates is important.
  
 
--- 
 ### Cell design and characterization flow 
 --- 

 #### Inputs for cell design flow
  
-	The buffers, flip flops, gates are all called as standard cells.
-	Standard cells are placed into library.
-	Library has got cells of different functionality , different sizes and different threshold voltages .

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/ff3c8bb9-f395-4051-9f55-b359f2c85ea8">

-	Variation of threshold voltages define the speed.
-	For a IC design flow the component has to be derived in the form of size,shape,power,drive strength and so on.
-	Cell design flow is divided into three parts
    -	Input: Inputs needed for the component. Consists of PDKs, DRC and LVS rules , SPICE models ,library and user defined specs.
        a.	DRC and LVS rules: Dimensions of active area.
        b.	SPICE models : Drain current and threshold voltage equations.
        c.	Library and user defined specs: Cell height defined by power and ground rail.Cell width is dependent on the timing information. A typical component has to operate on a typical supply voltage taking care of the noise margin levels . the Cells to be built upon the particular metals layers . Pin location is defined by library developer. Pins should be near the power or ground rail.


 #### Circuit , layout and typical characterization flow
  
-	Design steps: Designing the component. Developer takes the input and comes with the library cell that adheres to the inputs. 
  -	Circuit design:Typically involves involves implementing the function and model the transistors to meet the library requirements. Output we get from here is a circuit description language.
  -	Layout design:a. Implementing the values into a layout.
                  b. Get Function Implement by the transistors
                  c. Get PMOS  and NMOS network graph out of the design. Euler’s path and stick diagram.
                  d. Obtain Euler’s path: traced only once.
                  e. Convert stick diagram into layout athering to the DRC rules and user defined specifications.
                  f. Hand drawn layout into a tool (eg magic tool i.e opensource tool).
                  g. Extract parasitic and characterize it in terms of timing.
  -	Characterization:Timing , noise and power information . 
-	Output : Output of the component actually used by the EDA tool. CDL , GDSII , LEF , extracted SPICE netlist
 
*Characterization:*
Layout , description and spice extracted netlist of the component. There is sub circuits consisting of transistor definations.
1.	Read the model files
2.	Read the extracted spice netlist.
3.	Recognize behaviour of buffer.
4.	Read the subcircuit of inverter
5.	Attach the necessary power sources.
6.	Apply stimulus.
7.	Provide necessary output capacitance.
8.	Provide necessary simulation command.(DC or transisition)
9.	Feeding all of the above the inputs from config file into characterization software called GUNA. This will generate timing noise.
a.	Timing characterization
b.	Power characterization
c.	Noise characterization


--- 
 ### General Timing Characterization parameters
 --- 

 #### Timing threshold definations
  
-	Understanding different threshold  points of a waveform.
    -	Slew_low_rise_thr: close to 0 power supply . Toc calculate the slew we need two different points. Typical value is 20%.
    -	Slew_high_rise_thr: 20% point from top power supply .
    -	Slew_low/high_fall_thr: Similar like the above.
-	We need the above values to calculate the skews of the waveform.
-	In the transient analysis definations related to the input in the waveform: in_rise_thr,in_fall_thr,out_rise_thr,out_fall_thr- thresholds for the delays. To calculate delays of the particular inverters. 50% values.

  
#### Propogation delay and transistion time
  
- Propogation delay: time out threshold-time in-threshold = delay of the buffer.
- Transistion time: time(slew_high_rise_thr)-time(slew_low_rise_thr) or time(slew_high_fall_thr)-time(slew_low_fall_thr)

  
  
 
  ## Day 3: 
 ## Design library cell using Magic Layout and ngspice characterization
  
 --- 
 ### CMOS Inverter ngspice Simulations 
 --- 
 #### Spice deck creation for CMOS inverter
  
  
 #### Spice simulation lab for CMOS inverter
  
  
  
  #### Switching threshold Vm
  
  
  
  #### Static and dynamic simulation of CMOS inverter
  
  
  
  --- 
 ### Inception of layout of CMOS fabrication process
 --- 
  
 #### Create active regions
  
  
  
 #### Formation on N-well and P-well
  
  
 #### Formation of gate terminal
  
  
 #### Lightly doped drain (LDD) formation
  
  
 #### Source and drain formation
  
  
 #### Local interconnect formation
  
  
  
#### Higher level metal formation
  
  

 
  

   ## Day 4: 
 ## Pre layout timing analysis and importance of good clock tree
  
 --- 
 ### Timing modelling using delay tables
 --- 
 #### Spice deck creation for CMOS inverter
