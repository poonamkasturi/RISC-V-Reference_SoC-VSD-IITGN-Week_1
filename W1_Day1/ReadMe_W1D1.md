# 🌟 RISC-V Reference SoC Tapeout Program VSD IITGN

## DAY 1 of WEEK 1 of 20 WEEK PROGRAM


This repository documents the labs and learning materials from the [sky130RTLDesignAndSynthesisWorkshop](https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop) workshop. It covers simulation and synthesis flow to introduce the fundamentals of Register Transfer Level (RTL) design using open-source tools like **Icarus Verilog**, **gtkwave** and **Yosys**, tailored for the Sky130 process node.

#### ➡️ Simulation ➡️ Synthesis ➡️ Netlist
<!-- 🟦 Simulation Stage -->
![Simulation](https://img.shields.io/badge/STAGE-Simulation-blue)     
![iverilog](https://img.shields.io/badge/iverilog-good__mux.v%20tb__good__mux.v-blue)
![run](https://img.shields.io/badge/run-./a.out-orange)
![gtkwave](https://img.shields.io/badge/gtkwave-tb__good__mux.vcd-green)

<!-- 🟨 Synthesis Stage -->
![Synthesis](https://img.shields.io/badge/STAGE-Synthesis-yellow)     
![yosys](https://img.shields.io/badge/tool%20invoke-Yosys-blue)     
![read_liberty](https://img.shields.io/badge/yosys-read__liberty%20--lib%20../lib/sky130__fd__sc__hd___tt__025C__1v80.lib-lightgrey)
![read_verilog](https://img.shields.io/badge/yosys-read__verilog%20good__mux.v-lightgrey)
![synth](https://img.shields.io/badge/yosys-synth%20--top%20good__mux-yellow)
![abc](https://img.shields.io/badge/yosys-abc%20--liberty%20../lib/sky130__fd__sc__hd___tt__025C__1v80.lib-lightgrey)

<!-- 🟪 Visualization & Netlist -->
![Netlist](https://img.shields.io/badge/STAGE-Netlist_&_Visualization-purple)     
![show](https://img.shields.io/badge/yosys-show-purple)
![write_verilog](https://img.shields.io/badge/yosys-write__verilog%20--noattr%20good__mux__netlist.v-green)
![gvim](https://img.shields.io/badge/yosys-!gvim%20good__mux__netlist.v-red)

---

## 🔰 DAY 1: Introduction to Verilog RTL design and Synthesis

---
**📌 Key Highlights**  
- Practical RTL workflows aligned with the **Sky130 Process Design Kit (PDK)**  
- Simulation using **Icarus Verilog** and waveform analysis with **GTKWave**  
- Synthesis using **Yosys**, targeting Sky130 standard cell libraries  
- Real-world design flow from **RTL** to **gate-level netlist**


**🏁 Outcome** 

To Gain a fabrication-aware understanding of digital design, bridging theory with open-source implementation.

---

### 📚 Table of Contents

- [🔰 Tools Used](#-tools-used)
- [📅 Day 1 – Introduction to Verilog RTL Design and Synthesis](#-day-1--introduction-to-verilog-rtl-design-and-synthesis)
  - [1️⃣ Introduction to open-source simulator Icarus Verilog - iverilog](#1-introduction-to-open-source-simulator-icarus-verilog-iverilog)
  - [2️⃣ Labs Using iverilog and gtkwave](#2-labs-using-iverilog-nd-gtkwave)
  - [3️⃣ Introduction to Yosys and Logic Synthesis](#3-introduction-to-yosys-and-logic-synthesis)
  - [4️⃣ Labs using Yosys and Sky130 PDKs](#4-labs-using-yosys-and-sky130-pdks)                          
                     - 🔧 Yosys Synthesis Flow Setup - how to synthesize Verilog designs using **Yosys**,  
    											   targeting the Sky130 standard cell library.   
                     - ✅ Verifying the Synthesized Netlist - Inspect the synthesized netlist  
    					and validate its structure and logic using **NetlistSVG** or other visualization tools.

  
### TOOLS USED:

* **Icarus Verilog:** It is a verilog simulation and synthesis tool. It operates as a compiler, compiling source code written in Verilog (IEEE-1364) into some target format.Icarus Verilog is an open source Verilog compiler that supports the IEEE-1364 Verilog HDL including IEEE1364-2005 plus.     
* **GTKwave	:** GTKWave is a VCD waveform viewer based on the GTK library. This viewer support VCD and LXT formats for signal dumps.It also reads LXT, LXT2, VZT, FST and GHW files as well as standard Verilog VCD/EVCD files and allows their viewing.     
* **Yosys 	:** Yosys is a framework for Verilog RTL synthesis. It currently has extensive Verilog-2005 support and provides a basic set of synthesis algorithms for various application domains.
**Technology used:** Sky130 technology.   
	
### 1. Day1 - Introduction to Verilog RTL Design and Synthesis
The first day of the workshop covers the brief description of iverilog simulator, Test Bench setup, iverilog simulation flow  and lab using iverilog, gtkwave, yosys tools.    

### 1.1 Introduction to iverilog, Design and Test Bench	
* **RTL Design :** It is the actual verilog code or set of verilog codes which has intended functionality to meet with the required specifications.
Register Transfer Level (RTL) is an abstraction for defining the digital portions of a design. It is the principle abstraction used for defining electronic systems today and often serves as the golden model in the design and verification flow. The RTL design is usually captured using a hardware description language (HDL) such as Verilog or VHDL.
	
* **Test Bench :** It is the setup to apply stimulus(test_vectors) to the design to check its functionality. So to ensure that our design is obeying the required specification, we apply stimulus to the design ,observe its output and match it with respect to the specification.
	
	
### 1.2 Simulating the Designs with iverilog
* **Simulation :** Simulation is the process by which the design model coded in the HDL gets executed (after a successful compilation and elaboration) based on a given execution model.It is done by using a simulation software (simulator) to verify the functional correctness of a digital design that is modeled using a HDL (hardware description language) like VHDL,Verilog. It is the process of checking whether the design is adhering to the given specs.
		
* **Simulator :** It is the tool used for simulating the design.The simulator used here is "iverilog". The RTL design is the implementation of the required specification and the functionality of the specs needs to be verified by simualting the design using simulator.
	
* **How does a simulator work ?**
Simulator works by continuously monitoring the changes in the inputs. Upon a change in any one of the inputs, the output is re-evaluated. If there is no change in input, the ouput will not be evaluated. Simulator dumps the change to the ouput to a file according to the change in input.
    
## 1.3 Design and Test Bench setup
 * The RTL design written in verilog code has some primary inputs and primary outputs. It may have one or more than one primary inputs and one or more than one primary outputs.
 * We need to give stimulus to all the primary inputs and need to observe the primary outputs. Thus we need stimulus generator at the input and stimulus observer at the output.
 * For giving stimulus we write the test bench, for that the design(module) is instantiated in the test bench, then stimulus is applied.
 * It is important to note that the test bench doesn't have any primary input and primary output.
 <dl>
  <dd>Below image shows the test bench set up : </dd>
 </dl>
 
![](https://github.com/poonamkasturi/RISC-V-Reference_SoC-VSD-IITGN-Week_1/blob/main/W1_Day1/W1D1_ScreenShots/1.%20Test_bench_setup.png)
 	
## 1.4 iverilog Simulation Flow
* **Inputs to the simulator**:  
    The iverilog simulator accepts two main inputs.  
	1. RTL Design    : This is the behavioral description of the specs in some HDL language(verilog here).  
    2. Test Bench    : The testbench is the setup to apply stimulus or test vectors to the design to check its functionality and correctness.
  Test bench instantiate the verilog module of design and give stimulus to the input ports of the RTL design.
	
* **Output of the simulator** :  
 The iverilog simulator outputs a value chage dump (.vcd) file as output.This vcd file can be viewed using the GTKWave viewer tool.  
<dl>
  <dd>Below image shows the complete iverilog simulation flow : </dd>
</dl>	 
 
![](https://github.com/poonamkasturi/RISC-V-Reference_SoC-VSD-IITGN-Week_1/blob/main/W1_Day1/W1D1_ScreenShots/2.%20iverilog_sim_flow.png)

## 1.5 Iverilog Simulation of Multiplexer(MUX)
Iverilog simulation is done as per below steps:
*  Iverilog takes RTL design and test bench as input and generates a executable file " a.out".
*  On executing "a.out" ,it dumps the simulation in value change dump format(.vcd file).
*  Then GTKWave takes the .vcd file and display the simulation waveform.
The above steps are shown below:

### 1. Run iverilog with the design verilog file and the testbench as inputs. This will create an executable named a.out.
   
```
$ cd sky130RTLDesignAndSynthesisWorkshop
$ ls -ltr
$ cd verilog_files 
$ iverilog good_mux.v tb_good_mux.v
```

### 2. Execute the file a.out. This will generate the value change dump (.vcd) file.

```
$ ./a.out
```
### 3. Now run GTKwave with the vcd file as input to view the simulation waveform.
```
$ gtkwave tb_good_mux.vcd
```
### 4. To view the signal on the wave window click and drag them to the signal column..


     
## 1.6 Synthesis with Yosys
**Synthesis :** Synthesis is the process during which RTL design actually gets converted into a circuit. There are special programing languages called Hardware Description Languages (HDLs) which are used to describe the hardware of a circuit and then the computer makes the circuit based on the program written. After synthesis we obtain a “Gate Level Netlist”. This netlist is how our circuit will look. The tool used here to do synthesis is Yosys.
In simple terms:
* It does RTL to gate level translation
* The design is converted into gates and connections are made between them.
* This gives out a file called netlist.
	      
### 1.6.1 Yosys Synthesis Flow setup
The synthesis tool takes the RTL design and the liberty file(.lib) as inputs and synthesize the RTL design into netlist which is the gate level representation of the RTL design.

Below image shows the Yosys synthesis flow setup: 

![](https://github.com/poonamkasturi/RISC-V-Reference_SoC-VSD-IITGN-Week_1/blob/main/W1_Day1/W1D1_ScreenShots/Yosys_setup.png)

   
Below are the steps to synthesize the multiplexer design(good_mux.v):
### 1. To invoke Yosys:   
```
$ cd verilog_files
$ yosys
```
Below image show the yosys synthesis suite:
![](https://github.com/poonamkasturi/RISC-V-Reference_SoC-VSD-IITGN-Week_1/blob/main/W1_Day1/W1D1_ScreenShots/yosys_invoke.png)

### 2. Reading sky130 standard library :
```
$ read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
```
read_liberty : It reads cells from liberty file as modules into current design.
	       The option "-lib"  only create empty blackbox modules.
	       
### 3. Reading the RTL design(verilog file) :
```
$ read_verilog good_mux.v  
```
**read_verilog :** This command is used to read the verilog desgin file. It load modules from a Verilog file to the current design.
Below image show the yosys synthesis suite:
![](https://github.com/poonamkasturi/RISC-V-Reference_SoC-VSD-IITGN-Week_1/blob/main/W1_Day1/W1D1_ScreenShots/Yosys_setup_2.png)

### 4. Synthesize the top level module  : Below command is used to synthesize the module
```
$ synth -top good_mux  
```
**synth :** This command runs the default synthesis script. This command does not operate on partly selected designs.
**-top <module> :** This option use the specified module as top module (default='top'). Here we have module name "good_mux".
	
![](https://github.com/poonamkasturi/RISC-V-Reference_SoC-VSD-IITGN-Week_1/blob/main/W1_Day1/W1D1_ScreenShots/Yosys_setup_3.png)
	
### 5. Mapping to the standard library 
```
$ abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
**abc :** This pass uses the ABC tool for technology mapping of yosys's internal gate library to a target architecture. This command converts RTL code into gates,cells which is taken from the sky130_fd_sc_hd__tt_025C_1v80.lib file.
**-liberty <file> :** It generate netlists for the specified cell library (using the liberty file format).

**NOTE:** The path of the sky130_fd_sc_hd__tt_025C_1v80.lib   should match with the path where the file is saved in the system
![](https://github.com/poonamkasturi/RISC-V-Reference_SoC-VSD-IITGN-Week_1/blob/main/W1_Day1/W1D1_ScreenShots/Yosys_setup_4.png)	
	
### 6. To view the result as a grapviz use the below command
```
$ show
``` 
**Show :** It creates  graphviz DOT file for the selected part of the design and compile it to a graphics file (usually SVG or PostScript).It is used to show the logic realized from the verilog code after synthesis.
![](https://github.com/poonamkasturi/RISC-V-Reference_SoC-VSD-IITGN-Week_1/blob/main/W1_Day1/W1D1_ScreenShots/Yosys_setup_5.png)
	
### 7. To write the netlist in a .v file
$ write_verilog -noattr <filename.v>
![](https://github.com/poonamkasturi/RISC-V-Reference_SoC-VSD-IITGN-Week_1/blob/main/W1_Day1/W1D1_ScreenShots/Yosys_setup_6.png)
