# 🌟 RISC-V Reference SoC Tapeout Program VSD IITGN

## DAY 3 of WEEK 1 of 20 WEEK PROGRAM

This repository documents the labs and learning materials from the [sky130RTLDesignAndSynthesisWorkshop](https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop) workshop. It covers simulation and synthesis flow to introduce the fundamentals of Register Transfer Level (RTL) design using open-source tools like **Icarus Verilog**, **gtkwave** and **Yosys**, tailored for the Sky130 process node.

### 🔰 DAY 3: TOPICS -----
- [Day3 - Combinational and Sequential optimizations](#Day3---Combinational-and-Sequential-optimizations)
   1. [Introduction to Optimizations](#Introduction-to-Optimizations)
   2. [Combinational Logic Optimizations](#Combinational-Logic-Optimizations)
      * [Constant Propagation](#Constant_Propagation)
      * [Boolean Logic Optimization](#Boolean-Logic-Optimization)
   3. [Sequential Logic Optimizations](#Sequential-Logic-Optimizations)
      * [Sequential Constant Propagation](#Sequential-Constant-Propagation)
      * [Advanced Techniques](#Advanced-Techniques)
   4. [Logic optimizations with Yosys](#Logic-optimizations-with-Yosys)
      * [ Combinational Logic Optimizations](#Combinational-Logic-Optimizations)
      * [Sequential Logic Optimizations](#Sequential-Logic-Optimizations)
   5. [Sequential optimizations for unused outputs](#Sequential-optimizations-for-unused-outputs)
   

		
# 3. Day3 - Combinational and Sequential optimizations
## 3.1 Introduction to Optimizations:
**Logic Optimization** :
It is the process of finding an equivalent representation of the specified logic circuit under one or more specified constraint. Optimization is the process of iterating through a design such that it meets timing, area and power specifications.The purpose of logic optimization is to enhance the simulation efficiency. A typical optimization process consists of the transformations,as each gate corresponds to one or more statements in the compiled code, logic optimization reduces the program size and execution time.

## 3.2 Combinational Logic Optimizations:
The logics optimization squeezes the logic to get the most optimized design. The optimized design is then efficient in terms of area and power saving.
### Common techniques used for optimizing combinational logic :
* Constant Propagation 
	* Direct Optimization technique
* Boolean Logic Optimization.
	* Karnaugh map
	* Quine Mckluskey
   
### 3.2.1 Constant Propagation:
Constant propagation is the process of substituting the values of known constants in expressions. 
Constant propagation eliminates cases 
    - where values are copied from one location/variable to another, in order to simply assign their value to another variable. 
The constant input to the circuit is propagated to the output which results in a minimized expression of the logic.
Below image show propagation of constant input to the output:

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/e0e51e26-1ed0-43f2-9987-2167247e632a" />


### 3.2.2 Boolean Logic Optimization:
In terms of Boolean algebra, the optimization of a complex boolean expression is a process of finding a simpler one, which would upon evaluation ultimately produce the same results as the original one. This technique uses boolean algebra rules/theorems to minimize the logic.
Below image shows the example of optimization of given boolean logic:  

<img width="800" height="600" alt="Boolean_logic_optimization" src="https://github.com/user-attachments/assets/fe36ec7f-6765-47c1-80e6-1b7a83a52b78" />

	
## 3.3 Sequential Logic Optimizations
Below are the techniques used for optimizimg the sequential logic :
* Basic Tecnique
	* Sequential Constant Propagation
* Advanced Technique
	* State Optimization
	* Retiming
	* Sequential Logic cloning(Floorplan aware synthesis)
   
### 3.3.1 Sequential Constant Propagation

In above code we can see that if ```set = 1 ``` then ``` Q = 1 ``` and when ``` set = 0 , clk = 1 ``` then ``` Q = 0 ```. Thus output is following input 'd' at clock edge. So the output Q can not be optimized, thus sequential constant can not propagate. 
	If a constant connected to the input of a D Flop makes the Q output always constant.. 
                then the flop can be optimized (replaced by the constant propogated)

<img width="800" height="600" alt="Sequential_Constant_propagation_opt" src="https://github.com/user-attachments/assets/38699711-bb3a-4c99-9d18-750fcccab4d5" />


  But if a constant connected to the input of a D Flop does not makes its Q output a constant value
                then that flop or logic can not be optimized. The Flop needs to be retained

<img width="450" height="300" alt="Sequential_Constant_NO_propagation_" src="https://github.com/user-attachments/assets/d0a23e40-b98f-4880-8f9e-332e4ccd5424" />

**NOTE** :
* A constant connected to the input of a flop does not mean that we can always optimize its output.
* Every flop with D input tied to '0' is not a sequential constant.
* For flop to become sequential constant , the Q output pin should always take a constant value.

	
### 3.3.2 Advanced Techniques
1. State Optimization : it is used  for Optimization of unused states.
2. Sequential Logic cloning : It is done when we are using physical aware synthesis.
3. Retiming : It is done to reduce combinational delay and to improve the performance of the circuit.
	
	
## 3.4 Logic optimizations with Yosys
	
### 3.4.1 Combinational Logic Optimizations:
To understand optimization with yosys, lets take an exmaple of opt_Check4.v :


Synthesis & optimization commands :
```
$ read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
$ read_verilog opt_check4.v
$ synth -top opt_check
$ opt_clean -purge 				: command to do all optimizations
$ abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
$ show		
```
**opt_clean** : This pass identifies wires and cells that are unused and removes them. Other passes often remove cells but leave the wires in the design or reconnect the wires but leave the old cells in the design. This pass can be used to clean up after the passes that do the actual work.This pass only operates on completely selected modules without processes.

**-purge** : also remove internal nets if they have a public name.

<img width="800" height="900" alt="Comb_yosys_opt" src="https://github.com/user-attachments/assets/7154dec8-ecdc-4343-a37d-36f3c6ddd4fa" />


### 3.4.2 Sequential Logic Optimizations
To understand optimization with yosys, lets take an exmaple of dff_const5.v :
In the below circuit we can see that the circuit obatained after synthesis and optimization is similar to what we expected as per RTL code. Thus in this case no optimization is possible. 

<img width="900" height="900" alt="Sequential_yosys_opt" src="https://github.com/user-attachments/assets/b8e7f12a-78de-4786-8db1-b421cb6b1e60" />


## 3.5 Sequential optimizations for unused outputs :
Lets take an exmaple code counter_opt.v to understand this case:
<img width="900" height="900" alt="Sequential_unused_output" src="https://github.com/user-attachments/assets/d7d7a88f-3a1c-4a8a-9192-10c3d632c750" />

At first going through code, we might be think that the design would contain 3 flops after synthesis as it seems to be a 3-bit counter. But, if we look closely, after a reset ,value of count is 000 .The value of count is increasing on positive edge of clock but the Output q is simply following count[0] as per code and its simulation result. 
On synthesizing we get the optmized circuit , in which we can  that only on e flop is inferred for the code and the output of flop ,Q is count[0]. The input to the flop is compliment of output.
	
Thus we can say that, the logic which is no way related to primary output will be optimized by synthesis tool.
<img width="900" height="900" alt="Sequential_unused_output_opt" src="https://github.com/user-attachments/assets/335e1a66-5b3c-447e-80c7-ab7c370373cd" />

