## DAY 2: 
     ➡️ Timing libs 
     ➡️ Hierarchical Vs Flat Synthesis 
     ➡️ Various FLop Coding Styles

## 🌟 Timing Libs
The libraries have a collection of various flavours of the standard cells in order to capture the variations of Process Voltage Temperature (PVT) conditions

## 🌟 Hierarchical Vs Flattened, Multiple Submodules 
## 🔧 Sky130 RTL Design & Synthesis Flow using Yosys
This repository demonstrates a complete RTL-to-Gates synthesis flow using  Yosys and Sky130 process node, integrating the Verilog modules, Yosys synthesis steps, Liberty mapping, abc optimization and visualization.




## 📁 Verilog Module Definitions having submodules   - HIERARCHICAL DESIGN APPROACH
We begin with a simple RTL design composed of two sub modules:   
```  pkasturi@pkasturi-virtualBox:-/sky130RTLDesignAndSynthesisWorkshop/verilog_files $gvim multiple_modules.v  ```

<img width="600" height="400" alt="image" src="https://github.com/user-attachments/assets/2f889593-2527-4e92-b7a9-0515eb31d900" /> </dv>

📌 File path: /sky130RTLDesignAndSynthesisWorkshop/verilog_files/multiple_modules.v

## ⚙️ Yosys Synthesis Flow after invoking yosys    
1️⃣ Reading Liberty and Verilog Files   
     - read_liberty -lib ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib    
     - read_verilog multiple_modules.v   

<img width="500" height="400" alt="image" src="https://github.com/user-attachments/assets/176984d2-1798-432a-9356-5349b274b375" /> </dv>    

✅ Liberty file loaded with 418 cell types
✅ Verilog modules parsed: sub_module1, sub_module2, multiple_modules

2️⃣ Synthesizing the Design  `     yosys> synth -top multiple_modules   `

<img width="740" height="550" alt="image" src="https://github.com/user-attachments/assets/81a9c689-b16b-41cd-bd71-33aaf8412fc8" />

3️⃣ Hierarchy and Cell Mapping    `     yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib    `
      
     - 🧠 ABC Optimization     
     - 🔁 ABC maps RTL to standard cells  
     - shows multiple_modules   
      
📊 Graphical Visualization THROUGH a Graphviz file     
     - 📌 Graphviz output shows RTL flow and module interconnects
     - 🖼️ yosys shows, the design is rendered as a block diagram:    

<img width="700" height="250" alt="image" src="https://github.com/user-attachments/assets/717076a0-873a-458a-b12f-4b201945c366" />

- Inputs: a, b, c   
- Intermediate wire: net1   
- Output: y   
- Modules: sub_module1, sub_module2 connected sequentially    
   - sub_module1   
   - sub_module2     
- Design hierarchy shows nested instantiations     </dv>

**📝 Final Dump and Hierarchical Netlist**    `     yosys> write_verilog multiple_modules_hier.v    `
📚 hierarchical design netlist

<img width="740" height="550" alt="image" src="https://github.com/user-attachments/assets/d4e7682d-ce36-46ed-a9d4-a03c576818c5" />
<img width="740" height="550" alt="image" src="https://github.com/user-attachments/assets/05769382-460d-4b21-abc4-d768bfa4d0cc" />

## 📁 Verilog Module Definitions having submodules   - FLATTEN DESIGN APPROACH
We begin with the same RTL design composed of two sub modules:   
```  pkasturi@pkasturi-virtualBox:-/sky130RTLDesignAndSynthesisWorkshop/verilog_files $gvim multiple_modules.v  ```

📌 File path: /sky130RTLDesignAndSynthesisWorkshop/verilog_files/multiple_modules.v

### TO FLATTEN THE DESIGN commands to be followed 

```
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025c_1v80.lib
yosys> read_verilog multiple_modules.v
yosys> synth -top multiple_modules
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025c_1v80.lib
yosys> flatten
yosys> show multiple_modules
yosys> write_verilog -noattr multiple_modules_flat.v
yosys> gvim multiple_modules_flat.v
```
Additional command is `    flatten    `

**DESIGN THAT IS FLATTENED ...shows gates**
if order of the two commands is as 
```
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025c_1v80.lib
yosys> flatten
```
i.e first liberty file mapped and then flattened ...the tool keeps the submodule structure but now represents the complete design in gates

<img width="940" height="900" alt="image" src="https://github.com/user-attachments/assets/1f123985-bc3f-4200-92fd-9c1acfa90331" />

**BUT IF ORDER OF THE TWO COMMANDS IS CHANGED** .. the tools optimizes the LOGIC and reduces redundant submodules
```
yosys> flatten
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025c_1v80.lib

```
<img width="633" height="350" alt="image" src="https://github.com/user-attachments/assets/419927ab-a77e-46ec-bd3f-dcdb049a1bdb" />  </dv>
### For ref: The Design That Retains Hierarchy - shows submodules

<img width="400" height="120" alt="image" src="https://github.com/user-attachments/assets/6e3636db-2f9b-4c59-8a55-e5a3e8daca8d" />

A comparison of the design that maintains hierarchy and that is flattened is as below
<img width="700" height="700" alt="image" src="https://github.com/user-attachments/assets/db1ed762-56d4-48c5-b72c-35fad237f323" />
<img width="700" height="700" alt="image" src="https://github.com/user-attachments/assets/b88fa05e-e87b-4a49-bcbc-940283ae6ffc" />

# 🌟 Synthesis of individual sub-modules
```
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025c_1v80.lib
yosys> read_verilog multiple_modules.v
yosys> synth -top sub_module2
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025c_1v80.lib
yosys> show multiple_modules
yosys> write_verilog -noattr multiple_modules_flat.v
yosys> gvim multiple_modules_flat.v
```

<img width="632" height="200" alt="image" src="https://github.com/user-attachments/assets/09ddcaaa-374d-4b99-b8eb-38fb502a13c4" />


## 🌟 Flop Coding STyles
## Active High Asynchronous reset 

Active High reset
####  ...........................................
<img width="700" height="250" alt="image" src="https://github.com/user-attachments/assets/2b08530b-005f-40d4-8b88-ae896d076b0a" />

On positive edge of clock data is read

####  ...........................................

<img width="700" height="250" alt="image" src="https://github.com/user-attachments/assets/8e6d45ab-d8df-4c7e-a4c1-0306b37edca3" />


Reset is Asynchronous

####  ...........................................

<img width="700" height="250" alt="image" src="https://github.com/user-attachments/assets/a8ed6153-bbb9-4ff6-93de-4eb91100e68e" />

####  ...........................................

<img width="700" height="400" alt="image" src="https://github.com/user-attachments/assets/b7b549f2-023d-4e45-a910-990ff6c76c4b" />

####  ...........................................

<img width="700" height="700" alt="image" src="https://github.com/user-attachments/assets/6a798698-3878-4d79-9b7b-6ae2ff16d6d4" />

####  ...........................................
<img width="500" height="200" alt="image" src="https://github.com/user-attachments/assets/f2349365-1e25-4a0a-9328-cf64ad724c55" />

####  ...........................................
<img width="700" height="300" alt="image" src="https://github.com/user-attachments/assets/fe5d8da3-4c21-44be-be3a-4a0ca2c6cad3" />


----
## Active High Asynchronous Set


####  ...........................................

Active High Asynchronous Set

<img width="700" height="250" alt="image" src="https://github.com/user-attachments/assets/840309c9-06e5-4784-81b9-33778ce0bdb2" />

####  ...........................................

Data is read on positive clock edge

<img width="700" height="250" alt="image" src="https://github.com/user-attachments/assets/cbac7a3e-c1fc-40e3-9fb3-b4a16dddd254" />

####  ...........................................

<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/61cde6cb-9bef-447e-aa69-33abcfaff4c2" />

####  ...........................................

<img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/eeecdd66-5681-462c-bc38-ac925fb5c428" />

---



## Active High Synchronous Reset
<img width="700" height="250" alt="image" src="https://github.com/user-attachments/assets/83d2e198-dadc-4491-9eac-26e53cdd2d8e" />

####  ...........................................


Data Read on positive edge of clock

<img width="700" height="250" alt="image" src="https://github.com/user-attachments/assets/68711a47-3f6f-4e3f-8788-3e99d260165e" />

####  ...........................................

<img width="300" height="300" alt="image" src="https://github.com/user-attachments/assets/ac039cd8-fe20-4b68-9eaa-56f466b5aef0" />

####  ...........................................

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/1881d2ff-be97-42c9-b13f-d41880d7e7cb" />

---

## Asynchronous Reset and Synchronous Reset – both active high
Synchronous Reset

<img width="700" height="200" alt="image" src="https://github.com/user-attachments/assets/88ee6a98-d3b5-4b60-b182-1598ff388eb3" />

####  ...........................................

<img width="700" height="200" alt="image" src="https://github.com/user-attachments/assets/390eaa8f-cdf5-4071-8579-033d929c1be8" />

####  ...........................................


Asynchronous Reset

<img width="700" height="200" alt="image" src="https://github.com/user-attachments/assets/927353b2-0422-4bd8-91b7-3bb6857957d1" />

####  ...........................................

Data is read when both Asynchronous reset and Synchronous reset are low

<img width="700" height="200" alt="image" src="https://github.com/user-attachments/assets/ea00c1c6-555e-4494-abc4-5c1926ce82e2" />

####  ...........................................

<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/9a0e39e3-c795-4f98-b46f-59787b6b7b34" />

####  ...........................................

<img width="700" height="300" alt="image" src="https://github.com/user-attachments/assets/a76f27f8-d6a0-4033-a3f0-f54217e1d704" />


## INTERESTING OPTIMIZATIONS
### y = a x 2
<img width="600" height="150" alt="image" src="https://github.com/user-attachments/assets/ce0cecbe-56af-4bcd-adb7-ba7dd1c5be6f" />

<img width="300" height="150" alt="image" src="https://github.com/user-attachments/assets/5dd92356-3dc6-467c-a36b-2569b3f335c9" />

<img width="700" height="300" alt="image" src="https://github.com/user-attachments/assets/0ae59fcf-40d8-455e-b360-049f24e3afd8" />


### y = a x 9
<img width="700" height="200" alt="image" src="https://github.com/user-attachments/assets/2400c6bf-2087-450d-9afd-b20cb8df95f0" />

<img width="300" height="150" alt="image" src="https://github.com/user-attachments/assets/7a7397e5-fbfa-4033-9666-882e547bb565" />

<img width="700" height="200" alt="image" src="https://github.com/user-attachments/assets/70b761df-599a-456e-b7c6-e114069ad50f" />







