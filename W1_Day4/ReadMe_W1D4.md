# 🌟 RISC-V Reference SoC Tapeout Program VSD IITGN

## DAY 4 of WEEK 1 of 20 WEEK PROGRAM

### 🔰 DAY 4: TOPICS -----
- [Day4- GLS, blocking vs non-blocking and Synthesis-Simulation mismatch](#GLS-,-blocking-vs-non---blocking-and-Synthesis---Simulation-mismatch)
   1. [GLS concepts and flow using iverilog](#GLS-concepts-and-flow-using-iverilog)
   2. [GLS setup using iverilog ](#GLS-setup-using-iverilog)
   3. [Synthesis-Simulation Mismatch](#Synthesis---Simulation-Mismatch)
      * [Missing Sensitivity list](#Missing-Sensitivity-list)	
      * [Blocking and Non-blocking assignments in verilog](#Blocking-and-Non---blocking-assignments-in-verilog)


	
# 4. Day4 - GLS, blocking vs non-blocking and Synthesis-Simulation mismatch
	
## 4.1 GLS concepts and flow using iverilog :
	
**Gate Level Simulation(GLS)** : Gate level simulation is used to boost the confidence regarding implementation of a design and can help verify dynamic circuit behaviour, which cannot be verified accurately by static methods. It is a significant step in the verification process.
Gate level simulation overcomes the limitations of static-timing analysis and is increasingly being used due to low power issues, complex timing checks, design for test (DFT) insertion at gate level and low power considerations.

The term "gate level" refers to the netlist view of a circuit, usually produced by logic synthesis. So while RTL simulation is pre-synthesis, GLS is post-synthesis. The netlist view is a complete connection list consisting of gates and IP models with full functional and timing behavior.
In this test bench is run with "Synthesized Netlist" as Desing under test(DUT). As, the netlist is logically same as the RTL code, same test bench can be used whic is used for RTL simulation.

* Why gate level simulation is done?
It is done for the following reasons:
	* To verify the logical correctness of the design after synthesis.
	* To ensure the timing of the design is met. For this GLS need to run with delay annoatation(Timing aware GLS).

## 4.2 GLS setup using iverilog :
Below image show the inputs to the iverilog tool and output for Gate Level Simulation:

<img width="800" height="400" alt="GLS_Setup" src="https://github.com/user-attachments/assets/a522b120-05ec-457f-ba80-4b93f6cdfe81" />


 
**Gate level verilog model** : It is one of the input to iverilog. It is used to tell iverilog about the standard cell models used in generated netlist after synthesis. The gate level verilog model can be :
* Functional : It can validates the functionality of the design alone.
* Timing aware : It can validate functinality and can ensure timing both.
	
## 4.3 Synthesis-Simulation Mismatch :
As we know, the generated netlist is the true representation of the RTL design, still we need to validate the functionality of the netist. This is because of the synthesis-simulation mismatch. This can happen because of the following reasons:
* Missing sensitivity list
* Blocking vs Non-Blocking assignment
* Non-Standard verilog coding

### 4.3.1 Missing Sensitivity list :
let us understand this with example 'ternary_operator_mux.v' and do RTL simulation , synthesis and gate-level simulation.

**RTL Simulation:**
```
$ iverilog ternary_operator_mux.v tb_ternary_operator_mux.v
$ ./a.out
$ gtkwave tb_ternary_operator_mux.vcd	
```
	
**Synthesis**
```
$ read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
$ read_verilog ternary_operator_mux.v
$ synth -top ternary_operator_mux
$ abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
$ write_verilog -noattr ternary_operator_mux_net.v			
```
	
**Gate_level Simulation**
```
$ iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
$ ./a.out
$ gtkwave tb_ternary_operator_mux_net.vcd	
```

<img width="800" height="600" alt="good_mux" src="https://github.com/user-attachments/assets/7484fc86-2215-444a-b8f9-8318ccd02061" />



  
In the above image we can see that the RTL simulation output waveform and synthesized netlist gate level simulation output waveform have similar behavior, so here there is no issus if missing sensitivity list.

Now, take a look of below example of "bad_mux.v" :
<img width="800" height="600" alt="bad_mux" src="https://github.com/user-attachments/assets/bd4895d2-d978-4d5a-be20-389edde10b18" />



In this we can see that the  RTL simulation output waveform and synthesized netlist gate level simulation output waveform do not have similar waveform. This is the because of problem of "missing sensitivity list". 
In the RTL simulation , we can see that always block is evaluated only when 'sel' is changing ans is independent of change in inputs(i0,i1). Thus as output is not evaluated for change in inputs, when we do RTL simulation ,simulator will infer the mux as latch. So this comes under problem of "missing sensitivity list".

To solve this issue we can write always block as : always(*)
So, always block will be evaluated when any if the inputs i0, i1, sel change.
	
But we can also see that when synthesis tool evaluate the same RTL code and the gate level simulation is done. 
The output waveform shows the MUX behaviour. Thus systhesis tool has inferred the same code as mux.
This is a problem of synthesis- simulation mismatch and that is the main reason to perform GLS.
	
### 4.3.2 Blocking and Non-blocking assignments in verilog :
Blocking and Non-blocking statements come into picture when we are using "always" block. 

**Blocking statements** : 
* Inside always block , if we are using "equal to "(=) to make assignments, the assignment is called as blocking statement.
* Blocking statements execute the statements in the sam eorder they are written.
* So the first statement is evaluated first ,then second and so on. Thus behaviour of such statements is sequential.


**Non-Blocking statements** : 
* Inside always block, the non- blocking assignment is done using "less than equal to"(<=).
* It executes all the RHS when entered in always block and assign it to LHS.
* All the statements are evaluated in parallel, so their order does not matter.
* Thus, the evaluation of staements is done parallely.									   
	
**Caveats with Blocking Statements** :
lets understand the cavest in blocking statements using an example :	

<img width="800" height="600" alt="cavet_blocking" src="https://github.com/user-attachments/assets/140367e0-5ebc-4bde-bbd9-5720f5db5e86" />


											 
We can see the in the above case the RTL simulation, the latch behaviour is inferred by simulator. This is because the assignmenst are blocking assignment. Thus first value of 'd' is evaluated which will use old value of 'x' as the value of 'x' is evaluated after the first statement.
