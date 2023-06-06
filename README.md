# PD-OpenLANE-Workshop
A detailed summary of Advanced-Physical-Design-using-OpenLane-SKY130 workshop organised by VSD and hands on performed by Archita Malgaonkar 

## Table of Contents 
  
 * [Day 1: Inception of open-source EDA, OpenLANE and Sky130 PDK](#day-1) 
     + [How to talk to computers](#how-to-talk-to-computers) 
     + [SoC Design and OpenLANE](#soc-design-and-openlane) 
     + [LAB1](#LAB1) 
 * [Day 2: Good Floorplan vs bad Floorplan and Introduction to Library Cells](#day-2) 
     + [Chip floor planning consideration](#Chip-floor-planning-consideration) 
     + [Library binding and placement](#Library-binding-and-placement) 
     + [Cell design and characterization flow](#Cell-design-and-characterization-flow) 
     + [General timing characterization parameters](#General-timing-characterization-parameters) 
     + [LAB2](#LAB2)       
 * [Day 3 - Design library cell using Magic Layout and ngspice characterization](#day-3) 
     + [CMOS Inverter ngspice Simulation](#CMOS-Inverter-ngspice-simulation) 
     + [LAB3](#LAB3) 
 * [Day 4 - Pre-layout timing analysis and importance of good clock tree](#day-4) 
     + [Timing modelling using delay tables](#Timing-modelling-using-delay-tables) 
     + [Timing analysis with ideal clocks using openSTA](#Timing-analysis-with-ideal-clocks-using-openSTA) 
     + [CLock tree synthesis using Triton-route and signal integrity](#CLock-tree-synthesis-using-Triton-route-and-signal-integrity) 
     + [Timing analysis with real clocks using openSTA](#Floorplanning-and-Placement) 
     + [LAB4](#LAB4) 
 * [DAY 5: RTL to GDSII](#Day-5) 
     + [Routing and design rule check](#Routing-and-design-rule-check) 
     + [Power Distribution Network](#Power-Distribution-Network) 
     + [LAB5](#LAB5) 
 * [Acknowledgement](#acknowledgement) 
 
  
  
 ## Day 1: 
 ## Inception of open-source EDA, OpenLANE and Sky130 PDK 
  
 --- 
 ### How to talk to computers 
 --- 
 #### Introduction to QFN-48 package , chip , pads,core,dir and IPs  
 
 This workshop we will be trying to talk more about the industry behind the chip which is the processor
 
 <p align="center"> 
      <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/b154a5a6-dc07-487c-884b-1156cc5c2ff7"> 
	
	
 
  Along with the processors there are many other interfaces on the board like FLASH , EEPROM’s , GND and VCC pins,etc. Below is a typical image of a board. We are more focused about the chip design
  
  <p align="center"> 
     <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/0b3717b7-e2a1-4a86-907a-933b6e9148df"> 


  A typical IC can be visaulized as shown in the figure. 
<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/d99594e1-4592-450d-b2a0-ecfb55d853a3"> 
     

      
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/8b4d499f-0f4c-4af0-a434-626c1be76f47"> 
      
  
   
   
 - The internal structure of the IC looks like shown in figure 1. This is package QFN-48. 
 - Chip is in the centre of the package and is connected to the other pins via wirebonds. This is how signal is tranferred from outside to the internal of the chip.
 - Figure 3 shows the chip internal circuit consisting of various components.
      
      
```python
	
      Pads: send signal inside the chip
      
      Core: digital logic is placed here
      
      Die: size of the entire chip that gets manufactured
	
````
	
<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/f8d33a05-00ce-46a8-87ae-a3085b0c70f0"> 
  


  
 - Typical core consists of SoC , SRAM , ADC,PLL, DAC, SPI. 
 - All of the above are called as **foundry IPs**.  All device performances are dependent on the foundry. IP is the intelligence needed to build up the blocks. 
 - **Macros** (SPI,SoC) – it consists of pure digital logic.
  
  
 #### Introduction to RISC-V  
 Instruction Set Architecture – It is the language of the computer the way we talk with the computer. 
 <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/12c59ff8-97db-4339-aba5-f201013bdaab">     
         


 - Left image is a C program. To make the C program run on a particular layout it is first complied into an assembly language program , in this case it is RISC-V assembly language program 
 - This assembly language program is then converted into machine language program which is the binary language program consisting of 1s and 0s which is understood by hardware of the computer this is then executed in the layout and we get the required layout. 
 - Another interface that needs to be between the RISC V architecture and layout is the hardware description language. 
 - RTL implements the specifications of RISC-V architecture and from the RTL to layout is a standard RTL to GDSII flow. User only has to program in C other flow will be automatically done by the system
   
 #### From Software Applications to Hardware 
  
<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/3b539599-4ee5-4f03-a063-50c29786fa21"> 

       
     
 - Application software enters into a block of system software and the system software converts the application program into binary language.
 - The major components of system software are the operating system (OS) , the compiler and the assembler.
 - OS(Operating system) : Handle IO operations , allocate memory and low level system functions. Alongwith it also converts the application into its respective assembly language or binary language program.
 - The output of OS is C,C++ etc and then its complied and converted into instructions . The syntax of instructions depends upon the hardware language. Intructions language is into .exe.file.
 - Assembler converts the instructions into binary/machine language program. This is further fed to the hardware. 
 - Instructions act as an abstract interface between the C program and the hardware. This interface is the instruction set architecture.
  
 <p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/bcd4af9a-5b38-41f3-bd88-cc882426fd6a">
  
 - To reach from the abstract interface to the hardware the another interface os the hardware description language to implement the specification of the instructions which is synthesized into a netlist in form of gates and then we have a physical design of the netlist. 
   
 <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/c011bf39-e870-4bac-b2fa-184bcedbb8a9">
 

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
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/6b761b2a-1e0a-43ed-8df5-7f89d398201b">
   
 

   
 1. **RTL Designs(Register Transfer Level):** There are many open source RTL designs present some of them are librecores.org , opencores.org , github.com. 
 2. **EDA Tools(Electronic Design Automation):** Spicesimulatot, sis ,magic . Nowadays we have academic tools like Qflow , OpenROAD, OpenLANE. 
 3. **PDKs Data*(Process Design Kits):** It is a collection of files used to model a fabrication process for EDA tools to design an IC. Skywater 130 nm + Google pdk is opensource PDK.
   
 <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/7d0b478f-6b99-4df8-9419-cee28494d11d">
   


#### Simplified RTL to GDSII flow
  
 The main objective of ASIC design is to take the design from RTL to GDSII
   
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/50f64d14-5466-4c98-a42c-0b127966e001">
  


  
 1.**Synthesis** 
 - The RTL design is converted into a circuit out of components from the standard cell library.
 - Result is the gate level netlist that is functional equivalent to an RTL.  
 - Standard cells have a fixed regular layout of fixed height and variable width. 
 - Each cells have different models  Libraty View(includes electrical modles for cells such as delay and power modles), HDL View of the Cells,SPICE View of the Cells,Layout View of the Cells(GDSII view which is the detailed view and Delev view which is the abstract view)

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/fd73f2e7-fdef-44a7-9e75-c8e8cd1ca055">
  
 


 2.**Floor and Power Planning** 
 - Here the objective is to plan the silicon area and create robust power distribution. 
   1. Chip floor planning: Chip die is position between different chip positions.
   2. Macro floor planning: Defining the micro dimensions and its pin locations. Also the row definition
 
 - In power planning the power network is constructed typically a chip is powered by multiple Vdd , and power pins. The power pins are connected to horizontal and vertical metal straps parallelly . Parallel connection reduces the resistance and to address electromagnetic radiation. Power distribution network uses upper metal layers as they are thicker to lower metal layers and hence have low resistance.


 
 3.**Placements** 
 - Placing the gate level netlist on the vertical rows and must be placed close to eachother to reduce the interconnect delay and to enable successful routing . 
 - Cell placements are done 2 steps:  
   1.	Global placement: Global placement have to find optimum positions that may not be legal and hence cells may overlap on eachother.
   2. Detailed placement: The positions obtained here from global placement are minimally altered to be legal.
  
 <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/998641fe-8139-4c94-8407-13d1d1ef5adb">
   

  

 4.**Clock Tree Synthesis (CTS)** 
 - Creating a clock distribution network before routing. Delivering clock to all sequential elements . 
 - Clock network is like tree where clock source is root and clock elements entity. Minimum skew and minimum latency . 
 - Skew means arrival of clock at different components at different times.
  
  <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/29576aa1-d1c9-4cce-9b3b-d7329a09f028">
    

  

 5.**Routing** 
 - In given placement and fixed number of metal layers is require to find a valid pattern of horizontal and veritical wires to implement the nets that connects the cells together. 
 - The router uses the metal layers as defined by the pdk . pdk defines the thickness,pitch , track and minimum width.
 - The skywater pdk defines 6 routing layers. Thw lowest one is called local interconnect layer which is titanium nitride layer.The following five are aluminium layers. 
 - Most of the routers are grid routers they construct the routing grid out of the metal layer track . As the roouting layer is huge we go for divide and conquer approach.
    1. Global Routing: generates routing grids
    2. Detailed routing:Uses routing grids to implement actual wiring
    
 <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/d39462ac-7fd5-4733-95ff-977c1b6ff9f2">
   


  
 5.**Sign-off** 
 - Now we can construct a final layout after routing which undergoes  
     1.Physical verifications:
        a. 	Design Rules checking: It makes sure the final layout undergoes all design rules. 
        b.  Layout vs Schematic: Makes sure that final layout matches the gate level netlist.
     2.Timing Verifications:
        a.  Static Timing analysis: makes sure that all timing constraints are met at a designated clock frequency.

  
 #### Introduction to OpenLANE and strive objects 
 Using OpenSOURCE tools the problem is tougher. We need to take care about
  a.	Tools Qualification
  b.	Tools Calibaration
  c.	Missing tools

OPENLANE is OpenSOURCE and free and can be used for our purpose. Openlane is started as a Open Source flow for a true Open Source tape out experiment 
Efabless has a family of SoCs striVe where there is openpdk,openeda,openrtl.

 <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/915cbdcc-6077-4000-b770-c807a4b35c81">

  
 This family has several members
 
<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/68489b5f-046c-4a60-b389-3e6e9a59425d">

   
- The main goal for Openlane is to produce a clean GDSII without human intervention. Clean means no LVS violations , no DRC violations , no timing violations.
- Openlane is tuned for Skywater 130nm open pdk also supports XFAB180 and GF130g
- It is containerized which means functional out of box ,and instructions to build and run natively.
- Openlane can be used to harden macros and chips in the final layout
    a.	Autonomous : Push button flow. Configure the flow , push button wait for some time based on the design size and we get GDSII TOOL. 
    b.	Interactive : We can run commands and steps one by one.
  
* Open lane has a nice feature of design space exploration to find best sets of flow configurations .
* OpenLane currently has 43 different designs will be using picorv32a 

 #### Introduction to OpenLANE detailed ASIC flow 
   
 <p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/41f5c323-97f9-4922-aeb1-ee3a9e65f62f">
  

-	The flow is transferred from the design RTL to the final GDSII layout , to function it needs the pdk.
   
 
   
-	Openlane is based on several opensource projects.
-	The RTL is fed to Yosys with design constraints . Yosys translated the RTL into logic circuits and later the design is mapped into standard cell library using STL the abc.
-	Abc has to be guided during the optimization and this guidance comes in the format of abc script referred as synthesis strategis. We have strategis that target the least area and other strategis that target the best timing
-	Different designs using different to achieve the objectives for that we have the synthesis exploration utility that can be used to generate reports that shows how the design and area is affected by synthesis translation
-	Based on the exploration we can pick the best strategy to continue with.
-	Openlane also has design exploration utility which can be used to sweep design configurations  and generate a report.the report shows different design matrix also shows number of violations generated.
-	This is useful to find the best configurations for openlane for any given design.
-	Design Explorations can also be used for regression testing for continues integration{OpenLANE  currently run the exploration in 70 designs and compare the results to the best knonw one}
-	If we want our design to be ready of testing after fabrication we enable DFT(Design for Test) step(which is optional) and this used open source project "Fault" to perform scan insertion ,automatic test pattern generation , test patterns compaction , fault coverage , fault simulation.
   
-	***DFT(Design for Test)*** also add the data controller which enables the external access to the internal structure.
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
 ### LAB1
 --- 
	 
### Get familiar to open source EDA tools
	 
 #### OpenLANE directory structure in detail
	 
-	OpenLANE is a flow conisiting of open source EDA tools like yosys , OpenSTA. Basic requirement for OpenLANE was to have complete RTL to GDS flow and avoid human intervention.
-	Putting the RTL netlist and foundary pdks what we obtain from the flow is the GDSII file. OpenLANE is very similar to commercial EDA tool.
-	In this workshop we are working on OpenLANE 
-	**PDKs:** Process design kit consists of all the information related to pdk. The pdk used in the workshop is skywater 130nm pdk. OpenLANE is built around this pdk. The pdk folder consists of the following files
	 
<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/fed0d90c-5700-430f-aa03-18d0324ae6ab">	 
	
- Some commands above.	
	-   `Skywater-pdk` : has all pdk related files eg, timing libraries, lef files,tech lef,etc.
	-   `open_pdks` : Silicon foundary files are made to work with the commercial EDA tools and not open source. Open_pdks help in mitigating the problem . There are sets of scripts and files that convert these foudnary level pdks to be compatible with open source tools.
	-   `sky130A` :pdk which is made compatible to work with open source environment. It has libs.tech and libs.ref subdirectories.
			1.	libs.ref: All process/ technology related files like timing , cell lef,tech lef , etc. We will be woking on the pdk variant ***sky130_fd_sc_hd***.		
	-   `sky-process name   fd-foundary  sc-standard cell  hd-high density`
	
<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/9c1e4211-7dde-43b7-b8cc-72c57c098b09">		 
	 	
	 		2.	`libs.tech`: Files specific to tools. 
 <p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/33dd1944-ccd1-4163-9816-dbfdc01f2e60">	 
 
	

 #### Design preparation step

-	Some of the commands in terminal prompt of linux:-
	-	`cd` : change directory
	-	`ls` : list of the contents inside a folder
	-	`ls –ltr` : listing everything inside a folder in a chronological order
	-	`<command name>:` –-help: specifies about the command
	-	`./flow.tcl:`  Specifies the flow has to go according to the script
	-	`–interactive:` step by step process
	-	`package require openlane 0.9` :import packages required to run the flow
	-	`cmds`: all the commands that we have run.
	-	`cp` : copy one folder into another directory.
	-	`vim` : to make changes to any file in the terminal.

*We will be working in OpenLANE*

<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/229fc177-832f-4be5-b4dd-21de3b76fbcb">	 

	

-	The designs required to run on the OpenLANE are been extracted from the `designs` directory under `openlane`. There are about 30-40 designs already built in openlane , here we can also built our design.	
	
<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/4b6d663a-e620-40a1-8d2f-130f5f13d6b6">	


- For this workshop will be doing **picorv32a**. The folder consists of following subdirectories.
	-   `src` : source. RTL  and SDC information will be present here.
	-   `sky130A_sky130_fd_sc_hd_config.tcl` : pdk specific config file.
	-    `config.tcl` : Bypassess all the configuration that has already been done in openLANE . Many switches use default values that are already present in openLANE flow.

<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/69901dd9-03da-4962-ba62-e2104ed3ff86">	
	
` less config.tcl `
<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/3d0ee9f8-bab5-417a-b742-ec5640601f96">	
	
-	**set filename** : Tells if there exists a file source that file . So when we run our custom design the sky130 file wont be there but , we have to create a config.tcl file.
-	 The precedence
	3	Default value set in openLANE
	2	config.tcl
	1	sky130A_sky130_fd_sc_hd_config.tcl – highest precedence
-	Before synthesis it is desirable to prepare the design setup that would set data structure for the design. It is necessary to set the file system specific to the flow
	-	Command :` prep –design picorv32a `
	-	`mergeLef.py : merge the cell lef and tech lef files`

<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/e12bf36b-b263-4aab-bd97-77e96bada0ae">	
	

 #### Review files after design prep and run synthesis
-	Check if anything new created in design library. *Runs* directory is created with the current working date.
-	`config.tcl:` Tells about the default parameters that will be taken by the run
<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/96008773-99c0-48a1-b0b3-1af4c5d63469">

*merged.lef files:*
1. `tech.lef` 	
	
<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/00727df0-8357-41cc-99b5-eb580a4ce940">

	
2. `cell.lef`
	
<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/13436f9f-18bb-447f-bbeb-1e5b5660b714">

	

-	One good thing about openLANE is that changes can be made on the flight . Making changes in the original file the number will be updated on the config.tcl file
-	Running the synthesis : `run_synthesis`. This will run the abc and yosys synthesis.


<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/adb9e2c4-02d1-4349-9e17-9ff1537f926e">	
<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/ad312c01-3644-4c28-bc42-384d6dae4c4c">	


#### Steps to characterize synthesized results
-	```python
	Calculation of flop ratio: No of d flip flops to the total number of cell
	```
	
<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/40cfbbeb-def1-416c-8eb0-4b992f286ce8">	
	

<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/cbc76488-68f8-4e95-a36e-d7f12e799c6d">	
	

```python
	Flop count = 0.1084 = 10.84%
```
-	Check the `results` and `reports` in the `runs` folder. Check under **synthesis**.

<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/9d78edc9-ee5f-47ea-bf85-4253febc2c5a">	
<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/ffd1d876-e4c5-4943-81db-7e6a12e86ffa">	




	
 ## Day 2: 
 ## Good floorplan vs bad floorplan and introduction to library cells
  
 --- 
 ### Chip floor planning considerations 
 --- 
 #### Utilization factor and aspect ratio
   
<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/69dd80d7-2d22-481c-9004-cebfd9ceb547">

Consider a basic netlist of 2 flip flops :- Launch flop(Left one) and the capture flop(right one) and a simple combinational logic between. 
  
<p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/354482ef-f3e1-418d-b988-22d657abac6a">
  

 - And,OR gates :- Standard Cells         
 - Flip Flops:- Latches/Registers
  
-	Netlist defines connectivitiy between all the components.
-	Dependent more on the dimensions of the logic gates/Standard cells: therefore try giving a proper length and breadth to the gates.

 <p align="center"> 
   <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/e6eff31a-e3dc-49cb-8708-7e706146c4b2">


- To identify the dimensions of the chips we should be knowing the dimensions of the gates excluding the wires.
-	Considering the Standard cells and Flip flops having dimensions of 1unit we get an area of 1sq unit.
-	Bring the gates and flip flops together : the total area we get is 4sq units. This is the minimum area of the netlist.
   
 <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/501465c3-4729-46ad-a8d3-a11784543f6e">


**Core:** Section of the chip where the fundamental logic of design is placed
	 
**Die:** Consists of core , a small semiconductor material specimen on which the fundamental circuit is fabricated
   
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/ce558824-0a4a-4bd3-9c2e-3a7913dc0198">

   
Placing all the logic cells inside the core . The logical cell occupies all the area of the core which specifies the utilization is 100%

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/6f46e61b-1d07-4f9c-81fc-6a104763532c">

```python
	Utilization factor = Area occupied by netlist / Total Area of core  = (4 x 1sq unit)/ (2sq unit x 2sq unit)=1=100% i.e extra cells cannot be added in this core.
- Ideally the utilization factor is 50-60%
  
Aspect ratio= Height/Width = 2unit/2unit = 1 signifies  chip is square . If other than 1 specifies chip is rectangular
```

 #### Concept of pre placed cells
  
  
**Preplaced cells:** Combinational logic undergoes a simple digital logic .The output of combinational logic can consist of huge gates. We can divide the circuit logic for further simplification  and can be separated into two different blocks. The individual blocks can be made to work when needed. Both the blocks can be implemented seperately.
  
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/0689a587-e8b3-4d08-9419-64023e9d0c2a">

  
When they are put into a black box and the wires are detached the circuitry inside the block in not visible for the user.

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/d0eb6055-09be-4219-84df-5f60c35343b4">
	

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/1fbe1e03-dac0-44bf-9a98-35836a78f328">


- Similarly, there are some other IP’s available like comparators, MUXs, etc
-	The arrangement of IPs in the chip is called floorplanning.
-	The IPs have user-defined positions and hence are placed in chip before automated placement and routing and are called as pre placed cells.
  
  #### Decoupling Capacitors
  
  The preplaced cells can be be placed into the chip area according to the design scenarios. These locations cannot be changed once placed.
  
  <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/1227ade5-a6e8-4ffa-bc1d-77ba49a56195">
  
-	We have to surround the preplaced cells with decoupling capacitors.
-	Consider the circuit below to be a part of any block. Whenever a circuit switches there is a amount of current it needs. There is a small capacitor and when the transition happens the capacitor has to completely charge or discharge. The amount of charge will be sent by the supply voltage. 
-	When voltage flows through the wires there is a drop because of the resistance. Hence the voltage reaching the circuit is less than the supply.
-	The voltage reaching the circuit must be within the noise margin range.
-	If it's not within the noise margin range then unwanted transitions between logic 0 and logic 1 can take place.

 <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/0dbeeb19-87f3-41b8-8c7a-ac3b4b479342">
  
 
- This problem can be solved by decoupling capacitors. Huge capacitor filled with charge . the voltage across it is similar to that of power supply. The circuit gets the current from the decoupling capacitor.
-	The issue of voltage drop is resolved.
   
 <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/06665f16-4ab6-4915-8375-be6bb8d3c905">
 
 
  <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/e6596c37-00cb-48ff-828f-1369fe2e3d79">
    
 

  #### Power planning
    
- Consider a circuit consisiting of driver and load connected with the power supplies, where we want that the signal coming from the driver should reach the load in the similar fashion without any transistion.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/2aa9c57a-0287-45a3-ba36-15b89a9be21e">


- Assume the red path is 16 bit bus and is charged from logic 0 to logic 1
- For load to retain the same signal it should get the necessary supply from the power because we don’t have any decoupling capacitor. Hence , power supply has to provide the voltage.
- It is not a good idea to attach decoupling capacitors to the entire chip. For some blocks its alright.
- As the supply line and the bus line are placed far away there will be voltage drop occurring. There cn be issues of ground bounce and voltage drop.
- Power was coming from single source instead if multiple power supplies can be given the issue can be solved as the circuit takes the current from the nearest power supply.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/522fc467-387b-476f-b452-db4be8974437">


 #### Pin placement and logical cell placement blockage
 
Consider a design
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/98a3a65e-f7d3-4dd6-bb46-1ce438d7f7bd"> 


-	The connectivity information between the gates is coded using VHDL/Verilog language and is called a netlist.
-	Placing all of the circuits in the core and die.
-	All the i/p pins on right and o/p pins on left preferably
  
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/b0270ada-3a42-4202-b8e3-39d479938db3">

  
-	Ordering of i/p and o/p pins is random.
-	Clock ports are bigger in size compared to data ports as they are driving all the cells continuously.
-	Bigger the size lower is the resistance.
-	Other area should be blocked for placement and routing.

 
--- 
 ### Library binding and placement 
 --- 
 #### Netlist binding and initial place design
  
- Bind netlist with physical cells : There are no shapes defined for gates . In reality they are contained in boxes. Giving the cells physical dimesnions. Each component in the netlist is been given a proper width and height.


 
- These blocks are present in library. Library also has all timing information . 
Library consists of also shapes and sizes. Bigger in size least resistance path and they can be faster.
- Placement : Placing the shapes and sized inside a floorplan. The physical views are to be placed on the floorplan  with the help of connectivity seen inside a netlist.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/1f49cab0-c499-4b3d-acbb-607299052706">
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/1f49cab0-c499-4b3d-acbb-607299052706">

  
- Place the netlist close to the i/p blocks in the floorplan so that the logical connectivity is maintained.

 #### Optimize placement using estimated wire length and capacitance
  
-	Correctly optimizing the placement of the blocks inside the floorplan.
-	The problem of distance is called optimize placement : this  is the step where we estimate wire length and capacitance and based on that insert repeaters.
-	Repeaters are buffers that recondition the original signal make a new signal and replicate the original signal and send it again.
-	Hence signal integrity is maintained. But , in this there is loss of area.
-	Based on the estimation of wire length required there will be capacitance calculation and upon that waveform is created and the transistion of waveform should be in persimmable range.
-	Slew is dependent on the value of capacitor. Higher the value of capacitor the amount of charge required to charge the capacitor will be high and the skew will be high.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/b70c592c-0174-4066-85af-9616b9495416">
  


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
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/4acbe8ee-61de-40ef-a00d-e3bf77a750cd">


-	Variation of threshold voltages define the speed.
-	For a IC design flow the component has to be derived in the form of size,shape,power,drive strength and so on.
-	Cell design flow is divided into three parts
    -	Input: Inputs needed for the component. Consists of PDKs, DRC and LVS rules , SPICE models ,library and user defined specs.
        a.	DRC and LVS rules: Dimensions of active area.
        b.	SPICE models : Drain current and threshold voltage equations.
        c.	Library and user defined specs: Cell height defined by power and ground rail.Cell width is dependent on the timing information. A typical component has to operate on a typical supply voltage taking care of the noise margin levels . the Cells to be built upon the particular metals layers . Pin location is defined by library developer. Pins should be near the power or ground rail.


 #### Circuit , layout and typical characterization flow
 <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/e5930ca7-0f3e-4904-8f2f-5d983f52c198">
	 
	

-	**Design steps:** Designing the component. Developer takes the input and comes with the library cell that adheres to the inputs. 
  -	**Circuit design:** Typically involves involves implementing the function and model the transistors to meet the library requirements. Output we get from here is a circuit description language.
  -	**Layout design:**
	 
	 	  a. Implementing the values into a layout.
	 
                  b. Get Function Implement by the transistors.
	 
                  c. Get PMOS  and NMOS network graph out of the design. Euler’s path and stick diagram.
	 
                  d. Obtain Euler’s path: traced only once.
	 
                  e. Convert stick diagram into layout athering to the DRC rules and user defined specifications.
	 
                  f. Hand drawn layout into a tool (eg magic tool i.e opensource tool).
	 
                  g. Extract parasitic and characterize it in terms of timing.
	 
  -	**Characterization:** Timing , noise and power information . 
-	**Output:**  Output of the component actually used by the EDA tool. CDL , GDSII , LEF , extracted SPICE netlist
 
*Characterization:*
Layout , description and spice extracted netlist of the component. There is sub circuits consisting of transistor definations.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/a0bd88a2-fe89-4f3e-9936-044b6745fa76">
	


1.	Read the model files
2.	Read the extracted spice netlist.
3.	Recognize behaviour of buffer.
4.	Read the subcircuit of inverter
5.	Attach the necessary power sources.
6.	Apply stimulus.
7.	Provide necessary output capacitance.
8.	Provide necessary simulation command.(DC or transisition)
9.	Feeding all of the above the inputs from config file into characterization software called **GUNA**. This will generate timing noise.
a.	Timing characterization
b.	Power characterization
c.	Noise characterization
 <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/e92aba05-560f-4ca6-a062-14904ee30840">



--- 
 ### General Timing Characterization parameters
 --- 

 #### Timing threshold definations
  
-	Understanding different threshold  points of a waveform.
    -	`Slew_low_rise_thr`: close to 0 power supply . Toc calculate the slew we need two different points. Typical value is 20%.
    -	`Slew_high_rise_thr`: 20% point from top power supply .
    -	`Slew_low/high_fall_thr`: Similar like the above.
-	We need the above values to calculate the skews of the waveform.
-	In the transient analysis definations related to the input in the waveform: in_rise_thr,in_fall_thr,out_rise_thr,out_fall_thr- thresholds for the delays. To calculate delays of the particular inverters. 50% values.

 <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/a3163ed9-ca9e-4621-b2b2-aacf661ed92a"> 
	 
	
#### Propogation delay and transistion time
  
- **Propogation delay:** time out threshold-time in-threshold = delay of the buffer.
- **Transistion time:** time(slew_high_rise_thr)-time(slew_low_rise_thr) or time(slew_high_fall_thr)-time(slew_low_fall_thr)

  --- 
 ### LAB2
 --- 
	 
 ###  Floorplan and PLacement steps
 #### Steps to run floorplan using OpenLANE

 
-	Synthesis run has been successful and now we are going for floorplan.
-	In floorplan we set the die area, the core area , aspect ratio , utilization factor , place input output cells, power distribution network, macro placement.
-	Standard cells are not placed in floorplan.
-	OpenLANE has many switched which address the floor direction.
-	To know about the switches go in the configuration folder of openlane.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/9e1d1cb6-8331-41f4-b6fd-c7e9da46586b">
	

-	Open the `README.md` file . There are variables(switches) required for each stage.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/67979df1-c794-45a3-b083-bd472fd02c6b">

	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/5667fd27-189c-48e6-9fff-a67499351dc8">	
	
	

-	We can set the parameters according to the flow. Because it might happen that we require certain parameters at certain time hence, we can keep the others undisturbed.
-	*Target density* ` (PL_TARGET_DENSITY) ` states how close or how spread you want ypur cells to be . 1-closely packed 0-widely spread.. Preferable is to set it to 0 as we have to d conjestion analysis and timing constraints are to be met.
-	Open the **floorplan.tcl** in configuration , you will see the parameters that are default set in openlane.

	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/aef6dacc-0bca-4bff-825d-a04e368c7323">	
	
` (FP_IO_MODE):` Tells about how we want the pin configuration. 1- randomly but equidistant  0-not equidistant
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/aef6dacc-0bca-4bff-825d-a04e368c7323">
	
	
-	Precedance of the settings is
	- `floorplan.tcl`  - lowest
	- `config.tcl` the horizontal and vertical metal layers here are one more than in the
        - `sky130A sky130_fd_sc_hd_config.tcl` – highest

	
-	Finally run the floorplan **run_floorplan**
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/49b7e9c3-19fe-489d-a5fd-7b158c2a40c0">
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/7506558a-5266-4079-bfff-780968c08578">	


	
 #### Review floorplan files and steps to review floorplan files

-	Check whether the switches in config.tcl take precedence over system defaults.
-	Config.tcl file inside the runs directory tells about all the conifiguration taken by the floor.
-	To check the values of HMetal/VMetal go into the floorplan directory inside logs in runs. 
-	Go in ` -ioPlacer.log `.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/5c6d6ef5-7e77-4615-9997-25b434b5d4cc">

- 	Values are overwritten by pdk `config.tcl`
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/6aec93bd-b638-4678-8c52-e8c421669821">	
	

-	To see how floorplan looks like go in results , floorplan. A **def**(design exchange format) file will be present. Opening the def file	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/238d6577-1ce8-4c7c-ba98-7bc33cc4dd70">

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/83293eee-1c98-4e1b-a0fd-b443bbf63219">
	

	
```python
	Die area(Area of whole die) : (lower left x value    lower left y value)(upper right x value    upper right y value)
Unit is set above (displayed in the image)  . Therefore , the database is unit/micron.
```
	

	
#### Review floorplan layout in magic
	
- To look at the actual layout after program. `magic –T` <directory of the tech file>.**T** for tech file. 
- `lef` file will be taking here is the merged.lef file.
- **&** - free ups the prompt when magic launches	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/478d89a6-6715-4b49-a066-6b18e9a7c997">


	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/ac23f1f3-367c-46f0-8b01-ee1b8fa5d6b0">	
	
-	Select the entire layout by pressing `s` and then to centre align press `v`.
-	To zoom into a particular portion of the layout  press left mouse button then the right and then click `z`.
-	To select a particular cell , hover your pointer on the cell and press `s` – *the cell or pin will be selected*.
-	We had set the input output pins to 1 which tells they are equidistant.

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/616d74b7-7165-4d2d-bdbe-0c27c0e2aac4">



-	Go on the `tkcon` window and enter what. It shows the layer on which the pin is in.
-	Similarly this can be done for vertical pins.

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/463c19de-7873-4d3a-9e9f-29126097c543">

-	There are **d cap** cells arranged along the side row
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/f5f2ab65-f95a-4afe-a40a-461f03573abc">	

	
-	There are tap cells meant to avoid latch conditions in the CMOS devices like preventing substrate to the ground.They are diagonally equidistant which was already stated in the `README` 
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/04ec2204-8cc5-494f-8b5c-b821d3f0ee7f">
	
	

-	There are standard cells present at the bottom.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/a71d777d-5015-4078-a75e-cb580f2582db">


	
	
#### Congestion aware placement using RePlAce	
- Post floorplan is the placement stage that happens in 2 stages:
	1. Global Placement: Forced placement. No legalizations. Legalization means standard cells should be exactly under the standard cell rows without any overlaps.Reduces the wire length.
	2. Detailed Placement:
	
- `run_floorplan`
	
- We have to converge the overflow.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/67c6167c-1939-4f91-9aaa-96c7fae70845">
	

-  To see the design post placement . Go into placement folder of results. A def file will be created.
-  Enter the command for magic view.

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/c8da775b-9d29-4ff4-8ea7-04b49cbdb6a4">		

- Magic view of the placement 
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/dbab0231-6953-4f5a-8a89-89dace764312">	
	
- These many are the standard cells at the initial stage of floorplan.
- Placement of standard cells in standard cell rows.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/220c9a40-5540-4d49-bec3-8b411a7aa60">	


	
  ## Day 3: 
 ## Design library cell using Magic Layout and ngspice characterization
  
 --- 
 ### CMOS Inverter ngspice Simulations 
 --- 
 #### Spice deck creation for CMOS inverter
 **SPICE deck :** Is a connectivity information about the netlist having all the input informations , tap points for output.
- Creating a `Spice` desk for CMOS inverter

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/e8657ccd-5d57-46f1-8c09-58f7e74d6b0f">	
	
- Spice Deck should consist information about the substrate as well. Substrate pin tunes the voltage of PMOS and NMOS.
- cLoad value is assumed to be 10fF.
- Define the W/L values of PMOS and NMOS. Ideally PMOS should be twice or thrice the NMOS.
- Later define the voltage values og gate and drain.
- Identify the nodes.
- Naming the nodes
	
Writing the spice deck	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/d8503160-f049-4242-a38b-aded1a3b3441">	
<tran drain gate source substrate pmos W L>
<tran drain gate source substrate nmos W L>
	
 #### Spice simulation lab for CMOS inverter
 
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/71449ae3-7c9e-4238-9a56-443f5ab54deb">

***<compomenent node 1 node2 value>***

- Simulation commands
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/444c07fb-f568-4e7d-b355-a1feed8f761b">
 
Sweeping gate input voltage ` <start value    end value    step> `	

- Describing model file

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/49b0b4e0-4427-4d1f-991a-6bc20829d3a8"> 

All parameters related to the technology node are described here.


  
  #### Switching threshold Vm
  
- CMOS inverter is a robust device and is hence used for building logic gates.

*Switching Threshold (Vm):* a. Point at which the device switches. 
		       	b. Vin=Vout.
			c. Both PMOS and NMOS are in saturation region. hence, there will be leakage current.Therefore, current flows directly from power to ground.
 
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/73118af1-80d6-4612-82cc-cdeab62522af">  
  
  
  --- 
 ### Inception of layout of CMOS fabrication process
 --- 
  

  
 1. Selecting a substrate: GDSII from physical layout substrate gets actually fabricated on the substrate. Most common is p-type substate with a good resisitivty, doping and orientation.
 2. Creating active regions for the transistors.Creating pocket like structures on ths substrate which would be connected on the metal layers.
	a. Creating Isolation within the pockets by growing a insulator (SiO2) , then depositing a layer os silicon nitride (Si3N4) , deposition of photoresisit
  	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/8705a5eb-3dc3-4cd2-9fc6-83f43971e496"> 
	
	-  Layouts are called as masks. Masking the device using photolithography and creating wells.
	-  Removing the mask.
	-  Etching the Si3N4 and photoresist.
	-  Placing into furnance creating oxidation of Silicon.
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/fc7fa9b8-7ab8-4e35-99a3-3fb6bc259aef">	

  3. N-well and P-well Formation
	- Masking and Photolithography.
	- Ion implantation is carried to make the subsequent wells (Boron for p-well and phosphorous for n-well).
	- Diffuing the wells using furnance.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/f662a9c9-8f1d-4659-ab37-3136399e725c">
  
 4. Formation of gate
	- Oxide capacitence and doping concentration impacts the threshold voltage.
	- Create doping inside p-type and n-type transistors with less energy.
	- Oxide was etched and later regrown as due to masking damages are occured in the oxide layer.
	- Depositing polysiliocn layer for gate terminal and for low resistance dope it with more impurities.
	- Again carry photolithography for creating contacts.
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/507a76bb-0776-416c-bda9-30050b96be39">
	

5.  Lightly doped drain (LDD) formation
	-For PMOS fabrication we need P+,P-,N. Source and drain are P+.P- is the LDD and N-N-well.
	-For NMOS fabrication we need N+,N-,P. Source and drain are N+.N- is the LDD and P-P-well.
	- P- and N- is needed for short channel effect and hot electron effect.
	- Spacers help in keeping LDD intact while constructing source and drain.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/5252362f-dbcc-479e-8c6d-7433f888239a">	
	

  
6.  Source and drain formation
	- Thin screen oxide to avoid channeling during implants.
	- Annealing the device.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/854b9793-82f5-4f60-a9fa-c02830626bc0">	
	

7. Local interconnect formation
	- Etching oxide using HF.
	- Depositing titanium on wafer using sputtering.
	- Deciding the contacts needed
 <p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/2254299f-6a25-47af-9b05-098e5552ccf3"> 


8. Higher level metal formation
	 - Surface is non-planar. To planarize deposit SiO2 doped with phosphorous and boron , later do polishing.
	 - Mask and deposit TiN as it is a good barrier layer followed by deposition of tungstes to form good contacts.
	 - Making interconnects outside using Al and W.

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/900b2877-9d2b-4d12-ad7a-dfba72ffa5bb"> 
	
  --- 
 ### LAB3
 --- 
	
###  Labs for CMOS inverter ngspice simulations
 #### IO placer revision
- Here we will not be building the cell from scratch, we downloaded a mag file from the github link.
- From ` .mag` file will be doing post layout simulation in ngspice and post characterizing the sample cell.
- Will be opening the cell in picorv32a.
	
- OpenLANE alows changes on the flight. For this :
	1.There are 4 stragtegies placed by the IOPlacer(opensource EDA tool used to place IOs in the core) 
- To change the settings , go to floorplan.tcl where the default settings are wriiten , copy tha setting you want to change and set it to a new value as following.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/0b7fedc5-a177-45d8-b3b6-7f45ccccac3c"> 
	
-  Run the floorplan again and check if the changes are been appended.

#### Gitclone vsdcelldesign
	
- Copy the url from the github link : https://github.com/nickson-jose/vsdstdcelldesign.git
- In the terminal under openlane foler type `git clone <url>`.
- This created vsdstdcelldesign folder in the openlane.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/c38a645b-2559-4e8f-9e5b-596994836cf8"> 
	
-  Will be doing spice extractions and post layout spice simulations.
-  Copy the tech file in vsdcelldesign folder

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/c5d9e425-e62e-4431-a1ed-d724e6ee5c03">

- To view the layout in magic plane
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/5cc00e7e-c8c2-4e41-a6bf-c834bcdc4adf">
	

- We get the inverter layout	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/7daeafe7-49cb-497f-9862-bb0ba7c5a917">	
	
#### Introduction to Sky130 basic layers layout and lef using inverter

- Layers for basic CMOS inverter are visible on the magic layout.
- Right side of the magic window are the colour palettes these are the layers. The first layer is interconnecr layer. Metal layers are also shown in different colours.
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/d6f56676-ad4a-4cdd-a622-a2cadf2f922a">
	
-  The green in the centre is the n-diffusion region which is identified by the help of the colour palette.Similarly , brown is the p-diffusion region.
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/393d6847-98ae-419c-8357-7a22ff6ab584">	

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/da93bc9f-88ac-4a74-a643-baa54c04262e">
	
-  When poly crosses n diffusion - NMOS and when poly crosses p diffusion -PMOS. On layout you can verify by sending a command on tkcon window.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/e39a704d-25f1-48d8-a8c4-dfa3ac7d0b96">

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/68999377-1581-461c-91b9-b4b06c0529ae">

- Check if drain of PMOS and NMOS are connected. For this press s twice on the layout window.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/4933d089-cd4d-4ed4-96d9-e0a69b1ebfa6">
	
Similarly other connections can also be checked.
	
	
	
#### Create standard cell layout and extract spice netlist

- We have to take care of the *DRC* rules been validates. Checking out the metal layers.
- To extract the layout on spice , open the `tkcon` folder to get the location of our design.
- To create extraction file, commands in the tkcon window are as follows:
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/f96128aa-66e0-449e-971e-0590095fae46">
	
-  Check if the file is obtained inside the `vsdstdcelldesign` folder.
-  Use the ext file to create a spice file to be used in the `ngspice` tool.
-  Type following commands in terminal . the `cthresh 0 rthresh 0 extract` all parasitic capacitors.

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/b3aed3a4-2f45-453b-8dac-268ac78d39df">	
	
-  Spice file is created
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/b5eb4b13-857a-4cda-8971-cf3ea806e3c2">


	
#### Steps to create final Spice deck using Sky130 tech
- We need to check the dimensions.
- It should be the grid value specified in the layout.
- Dimensions of the box is:
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/bb45cfe7-d170-4fd5-8849-61bcae1080b7">

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/1293ce75-538a-47f0-b275-fe1eb4c10d6b">

#### Steps to characterize inverter using Sky140 model files

- To display the values taken by the ngspice 
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/faf6f030-d72d-407c-82d1-3c809e8b0175">

- To plot: plotting output vs time sweping the input

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/cd0c20dd-7d53-46de-bb1d-15f288bff0ab">	

- To zoom into a particular region in graph just make use of right ,ouse button and a box will be formes.
	
```python
Rise Time [output transition time from 20%(0.66V) to 80%(2.64V)]:
Rise Time = 2.25013ns - 2.18412ns = 0.06601ns ns

Fall Time [ouput transition time from 80%(2.64V) to 20%(0.66V)]:
Fall Time = 4.09365ns - 4.05066ns = 0.04299 ns	
	
Rise Delay [delay between 50%(1.65V) of input to 50%(1.65V) of output]:
Rise Delay = 2.21566ns- 2.15181ns = 0.02554 ns
	
Fall Delay [delay between 50%(1.65V) of input to 50%(1.65V) of output]:
Fall Delay = 4.07574ns- 4.05016ns = 0.06385 ns
	
```

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/77b0e0a6-2a84-4337-8700-3681f5738af6">
	
-  The characterization done above was done at 27°C.
-  We have characterized our inverter. Next objective is to create a .lef file using the layout to be used in openlane and plug the cell into picorv32a code.
	
 ## Day 4:
## Pre-layout timing analysis and importance of good clock tree
  
 --- 
 ### Timing modelling using delay tables
 --- 
 #### Introduction to Delay tables

Problem:

- The capacitance or the load at the output node of each and every buffer in the complete clock tree is varying.
- If the load is varying the input transition is varying.
- There will be variety of delays.
- To avoid large skew between endpoints of a clock tree (happening due to signal arrives at different point in time):

After splitting the buffers.
Buffers on the same level must have same capacitive load to ensure same timing delay or latency on the same level. It means that each buffer at the same level is having same load.
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/a23f5c01-5da4-4f1c-ac84-e93acef9fb5">

Solution was to bring *Delay tables*
	
- 2D table 
- With varying input transistons and output load the delay table was characterized .
- The timing model of each cell is recorded and is summarised in delay tables, which are part of the liberty file. The output slew is the main cause of delay.


#### Delay tables usage
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/65c0f21e-35a7-4ebc-8a7b-9c1cc426cd6c">
	
-  Fill up the value of input transistion , fill up the value of output load and get corresponding delay.
-  Capacitive load and input slew are also factors that affect output slew. The input slew has its own transition delay table and is a function of the previous buffer's output cap load and input slew.


	
--- 
 ### Timing analysis with ideal clocks using openSTA
 --- 
 #### Setup timing analysis and introduction to flip-flop setup time
 
*Setup Time Analysis:*
	
-  One edge of the clock is sent to launch flop and the other is sent to the capture glop.
-  The combinational delay should be less than the time period. If combinational delay is more the frequency reduces and the clock is shifted.
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/1a5dab3b-6c1a-4dc5-bfbc-48d52bfd4a36">	
	
-   Inside the flop there are combinational circuits with their own time delays.
-   Consider there are multiplexers at both the flops.
-   The delays through multiplexers restrict the combinational delay requirement.
=   Hence , capture clock requires a finite amount of time to settle. So this amount of time has to be seperated from the complete clock period
-   Setup time:	Time required for the capture flop to settle and display the output .

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/600ccb47-e264-4125-88bd-19b331923ac6">


 #### Introduction to clock jitter and uncertainity	
	
- *Jitter:* Clock signal has to be sent at regular time intervals of 0.T.2T... but ,due to wires and other delays so we dont receive a clock edge at a defined time period. Clock has its own variations. So the signal might gel delayed or arrive early. This temporary variation of clock is called to be jitter.
	
-  Clubbing the uncertainty of the clock now the time period has to be less than the previous setup + uncertainty time.
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/0caac8b9-3a75-46d1-bf75-8ddf42d167bb">
	
--- 
 ### Clock tree synthesis using Triton-route and signal integrity
 --- 
 #### Clock tree routing and buffering

-  Time required for clock to reach flip flop 1 is t1 and to reach flip flop 2 is t2.
-   Difference between both the time is skew. Skew should be as minimum as possible.
	
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/fed8cb39-dd31-4727-bac8-c2aeb38f0347">
	
-  We wont get zero skew using this route. 
-  Edge Tree: Takes a particular clock. Calculates the distance till the flip flops and build a tree. this reduces the time difference between them.
	
-  This clock tree has some wires that carry resistance within them.Hence, there will be many ytransistions on the path and we wont get the signal at the output that we want to obtain. So we add repeaters. The repeaters will have similar rise and fall time.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/2ae02f50-c672-4bf9-b8ae-728b6bc3e119">
		
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/24e50019-1613-409a-a947-49782c44f7f7">



 #### Crosstalk and clock net shielding
	
-  Clock nets are critical as we have built clock tree with zero skew.
-  But , there might be chances of crosstalk that may detoriate the singal.
-  Clock nets are shielded hence , they are protected from the outside connections.
-  There can be two problems of glich and delta delay.	
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/144ee52f-646e-4817-8f51-9bc71c1c5605">

L1 and L2 are latency.
- Because of crosstalk there will be some delay. Due to some gltich there will be some delta delay caused.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/3d409bb5-cd55-4ea1-aa85-37d47506a5db">	
	
	

	
--- 
 ### Timing analysis with real clocks using open STA
 --- 
 #### Setup and Hold Timing analysis
	
-  The clock tree has now buffers and wires.Now, the buffer delays are also added.
-  The time period increses.

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/96d65967-45e6-416c-87f5-bb5dcb933ae8">

*Data required time:* Complete circuitary needs a certain amount of time to function completely.
	
Data Arrival TIme < Data Required Time.

**If Data Required < Data Arrival - Slack**


*Hold Time Analysis:* 

-  Combinational delay must be greater than the hold timing of capture flow.
-  Hold time is the time required after the clock edge for for the Mux2 to give output based on the previos MUX's input. Or can be called as a time for steady for reliable sampling.


<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/0abfba7a-81e3-49f9-a3f5-0a0b98d653c2">	


	

  --- 
 ### LAB4
 --- 
	
###  Timing modelling using delay tables
 #### Convert grid info to track info

- We dont require information of power , ground in place and route.
- We just require the inner library and i/p , o/p port.
- This is where `.lef` file comes into picture . `.lef` consists of this info. It potects our IP/micro.
- Extracting `.lef` file from .mag file and plug into picorv32 flow.
	- Make sure the input and output port lie on intersection of vertical and horizontal track.
	- Width of standard cell must be odd multiple of the track pitch.
- Go into the `tracks.info` of the `pd`k folder
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/5bd317bb-5cad-4c95-864a-cd8588593efc">
	
- Tracks are used during the routing step. Routes can go over the tracks.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/11cc822b-1be7-4b1e-a4c4-9ed97e31eb8a">
	
- Routes are the metal traces . We have to decide where the routes will go which is given by the tracks.
- Horizontal track li `X 0.23(offset value) 0.46(pitch value)`. Each tracks are spaced *0.46um* to eachother.
- Similarly for the vertical track. This is there for every metal layer.
- Ports are on the li metal layer.
- Will converge the grid with the track value to verify if the ports are on the intersection of horizontal and vertical track.
- Set the grid parameters according to the track dimensions.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/d4f70757-d7c1-4fb4-be3e-169789244e9e">	
	
	
- Routing of li layer can happen with the help of those grids.
- The input and output ports are along the horizontal and vertical intersection
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/cd8f5bfe-2856-41ab-ac89-9f8306b7a1f9">

 #### Convert magic layout to cell .lef
	
- Width and height are odd multiple of track(x/y) pitch
For eg: width 
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/50861e29-e027-4e2a-9891-8a34325ab09e">
-  In layout there are no ports. Port defination are required only for extracting the `.lef` files. Ports are defined as pins of the `.mag` file.
	
-  Give the cell a custom name by 
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/cf851aa7-8c61-4fc0-96c8-a06f54ca9ccf">
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/522d2d9e-d5be-45c6-8ef9-533294c74191">

- Extraction of `.lef` file
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/6b7e964e-83dc-4246-a935-bfc5480473bd">
	
	
- Port enable checkbox creates a pin in `.lef` file
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/511256ca-e87d-4204-91ed-b11e94a31cfc">

#### Introduction to timing libs and steps to include new cell in synthesis

- Move all the files in `src` folder of design `picorv32a`.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/79693c53-9fd9-4c9f-b3a6-aca585f589f3">

- We have to ensure the `abc` flow maps the netlist to the library.
- There are 3 lib files.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/ec604473-3f01-46ab-8ccd-4b7139e6d7e0">
	
- *Typical lib file (tt)*
- *Slow lib file (ss)*
- *Fast lib file (ff)*
	
	
	Defined for different temperature and different voltage values. We will require these libraries for STA analysis.
	
	
-  Copy the above libraries into src folder
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/5ed74f1b-84ed-4aae-85e8-8270b39a046f">

	
- We need to modify the `config.tcl` inside designs folder.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/f05cdcf9-504e-4020-ae05-ee8923645455">

- Running the synthesis to check if we can plug the design
- Overwrite will take the new values that we defined in config.tcl
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/ecf19161-e560-49f7-9494-951b8502a0e9">
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/ae882c7b-cdfa-4311-b480-34c9bcb8b806">
	
`run_synthesis`
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/ae882c7b-cdfa-4311-b480-34c9bcb8b806">
	
	
-  There is huge slack violation at synthesis stage
	
#### Configure synthesis settings to fix slack and include vsdinv
- There are huge delays observed. The report we get is from open STA.
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/eec879aa-e49e-4ee9-96e2-abe3209415a5">
	
-  `wns` has the worst slack.
-  Need to balance between delay and area.
-  `Chip area : 147712.918400`
-  **Buffering** - Buffering is for high fan out rates.Sizing and buffering is been enabled.If fanout is high it needs more driving strength.


<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/a6928f31-52c4-4953-8a4c-d46396ab3914">
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/5c991caf-4677-4815-a76e-de9b3cb21381">


-  After synthesis we perform `run_floorplan`.

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/6bf14657-55c4-4594-b69f-4de62aadc2b1">


- Here ,we encounter an issue with the flow hence , we run other commands for the floorplan stage
```python
	- init_floorplan
	- place_io
	- global_placement_or
	- tap_decap_or
```
	
-  Finally we give the `run_placement` command. Simultanoeously , we obtain a .def file under our placement folder inside the result.
-  To view the layout give the command for magic tool.

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/56af3625-5dc5-4e75-bd47-4b513e72c9df">

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/a434a4ab-90e7-4fa5-85ad-a763f030592c">

-  Zoom into the layout to see whether our cell is plugged into the design.

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/eb61ff3a-2144-4874-b9e7-a32f3ff765a2">
	
- Expand the layout to see the internal structure.
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/75911dd5-56eb-4089-8e89-6171d2133853">

- There are huge delays observed. The report we get is from open STA.
	
	
#### Configure openSTA for post-synthesis timing analysis

-  Ensure slack is reduced.
-  In any placement and route, if there are timing violations timing analysis are carried out seperately. Here , placement and route is an openLANE flow and hence, we will be carrying out our timing analysis on openSTA.
	
-   Inside `openLAN`E folder a file is made for `pre_sta.conf` which sets the units and has all the library definations. Alongwith this , the verilog file is set here from where we have to read.
-    Linking of design is done here.

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/073de62c-613e-4e0d-833a-00c4928968ae">
	
-   The `src` folder consists of the `my_base.sdc` file.
-   In the typical file there are paratmers set our cell in_8. Modify the `my_base.sdc` file accordingly to tha parameters in `typical.lib`
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/a7f18864-34b5-4c5b-9441-3d3f31c364c6">


	
	
#### Steps to run CTS using Triton CTS

-  There is always a tradeoff between *power,area and timing.* Improving one would disturb other. 
-  After making modifications for reducing the delay times , we need the openLANE to use the modified netlist .
-  For this we use the command write_verilog
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/37810ca0-1092-4274-a608-7b95292560ad">

-  We need to update the file in the synthesis folder.
-  The following command is invoked. This will overwrite the command `verilog file`
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/7d53bc9e-27c9-4e5d-a395-743003690c77">

-  To check if the file is overwritten , check in the `synthesis folder`. Check for the modifcations you made are been displayed here.

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/ae3a4dbc-b421-4693-b179-dfecb6aa6051">

	
-  We will run the floorplan again which will be taking the new netlist.
-  Placement an route is an iterative flow.


-  Next is to perform cts: `run_cts` 


<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/28e76784-8fd1-4cb5-8ec1-05a38a8e8d1f">
	
	
-  The clock buffers are added here and we get a new verilog file with new netlist

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/a4e6a686-0c9e-45ab-b72d-4d2bff23d4cd">
	
	
#### Verify CTS

-  Inside the .tcl files there are procs that are defined during the flow. these .tcl commands are inside scripts folder. `.tcl` commands are created for every step.
-  *Procs* are similar to functions of a programming language.
-  When we invoke the command for running any step these are the procs that are been called.
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/9ecbe6ea-41bd-4c2a-a6cf-435ee578640f">
		
-  cts opens *openROAD*. openROAD is an EDA tool used in openLANE. 
-  cts passes control to scripts folder insider openROAD.
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/d8a99d2c-4e2b-4821-9750-a4fc53226186">

-  openROAD carries out the following processes.
	- Floorplanning
	- Placement
	- Optimization
	- Global routing
-  Hence , there is no `synthesis.tcl` file.


<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/dc137154-02cb-4946-96ed-5e17067d7341">

-  Inside `or_cts.tcl` there are few switches.

	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/2b459637-c826-4330-a194-760afc5f2c52">	
	
	
-  Running cts will create a `cts.def` value.
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/71ed7492-5296-47b3-a653-e10787de5a93">	
	
-  The `picorv32a.cts.def.png` file
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/1cce5920-ef97-4975-a8d1-aa6488512ed9">	



#### Analyse timing with real clocks using openSTA buffers on setup and hold time


-  *openROAD* is the part of *openLANE* and *openSTA* integrated on openROAD.
-  Instead of invoking a seperate openSTA tool , we enter into openROAD and do timing analysis as openSTA is already integrated.
-  As we are inside the openLANE we can now use the env variables.
-  Objective here is to do analysis of clock tree on the entire circuit.

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/09a7361a-3d1a-4416-909c-e4d473be9b4b">	
	
-  Here , timing analysis is done by first creating a `db` which is created by the `.lef` and `.def file`.
-  In our analysis will use this `db`.
-  First read the db inside the tmp folder `merged.lef`

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/12b42c16-32b1-4358-a075-6644c8bde20f">	
	
-  Now , we need to read the `def` created post cts. This is present in `results/cts`.
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/9180a549-5ebd-4eb9-89a4-5ec2fd7823cc">
	
	
-  Now , create a `db`. Give a custom name
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/7c02210f-8641-4f99-a2b9-e35581d8ce80">
	
-  Verify whether the db has been created.

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/23a65aed-559f-4354-840e-9ae8f23d9729">
	
- Read the db. Also read the `verilog` file.
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/db7de710-28f7-45b6-a0e5-92c143f49cb4">	
	
-  Read the library 
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/ce1e3f95-f4c2-4eda-ae31-9f936458681f">	

-  Read the `.sdc` file. Use the `set_propagated_clocks` to calculate the actual cell delay. And check for the delay.

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/c93ee53d-a559-4884-b577-690973f448b2">	

-  The slack is	incremented.
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/79d63e35-919c-44ee-9e0b-3949744cf74c">	



	
#### Execute openSTA with right timing libraries and CTS assignment
	
-  *Triton CTS* is built to optimize only according to one corner and we have built the clock tree for typical,min,max cornor.
-  We have built clock tree to particular one cornor but analysizing typical,min,max hence, this synthesis is not correct.
-  First exit from the openROAD by actually typing `exit`.
-  Again open the openROAD and carry the same steps.
-  Copy the libraries and link the design.
-  When openROAD is building the CTS it reads the skew value from reading the buffers from left to right.Check the buffer list. 
-  Skew values must be *10% of the maximum clock period*

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/0a667173-4622-4c4b-8fdb-ac4edef6f801">	


-   Syntax: `lreplace $::env(folder to be modified)` index index.
-   Try to remove the *buff_1*. `lreplace` dont show any modifcations. lreplace dont write the changes.
-   Setup slack time
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/76d145b3-1fe8-4197-af55-a33b3ef03a37">
	
	

-  **Hold slack time**
	
<p align="center"> 
	 <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/13ec62be-9452-43bd-8f8c-5847220caac4">
	

-   Run the `cts` to see the timing modification.
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/c8767b33-f16e-4830-931f-c6deb342ddad">


#### Observe impact of bigger CTS 
	
-   The cts flow failed as we had to modify while running the cts for the second time as the def is CTS def is been included. But , we have to take the placement.def file.	
-    Change the `.def` file.

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/da9c8c20-d019-485d-a60c-52883638c1ea">


-  Run the cts again.
-  Again open the openROAD and make a new db as we made changes in the `CLOCK_BUFFER_LIST` and run the commands again.

<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/62748dde-af4f-475a-bac6-da2ab947b5ad"> 


<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/d96c453f-6329-4b34-98fe-8e64b3cd496e"> 

-   **Setup slack time**
	
<p align="center"> 
    <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/739f8b3f-40fa-46d2-8fda-be277fcf44a4">
	
	


-  **Hold slack time**
	
<p align="center"> 
	 <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/50027d4a-8d4b-4dba-a9af-ceba221ea646">


-  Clock skew timings for setup and hold

<p align="center"> 
	 <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/062393fe-0cd0-4bba-a50f-506d6848d43c">

-  Again if we want to add the *buff_1* . Use the commant `linsert`
	
<p align="center"> 
	 <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/2b0495f1-157e-4157-b7aa-e1e12543d43c">	

 - Commands for openROAD 
```python	
openroad
write_db pico_cts.db
read_db pico_cts.db
read_verilog /openLANE_flow/designs/picorv32a/runs/03-07_11-25/results/synthesis/picorv32a.synthesis_cts.v
read_lef /openLANE_flow/designs/picorv32a/runs/03-07_11-25/tmp/merged.lef
read_def /openLANE_flow/designs/picorv32a/runs/03-07_11-25/results/cts/pico32a.cts.def
read_liberty $::env(LIB_SYNTH_COMPLETE)
link_design picorv32a
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
set_propagated_clock (all_clocks)
report_checks -path_delay min_max -format full_clock_expanded -digits 4
```
	
 ## Day 5:
## RTL to GDSII
  
 --- 
 ### Routing and design rule check
 --- 
 #### Introduction to maze routing
	
-  Routing is the best or the shortest possible path with less number of twist and turns between two points 	
-  Routing tools are based on Lee's Algorithm. For more info on the algorithm, please refer here.	
-  Two stages of Routing: *Global and Detailed Routing*.	
	
-  A typical maze route:
	
<p align="center"> 
	 <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/6ae967d8-a1a7-4351-ae61-75f2c6e0176f">		

	
	
 #### Design Rule Check(DRC)

-  The optical wavlength of the light is so small that minimun with of the wire should be very well taken of.
 
<p align="center"> 
	 <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/27063955-b45a-4e84-83a8-7e0d5a107fe1">	

-  The minimum pitch between two wires is also a concern.
	
<p align="center"> 
	 <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/327a2b50-17d7-491e-a863-b05427bfc8e6">	

-  Minimum wire spacing.
	
	
<p align="center"> 
	 <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/136eb743-3af9-497c-a62a-45bbbc611132">		
	
-  When two nets are shorted. It is a violation of signal short type

<p align="center"> 
	 <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/593d942a-2118-4441-b64e-965e231cb10a">		
	
	
-  Solution:Consider the two nets are of metal 2. One more layer will be introduced on top of it, which is metal 3. Upper metals are wider than lower metals.

<p align="center"> 
	 <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/94497183-efba-4e1d-ad31-9048e5c6e533">		

- *Rule: Vis-width*- connect the bottom metal layer with the top metal layer. 	
	

-  Parasitic Extraction:  The wires have some resistance and capacitence must be extracted and be used. These are extracted in .spef format.
	
	
	
<p align="center"> 
	 <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/ada5929b-2d44-4840-b80f-759c749abb47">		
	

--- 
 ### Power distribution network
 --- 
 #### Power strap

-  Below figure the green box is picorv32. the yellow , red and blue are i/o and power pads. red is the pwer pad and blue is the ground pad.
-  Square ones on the corner are the conour pads.
-  Power inside the chip comes inside through the pads and get supplied through the ring.
<p align="center"> 
	 <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/fcc5773c-19f9-499a-aba7-5a77b5a9274d">
	

#### Global and detailed routing
	
**Global Routing** : Fast route. Routing region is divided into rectangular grid cells. Output of this is a route guide.
	
**Detailed Routing** : Is is done by the triton route. Using algorithm the best possible connectivity is found.I	

	
**Triton Route:** 
-  Performs initial detail route.
-  Honors the preprocessed routing guides i.e, attempts as much as possible to route within route guides.
-  Assumes route guides for each net satisfy inter-guide connectivity.
-  Works on panel routing with intra layer parallel and inter layer sequential routing.
	

	
*Requirements of Preprocessed guided*
-  Should have unit width
-  Should be in preferred direction.
<p align="center"> 
	 <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/1a653fb6-e679-4deb-ac43-2a798309feca">



 --- 
 ### LAB5
 --- 
	
###  Final Steps for RTL to GDSII using Tritonroute and open STA
 #### Build Power Distribution Network
	
-  Invoke the `docker` again in the openLANE , include the `design` and prep the design.
-  Check the `.def` file created post the `cts` run.
<p align="center"> 
	 <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/c3a492f7-f61d-44c2-8979-8556e605709f">	

-   Power distribution network : The power and the ground trails.
<p align="center"> 
	 <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/1097a6ac-da9a-4579-8643-bd767c372bc8">		

-   It reads the `.lef`,`.def` files and create straps for power. 
-   Standard cells must have the power and ground lines.
-   Pdn `.def` file has both the cts def plus the power traps. 

<p align="center"> 
	 <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/9cea341b-e10c-4f48-8e35-43964729dda9">		

-  Run the routing.

 #### Final files post routing 
	
-  Inside the folder of routingthere are guide routes. We observe a fast routing guide which is glocal routing.

<p align="center"> 
	 <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/6765fe5c-9259-4c4f-9c69-490563491206">		

-   There are all nets inside the folder and we see metal layers.
<p align="center"> 
	 <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/650bfe65-9736-4f10-b097-2360faede5e0">		

Exctracting `SPEC` (SPEC extraction is done outside openlane as it does not have SPEC Extractor tool in openlane.

-  `picorv32a.def.png` file generated after routing
	
<p align="center"> 
	 <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/54aeca0c-3884-4389-bcf7-572d3722b5ad">	

 #### Extraction of GDSII file 
	
Finally do run_magic in the openlane to generate gds file.
	

-  At the end we give the command to run magic window.
	
`run_magic`
	
-   A `GDSII` file is generated

<p align="center"> 
	 <img src="https://github.com/Archita0102/PD-OpenLANE-Workshop/assets/66164675/eff3b4e8-c9d2-4e52-bfc8-2b384c9adc4a">	
	
	
	
	
	
	

## Acknowledgement
	
Further I would express my gratitude towards the entire VSD team for organizing and presenting the workshop. A big gratefullness to Kunal Ghosh and  Nickson Jose, VLSI Engineer for conducting such a well organzied,well designed and well planned workshop. This workshop taught me to build a whole RTL to GDSII design and gave more information about the design flow. It also made me familiar with the openSOURCE and openLANE tools and other open EDA tools.
	
	
	
