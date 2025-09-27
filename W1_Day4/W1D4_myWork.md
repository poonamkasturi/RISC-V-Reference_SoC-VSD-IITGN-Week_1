
## RTL vs Gate-Level Simulation
To validate the correctness of the synthesized netlist, 
it's essential to correlate the RTL (Register Transfer Level) design with the Gate-Level design. 
This is typically done by running the same stimuli (testbench) on both RTL and GLS (Gate-Level Simulation). 
If the outputs match, it confirms that the netlist faithfully implements the intended design.

> This step is crucial for ensuring functional equivalence
> before moving on to further stages like timing analysis or physical design.

<img width="600" height="400" alt="image" src="https://github.com/user-attachments/assets/60cba8cf-2c90-44b9-8032-177926a9ccab" />

Here we are doing only functional equivalence check and not the timing equivalence

`  Example Considered : 2 x 1 MUX  through ternary operator  `

### RTL definition
<img width="600" height="300" alt="image" src="https://github.com/user-attachments/assets/9554ef2e-8b4c-4564-a3c3-03d005688843" />

### Simulated waveforms for the RTL design of the ternary operator (implements a mux)
<img width="800" height="300" alt="image" src="https://github.com/user-attachments/assets/93efd2fa-3454-40a1-9826-cf1c0c39b4f0" />

### GLS Simulation output
<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/79603a7d-1246-4d77-adf8-d45933b0dc36" />

`    The RTL and GLS simulation match indicating the correct functionality of the design   `


### Synthesized Design
<img width="400" height="250" alt="image" src="https://github.com/user-attachments/assets/decb6172-b55c-494b-8306-0eb423f62cc4" />


### netlist of the eample design
<img width="500" height="400" alt="image" src="https://github.com/user-attachments/assets/91eeefa3-9226-4f4c-998f-c84201acf8c9" />

---

## SIMULATION - SYNTHESIS MISMATCH
## 
EXAMPLE 1: BAD MUX DESIGN TO HIGHLIGHT SIMULATION SYNTHESIS MISMATCH due to incomplete sensitivity list
#### ........................................RTL MODULE..............
<img width="600" height="300" alt="image" src="https://github.com/user-attachments/assets/1705a00e-0444-4b59-9e42-281cbdf01f9f" />

#### ........................................SYNTHESIZED DESIGN..............
<img width="300" height="300" alt="image" src="https://github.com/user-attachments/assets/a17748c6-b8b9-4b11-b8a4-18ea5b77bd21" />

#### ........................................NETLIST..............
<img width="700" height="450" alt="image" src="https://github.com/user-attachments/assets/7b537a45-b96d-424c-8154-8c62c2748ad1" />

#### ........................................RTL SIMULATION..............
<img width="700" height="300" alt="image" src="https://github.com/user-attachments/assets/7683c5f1-a687-4b4d-828d-e90217a70e24" />

#### ........................................GLS SIMULATION..............
<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/170322ba-e04c-42c8-8a2e-f79dda24c319" />

```
The comparison of RTL and GLS simulated waveforms clearly highlight the
* SIMULATION - SYNTHESIS MISMATCH
* This mismatch is due to the incompeletly specified sensitivity list
* The RTL simulation is not able to capture the changes in the inputs
                         - as they are missing in the sensitivity list
* whereas the synthesized design is able to capture
                         - as it is the actual physical gate level simulation
```
---
## 
EXAMPLE 2: TO HIGHLIGHT SIMULATION SYNTHESIS MISMATCH due to blocking statements caveat
#### ........................................RTL MODULE..............
D = (a and b ) or c
<img width="872" height="294" alt="image" src="https://github.com/user-attachments/assets/77e399a7-202d-4e29-94e5-e52ad4684b6d" />

#### ........................................RTL SIMULATION..............
<img width="940" height="371" alt="image" src="https://github.com/user-attachments/assets/e77f281d-7688-4102-9dde-e63d8c417a2c" />
<img width="940" height="374" alt="image" src="https://github.com/user-attachments/assets/c9d455a7-96da-40db-8672-0a06db720faa" />

##


> RTL simulation shows incorrect value at the cursor point (a or b) and c = 1
> since it considers the previous values on OR gate output due to the blocking assignment operator used and the sequence.

#### ........................................GLS SIMULATION..............
<img width="940" height="413" alt="image" src="https://github.com/user-attachments/assets/3963dcc8-eaee-4c0e-a3ea-bd6dfce227fb" />

> GLS simulation shows correct value at the cursor point (a or b) and c = 0 AS DESIRED


#### ........................................SYNTHESIZED DESIGN.............
<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/582e0f3f-5301-4137-8866-c0d0b7059956" />

#### ........................................NETLIST.............
<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/29d92095-7861-41ee-b74f-091baaf4d4c5" />









