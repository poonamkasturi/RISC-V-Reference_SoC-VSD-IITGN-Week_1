# 🌟 RISC-V Reference SoC Tapeout Program VSD IITGN

## DAY 2 of WEEK 1 of 20 WEEK PROGRAM

This repository documents the labs and learning materials from the [sky130RTLDesignAndSynthesisWorkshop](https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop) workshop. It covers simulation and synthesis flow to introduce the fundamentals of Register Transfer Level (RTL) design using open-source tools like **Icarus Verilog**, **gtkwave** and **Yosys**, tailored for the Sky130 process node.

### 🔰 DAY 2: TOPICS -----
[![Introduction to Timing Libs](https://img.shields.io/badge/TOPIC-Timing%20Libs-blue)](#31-introduction-to-timing-libs)
[![Hierarchical vs Flat Synthesis](https://img.shields.io/badge/TOPIC-Hierarchical%20vs%20Flat%20Synthesis-green)](#hierarchical-vs-flat-synthesis)
[![Efficient Flop Coding Styles](https://img.shields.io/badge/TOPIC-Efficient%20Flop%20Coding%20Styles-purple)](#efficient-flop-coding-styles)
[![Interesting Optimizations](https://img.shields.io/badge/TOPIC-Interesting%20Optimizations-olive)](#interesting-optimizations)   
[RTL Design] → [Elaboration] → [dfflibmap 🧬] → [Synthesis 🛠️] → [Netlist Generation]
____________________________________________________________________________________

### 📚 Table of Contents   
  **1️⃣ Introduction to Timing .lib**              
         - Library Naming Convention  
         - Liberty File (.lib)   
  **2️⃣ Hierarchical and Flat Synthesis**                     
         - Hierarchical Synthesis  
         - Flat Synthesis
         - Submodule Level Synthesis   
  **3️⃣ Flop Coding Styles and Optimization**                       
         - Different Flop Coding Styles 
         - Interesting Optimizations 

## 📘 2.1 Introduction to Timing .libs  
[![Library Naming Convention](https://img.shields.io/badge/Section-2.1.1--Library%20Naming%20Convention-blue)](#311-library-naming-convention)     
[![Liberty File](https://img.shields.io/badge/Section-2.1.3--Liberty%20File-green)](#312-liberty-filelib)

---

### 2.1.1 Library Naming Convention

SkyWater Technology Foundry provides **seven standard cell libraries** for use in SKY130 designs. These libraries differ in intended applications and are available in **three separate cell heights**.
In this workshop, we are using the following library:

<span style="color:#d73a49"><code>sky130_fd_sc_hd__tt_025C_1v80.lib</code></span>

This library belongs to the `sky130_fd_sc_hd` family, which is designed for **high density**. It offers:            
     - Higher routed gate density          
     - Lower dynamic power consumption         
     - Comparable timing and leakage power         

> ⚠️ **Trade-off**: Lower drive strength compared to other variants.

Libraries in the SKY130 PDK follow this naming convention:
``` <Process name><Library Source Abbreviation><Library Type Abbreviation>[_<Library Name>]```

Let’s break down the terms in the library name used:  
<span style="color:#d73a49"><code>sky130_fd_sc_hd__tt_025C_1v80.lib</code></span>

| Component   | Description                                                                 |
|-------------|-----------------------------------------------------------------------------|
| `sky130`    | Name of the process technology                                              |
| `fd`        | Abbreviation for the library creator — SkyWater Foundry                    |
| `sc`        | Type of content — Digital Standard Cells                                   |
| `hd`        | High density variant                                                        |
| `tt`        | Typical process corner                                                      |
| `025C`      | Characterization temperature (25°C)                                         |
| `1v80`      | Operating process voltage (1.80V)                                           |


### 2.1.2 Liberty File (.lib)

Liberty files are defined by IEEE standard
They are used to describe the **characteristics of standard cells** in a given technology node.     
They are industry standard format used to describe library cells of a particular technology.     
They are a collection of logic module/Standard cells and includes different types of gates and different flavours of these gates.

#### 📄 File Attributes

![ASCII Format](https://img.shields.io/badge/File%20Type-ASCII%20Format-blue)
![Extension .lib](https://img.shields.io/badge/File%20Extension-.lib-green)
![Logic Modules](https://img.shields.io/badge/Content-Logic%20Modules%20%2F%20Standard%20Cells-purple)

#### 📊 Included Parameters

![PVT Characterization](https://img.shields.io/badge/Includes-PVT%20Characterization-purple)
![I/O Relationships](https://img.shields.io/badge/Includes-Input%20%2F%20Output%20Relationships-blue)
![Timing & Power](https://img.shields.io/badge/Includes-Timing%20%26%20Power%20Data-green)
![Noise Parameters](https://img.shields.io/badge/Includes-Noise%20Parameters-red)

> 🧠 These files are essential for synthesis, timing analysis, and power estimation in digital design flows.  
> ...They are widely supported across EDA tools and form the backbone of cell-level timing models

Below image shows some details of sky130_fd_sc_hd__tt_025C_1v80.lib :
![](/DAY_2/Lib_details.png)
	
"sky130_fd_sc_hd__a21110_1" :  cell definitions inside the .lib file as shown in the below image.
![](/DAY_2/cell_a2111o.png)

This cell implements logic function with 5 inputs.       
The logic implemens AND of first two inputs and then output of AND along-with three more inputs is ORed.      
The cell definition also shows amount of leakage power for different combinations of 5 inputs(32 combinations here).

Different flavours of same cell in the .lib file.
![](/DAY_2/and_versions.png)
 
As per above image, the different flavours of  "AND" gate have different size, area and power consumption.       
Below details show the comparison between these different versions of " AND " gate:
```
Area  : and2_0  < and2_2 < and2_4
Power : and2_0  < and2_2 < and2_4 
Delay : and2_0  > and2_2 > and2_4
```
* Larger cells have wider transistors, thus more area, more power consumption but are faster cells(less delay).
* Smaller cells have thin transistors, thus less area, power consumptions but are slower (more delay). 

## 🏗️ 2.2 Hierarchical and Flat Synthesis

[![Hierarchical Synthesis](https://img.shields.io/badge/Section-2.2.1--Hierarchical%20Synthesis-blue)](#221-hierarchial-synthesis)  
[![Flat Synthesis](https://img.shields.io/badge/Section-2.2.2--Flat%20Synthesis-green)](#222-flat-synthesis)  
[![Submodule Level Synthesis](https://img.shields.io/badge/Section-2.2.3--Submodule%20Level%20Synthesis-purple)](#223-submodule-level-synthesis)  
	
### 2.2.1. Hierarchial Synthesis :

**📐 RTL Design Overview**
Cosider an RTL design with **two sub-modules**, both instantiated inside the **top module** as shown in the image below:

> 📷 *(Insert image here showing RTL hierarchy with sub-modules u1 and u2)*

---

**🔍 Expected Netlist Behavior**   
Based on the RTL code, we expect the synthesized netlist to consist of:   
    - AND gates      
    - OR gates      
> These gates are expected to be inferred from the standard cell library.

---

**🛠️ Synthesis Result**   
However, the synthesis result reveals that the **hierarchy has been preserved**.     
Instead of flattening the design into gates, the synthesis tool retains the **sub-module structure**.   

> ✅ Sub-modules `u1` and `u2` are used in place of individual gates in the synthesized design hierarchy.

---

**🧮 Netlist from ABC Synthesis Tool**       
     - The netlist confirms that hierarchy is preserved.       
     - Sub-modules `u1` and `u2` appear explicitly in the netlist.       
     - This matches the structure expected from the RTL code.       

> 📌 This behavior is typical when synthesis is performed in **hierarchical mode**, allowing better modularity and reuse.

---

**📎 Summary**
- RTL design includes two sub-modules: `u1` and `u2`
- Synthesis preserves hierarchy instead of flattening to gates
- ABC tool netlist reflects sub-module structure
- Matches RTL expectations
  
![Hierarchy Preserved](https://img.shields.io/badge/Synthesis-Hierarchy%20Preserved-blue)
![Submodules Inferred](https://img.shields.io/badge/Netlist-Submodules%20u1%20%26%20u2%20Inferred-green)
---

### 2.2.2. 🧱 Flattening the Design in Yosys
In Yosys, the **flatten** command is used to eliminate hierarchy by replacing module instances with their actual implementation.
This is especially useful for inspecting gate-level netlists and verifying that submodules have been fully expanded.

 **🔧 What Does flatten Do?**      
* The flatten pass replaces cells with their implementation using the current design as the mapping library.   
* It behaves similarly to the techmap pass, but uses the current design instead of an external mapping file.   
* Modules or cells with the keep_hierarchy attribute will not be flattened.

 **🛠️ Synthesis and Flattening Workflow**   
After synthesizing the design and generating the netlist, we flatten it to verify that all hierarchies are removed and gates are instantiated directly instead of submodules.

```
$ read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  : READ LIBERTY FILE
$ read_verilog multiple_modules.v                                    : READ FILE with multiple sub-modules
$ synth -top multiple_modules                                        : RUN SYNTHESIS WITH THE TOP MODULE 
$ abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib              : MAP TO STANDARD CELLS
$ write_verilog -noattr multiple_modules_hier.v			             : WRITE HIERARCHICAL NETLIST

TO FLATTEN THE DESIGN
$ read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib         : READ LIBERTY FILE
$ read_verilog multiple_modules.v                                    : READ FILE with multiple sub-modules
$ synth -top multiple_modules                                        : RUN SYNTHESIS WITH THE TOP MODULE 
$ abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib              : MAP TO STANDARD CELLS
$ Flatten							                                 : FLATTEN THE DESIGN TO ...
$ write_verilog -noattr multiple_modules_Flat.v			             : WRITE FLATTENED NETLIST
```
	
Below image shows the difference between the Hierarchical and flattened synthesized netlist:

![](/DAY_2/Hier_vs_Flat.png)

Below image show the generated netlist when desing is flattened:
![](/DAY_2/Flatten_net.png)

### 🔄 2.2.3 Submodule Level Synthesis   
With multiple sub modules in  top level module, sub module level syntheis can be done.  
* **Reuse of Synthesized Logic:** In case of top module having multiple instances of same submodule, synthesis of a submodule helps to synthesize single instance and using it for the other instances. It saves time as only single instance has to be instantiated.
* **Scalability for Large Designs:** For large designs, divide and conquer approach is followed to synthesize submodules which helps in reducing load on synthesis tool.

🧪 Example: Synthesizing sub_module1 from multiple_modules.v
To understand submodule-level synthesis better, synthesize only sub_module1 and observe the resulting netlist.

```
$ read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
$ read_verilog multiple_modules.v
$ synth -top sub_module1                                              : RUN SYNTHESIS WITH THE SUB-MODULE 
$ abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
$ write_verilog -noattr multiple_modules_sub.v			
```
After synthesis we have find out that synth command just looks at the specified sub_module1 alone and sub_module2 is ommitted from synthesis.

Below images show the sub module synthesis of sub_module1:
![](/DAY_2/sub_all.png)

Below images show the sub module synthesize netlist:
![](/DAY_2/sub_net.png)	


### ⏱️ 2.3 Flop Coding Styles and Optimization
Before diving into flip-flop coding styles, it's important to understand why flip-flops are essential in digital design.

#### 2.3.1 ⚠️ Why Flip-Flops Are Needed
In a purely combinational circuit, every gate introduces a propagation delay - a finite time for any of the input changes to reflect at the output. When gates have varying delays, input transitions can cause unwanted output fluctuations, known as: **Glitches**

#### 2.3.2 🧠 Boolean vs Reality
From Boolean algebra, the output of the circuit should always be 1.    
However, due to gate delays, the actual output may momentarily dip, as shown in the timing diagram:

![](/DAY_2/Glitch&wave.png)
 
Glitch and Timing Waveform

>The more complex the combinational logic, higher the chances of glitches.


#### 2.3.3 🛡️ Avoiding Glitches with Flip-Flops
To mitigate glitches, storage elements—flip-flops are introduced.
- Flip-flops store the output of a combinational block.
- Their outputs change only on clock edges, isolating them from input transitions between edges.
- By placing flip-flops between combinational paths, glitch propagation is prevented and stable outputs are ensured.

#### 2.3.4 🧱 Flip-Flops as Timing Barriers
Flip-flops act as barriers at the input of a combinational circuit, allowing outputs to settle before being used downstream. This results in:
- Stable inputs to combinational blocks
- Predictable and glitch-free outputs
- Improved timing closure and design reliability

#### 2.3.5 📝 Importance of Initial States in Flip-Flop Coding 
When coding flip-flops, it's critical to specify their initial states:
- The output of a flip-flop often feeds into combinational logic.
- If the initial state is unspecified or unknown, the combinational logic may evaluate to garbage values, leading to unpredictable behavior.

**🎛️ Controlling Initial States: RESET and SET**   
To avoid such issues, control pins are introduced to manage the initial values of flip-flops:
- RESET: Sets the flip-flop output to 0
- SET: Sets the flip-flop output to 1

##### 🔧 Flip-Flop Initialization Control

| Control Signal | Output Value | Timing Type     | Description                                                                 |
|----------------|--------------|------------------|-----------------------------------------------------------------------------|
| `RESET`        | 0            | Asynchronous     | Output is immediately set to 0, independent of clock                        |
| `RESET`        | 0            | Synchronous      | Output is set to 0 only on the active clock edge                           |
| `SET`          | 1            | Asynchronous     | Output is immediately set to 1, independent of clock                        |
| `SET`          | 1            | Synchronous      | Output is set to 1 only on the active clock edge                           |

> Proper use of RESET and SET ensures stable initialization and prevents logic corruption during simulation or synthesis.

	
### 2.4 Different Flop coding styles
#### 2.4.1 🧠 Flop Mapping During RTL Synthesis
When synthesizing RTL code, it's crucial to correctly map flip-flops using the **dfflibmap** command. This ensures that the synthesis tool knows where to source the flop cells from to map the sequential elements of the code.

#### 2.4.2 🔧 Command Used
```
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
#### 2.4.3 📚 Why Use dfflipmap
- In many library flows, flip-flops and standard cells are stored in separate Liberty files.
- The synthesis tool needs explicit direction to locate the flop cells.
- Without this step, flop inference during synthesis may fail or use incorrect cells leading to incomplete or incorrect synthesis result
	✅ Our Case
- The same Liberty file holds information for both standard cells and flops.
- Hence, the same path is passed to dfflibmap.
 `      $ dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib      

📁 Library Path
../lib/sky130_fd_sc_hd__tt_025C_1v80.lib


💡 Tip: Always verify your Liberty file contains both standard cells and sequential elements before skipping separate flop mapping.
>This ensures that flip-flops in the RTL are properly recognized and mapped during synthesis.

**Synthesizing various D-Flip flop with set and reset being synchronous or asynchronous**

Synthesis Steps using yosys remain the same with an additional step of dfflibmap
`$ dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib`

and the corresponding file names
```
$ read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
$ read_verilog dff_asyncre.v
$ synth -top dff_asyncres
$ dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
$ abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
$ show		
```
#### Asynchronous reset D-Flip flop
    - reset does not waits for the clock edge
#### Asynchronous set D-Flip flop
    - set will happen only at the next clock edge despite the time when set signal is asserted
#### Synchronous reset D-Flip flop 
    - reset will happen only at the next clock edge despite the time when set signal is asserted
#### Asynchronous & Synchronous reset D-Flip flop
    - The flop has both synchronous as well as asynchronous resetting options
    - Asynchronous reset will get the highest priority
	
**NOTE**: For asynchronous reset and synchronous set together, it will not cause race condition. 
          But if the desing have both synchronous reset and set ,it may cause race condition.
 
