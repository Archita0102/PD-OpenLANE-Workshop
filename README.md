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
  
 2.*Floor and Power Planning* 
 - Here the objective is to plan the silicon area and create robust power distribution. 
   1. Chip floor planning: Chip die is position between different chip positions.
   2. Macro floor planning: Defining the micro dimensions and its pin locations. Also the row definition
 
 - In power planning the power network is constructed typically a chip is powered by multiple Vdd , and power pins. The power pins are connected to horizontal and vertical metal straps parallelly . Parallel connection reduces the resistance and to address electromagnetic radiation. Power distribution network uses upper metal layers as they are thicker to lower metal layers and hence have low resistance.
  
 3.*Placements* 
 - Placing the gate level netlist on the vertical rows and must be placed close to eachother to reduce the interconnect delay and to enable successful routing . 
 - Cell placements are done 2 steps:  
   1.	Global placement: Global placement have to find optimum positions that may not be legal and hence cells may overlap on eachother.
   2. Detailed placement: The positions obtained here from global placement are minimally altered to be legal.
  
 4.*Clock Tree Synthesis (CTS)* 
 - Creating a clock distribution network before routing. Delivering clock to all sequential elements . 
 - Clock network is like tree where clock source is root and clock elements entity. Minimum skew and minimum latency . 
 - Skew means arrival of clock at different components at different times.
  
 5.*Routing* 
 - In given placement and fixed number of metal layers is require to find a valid pattern of horizontal and veritical wires to implement the nets that connects the cells together. 
 - The router uses the metal layers as defined by the pdk . pdk defines the thickness,pitch , track and minimum width.
 - The skywater pdk defines 6 routing layers. Thw lowest one is called local interconnect layer which is titanium nitride layer.The following five are aluminium layers. 
 - Most of the routers are grid routers they construct the routing grid out of the metal layer track . As the roouting layer is huge we go for divide and conquer approach.
    1. Global Routing: generates routing grids
    2. Detailed routing:Uses routing grids to implement actual wiring
  
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

