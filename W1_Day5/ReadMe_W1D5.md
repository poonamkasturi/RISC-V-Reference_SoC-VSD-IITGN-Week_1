# 🌟 RISC-V Reference SoC Tapeout Program VSD IITGN

## DAY 5 of WEEK 1 of 20 WEEK PROGRAM

### 🔰 DAY 3: TOPICS -----

- [Day5 - If, case, for loop and for generate](#Day5---If-,-case-,-for-loop-and-for-generate)
   1. [If statements](#If-statements)
   2. [CASE statements](#CASE-statements)
      * [Caveats with CASE statements](#Caveats-with-CASE-statements)
   3. [For loop and For generate constructs](#For-loop-and-For-generate-constructs)
      * [For loop](#For-loop)
      * [For generate](#For-generate)

											   

# 5. Day5 - If, case, for loop and for generate
'If' and 'case'; staementa are used inside the 'always' block, so the variables to which we are going to assign output value in both statements should be a register variable.
	
## 5.1 If statements
Syntax:
```
if <condition_1>		: it has first / highest priority among all statements
	<statements>
else if <condition_2>		: it has it has second highest priority 
	<statements>
else 
	<statements>
```
If the condition evaluates to true (i.e. any non-zero value), all statements within that particular if block will be executed. All the 'if-else' statements are mutually exclusive.
		
If statement is mainly used to create priority logic in terms of hardware. It can infer a MUX or priority logic based on how its coded.

** Danger/Caution with 'If' constructs** :
If "If statements" are not written correctly, it may infer latches. "Inferred latches" comes because of bad code style. The main reason for inferred latches is "incomplete if statements".
Below code show the example of "incomplete if statements" :
```
module incomp_if (input i0 , input i1 , input i2 , output reg y);
always @ (*)
   begin
       if(i0)
           y <= i1;
   end
endmodule
```
In above code,we can see that the if statement is not complete as else part is missing. As per code when i0 is 1 y is i1,It does not tell what will be the output if i0 is low, its not defined in the code clearly. So the synthesis tool will assume that if i0 is 0 , then y should retain its previous value ,which represents latch is inferred. We did not intend a latch while writing code but a latch is inferred in the circuit. "If statements" creates combinational circuit, but due the bad coding style(incomplete if statements), have sequential element latch is inferred. In combinational circuit we can not have inferred latches.
		  
## 5.2 CASE statements
The case statement checks if the given expression matches one of the expressions in the list and branches accordingly. It typically infers MUX. In comparison to if statement, if there are many conditions to check ."if-else " construct may not be suitable and would synthesize the priority encoder instead of multiplexer. Thus, when large number conditions are required to check "case" is better option to choose.

### 5.2.1 Caveats with CASE statements:
* Incomplete case statement :
lets understand this caveat with exmaple "incomp_case.v".

<img width="700" height="500" alt="incomp_case" src="https://github.com/user-attachments/assets/2fe6e035-1577-4d1b-8748-47a3709a8b91" />



From the above image, we can see that all the possible cases for 2-bit 'sel' is not defined in RTL code, that shows incomplete case statement. Beacuse of this ,the output will infer latch in case when sel = 10, 11. So, we can also say when sel[1] is 1, there will be latching action which can be seen from RTL code simulation waveform. The output 'y' is not changing in accordance to input for sel= 10, 11. 
From the synthesized netlist also, we can find out that the synthesizer has inferred latch for the RTL code.

To avoid inferring latches with case, use case statements with 'default' case in the code, which will cover all other cases not mentioned in case statement. However, using defalut case would not always avoid inferring latch in case of 'partial assignment cases'.
		  
		  
* Partial case assignment :		  
lets understand this caveat with exmaple "partial_case_assign.v".

<img width="700" height="500" alt="Partial_case_1" src="https://github.com/user-attachments/assets/1ef32b0d-72db-42c1-9c02-639d00d19d5c" />

<img width="700" height="500" alt="Partial_case_2" src="https://github.com/user-attachments/assets/7e44dab6-65bb-44fa-9778-a023260f4f75" />





From the RTL code , we can find out that the output 'x' is not assigned any value for sel = 01 and from the simulation we can figure out that the output waveform of 'x' is inferring latch for sel = 01. While output 'y' is inferring mux which is expected as per code. Thus a latching behaviour is inferred for output 'x' only.

From the synthesized netlist also we can see that synthezier has inferred latch for output 'x' only. 
To avoid inferrin latches in such cases, assign all the outputs in all the segments of case statements.
		  
* Overlapping case assignment :
lets understand this caveat with exmaple "bad_case.v".

<img width="700" height="500" alt="baad_case_1" src="https://github.com/user-attachments/assets/d7de6837-29b1-4136-8b46-494bc4f8817e" />

<img width="700" height="500" alt="baad_case_2" src="https://github.com/user-attachments/assets/4b475e12-1972-4e8d-b729-c60d61bbf1f4" />




From the RTL code, we can see that last case statement has condition ```sel= 2'b1? ```. This case is overlapping case ``` sel = 2'b10 ```. Thus this cse confuses the simulator causing the synthesis-simulation mismatch. This create overlapping case and different simulator shows different behaviour for such case. The tool will give unpredictable ouput for overlapping case statements.

From the synthesized netlist, we can find out that no latch is inferred for overlapping case and the GLS shows correct output behaviour as expected.
Thus overlapping case statements create synthesis-simulation mismatch and to avoid this , all the statements of case statement should be mutually exclusive which is a correct way of coding.

## 5.3 For loop and For generate constructs :
### 5.3.1 For loop :
* It is used inside the 'always' block.
* It is used for evaluating expressions.
* For loop is not used for instantiating hardware, gates.

lets understand this with example "mux_generate.v": 

<img width="700" height="500" alt="For_loop_1" src="https://github.com/user-attachments/assets/2d80a14c-309f-4c13-bf1f-f9801f9f6558" />

<img width="700" height="500" alt="For_loop_2" src="https://github.com/user-attachments/assets/fee7a077-dfbd-41f7-a190-ddc29763f3a9" />




we can see that the ouput waveform for RTL code functional simulation and synthesized netlist GLS are same. This is implemetation of small size mux, but we can make large size mux simply using same code except that we only need to change the input bus size and number fo times loop runs.	If we use 'case' statements for same size mux, it will not be difficult but when we increase the size of MUX, the implementation using 'case' statements will become cumbersome which can be built using "for-loop " easily.
		  		  
		  
### 5.3.2 For generate :
* It is used outside the 'always' block.
* It can not be used inside 'always' block.		  
* It is used for instantiating hardware multiple times.

lets understand this with example "rca.v": 		  

<img width="700" height="500" alt="Generate_1" src="https://github.com/user-attachments/assets/1507027c-e2d3-4a82-9ca7-da05ab3d02bd" />

<img width="700" height="500" alt="Generate_2" src="https://github.com/user-attachments/assets/69b8061a-893f-44d0-bbd1-458cbfb0f3ce" />

<img width="700" height="500" alt="Generate_3" src="https://github.com/user-attachments/assets/c40e4d7c-6db3-4b6c-8280-2881cb53f4bf" />


The above images shows that the ouput waveform for RTL code functional simulation and synthesized netlist GLS show similar behaviour. Here the code has implemented 8-bit ripple carry adder using "for-generate" statement. Using "for-generate" helps us to replicate the hardware easily when we need same hardware to repeat large number of times, else we have to instantiate each hardware individually which will be cumbersome unlike number of times the hardware replication is required is small.
		  
