## DAY 5: 
     ➡️ Incomplete If - EXAMPLE 1, 2
     ➡️ Incomplete Case - EXAMPLE 3, 4, 5, 6
     ➡️ For and Generate

##

### EXAMPLE 1 : Incomplete if statement

### .........................RTL DESIGN..................
<img width="700" height="235" alt="image" src="https://github.com/user-attachments/assets/86ebd37f-9211-4e50-bfd8-571b68f0dad1" />

### .........................RTL SIMULATION..................
<img width="700" height="370" alt="image" src="https://github.com/user-attachments/assets/514c41df-37ca-492a-8679-490d410c03ba" />
<img width="700" height="216" alt="image" src="https://github.com/user-attachments/assets/ebf6047c-481f-4c86-a1e4-1d10708281bf" />

##

`   Output is latched to previous value when i0 goes low   ` 

### .........................SYNTHESIZED DESIGN..................
<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/25862a96-eaf2-4928-92f2-a0af5ba5dff3" />

## 

`  A D Latch is inferred due to incomplete if statement   `

### .........................NETLIST.................
<img width="800" height="350" alt="image" src="https://github.com/user-attachments/assets/c104d546-3650-4a30-99fb-4e7fd6889bdf" />

###
---
##

### EXAMPLE 2 : Incomplete if statement
##

### .........................RTL DESIGN..................
<img width="600" height="350" alt="image" src="https://github.com/user-attachments/assets/94d699ed-88da-4f25-9602-5d47eed77eb1" />

### .........................RTL SIMULATION.................
<img width="800" height="300" alt="image" src="https://github.com/user-attachments/assets/b9d50693-704a-4d63-8ab3-09549fab44b1" />
<img width="800" height="300" alt="image" src="https://github.com/user-attachments/assets/3e60e494-e68c-4808-97d8-a86cfd293ceb" />

```
Whenever i0 is high – output is following i1
i0 and i2 is low output is constant
i0 low i2 is high – y follows i3
```

### .........................SYNTHESIZED DESIGN.................
<img width="600" height="500" alt="image" src="https://github.com/user-attachments/assets/23e6fb35-c7f4-48d1-85cc-cf76c3ae9481" />

## 

`  A D Latch is inferred due to incomplete if statement   `

### .........................NETLIST.................
<img width="800" height="700" alt="image" src="https://github.com/user-attachments/assets/be405077-5317-413c-8469-a4f722a2837e" />


---
##

### EXAMPLE 3 : Incomplete OVERLAPPING case statement
##

### .........................RTL DESIGN.................
<img width="600" height="500" alt="image" src="https://github.com/user-attachments/assets/10f460d1-c461-480d-b478-bad16c5a371c" />


### .........................RTL SIMULATION.................
<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/24edbca1-ebca-40ca-bee3-c08d509b047a" />
Incomplete case
i0i1 = 00 y = i0
i0i1 = 01 y = i1
i0i1 = 10 y = latch to previous value of y
i0i1 = 11 y = latch to previous value of y

### .........................SYNTHESIZED DESIGN.................
<img width="800" height="600" alt="image" src="https://github.com/user-attachments/assets/fc946fdf-6c3c-428a-a2ec-06f487d3da51" />


### .........................NETLIST.................
<img width="700" height="800" alt="image" src="https://github.com/user-attachments/assets/30a9eb96-414b-4484-aaaa-f3c47f4e68fa" />
<img width="300" height="400" alt="image" src="https://github.com/user-attachments/assets/9abb2716-5d73-4028-bb78-a6050915445c" />

---
##

### EXAMPLE 4 : Complete OVERLAPPING case statement
##
use of default eliminates the latches that were inferred in the incomplete overlapping case (EXAMPLE 3)
##

### .........................RTL DESIGN.................
<img width="700" height="400" alt="image" src="https://github.com/user-attachments/assets/924fb4e9-25dd-475e-8954-88621d7bdbc9" />


### .........................RTL SIMULATION.................
<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/16533080-558b-47f4-abfa-bb41dcdf34d6" />

```
OUTPUT FOLLOWS PROPER LOGIC…..THERE IS NO LATCHING OPERATION
Sel 00 , y = i0
Sel 01, y = i1
Sel 10,11. Y = default to i2
```

### .........................SYNTHESIZED DESIGN.................
<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/ae7b08d0-f7e2-459d-b1ab-27969b712798" />

> No Latch inferred 
> Only combinational Logic due to default – that completes the case statement

### .........................NETLIST.................
<img width="700" height="800" alt="image" src="https://github.com/user-attachments/assets/c21763f1-4016-46f0-97dc-21f145b420d5" />
<img width="300" height="300" alt="image" src="https://github.com/user-attachments/assets/bc631e4f-8e1e-4d76-99ee-effe944c8c21" />

---
##

### EXAMPLE 5 : PARTIAL case statement
##

### .........................RTL DESIGN.................
<img width="600" height="400" alt="image" src="https://github.com/user-attachments/assets/e4e325b2-8893-43d1-9898-e1ad5ade191f" />


### .........................RTL SIMULATION OUTPUT.................
<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/267f2b41-e8b0-43ca-9b81-740a94196f13" />

```
Sel 00 , x = i0   , y = i0
Sel 01, x = latch   . y = i1
Sel 10, x = i1 ,  y = i2
Sel 11, x = i1 ,  y = i2
```

### .........................SYNTHESIZED DESIGN.................
<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/9cb082a7-a2d8-4558-bce6-0b365a0cf070" />


### .........................NETLIST................
<img width="800" height="800" alt="image" src="https://github.com/user-attachments/assets/7a1752b6-decd-4aca-915b-9e3942276c21" />
<img width="350" height="800" alt="image" src="https://github.com/user-attachments/assets/971f3dad-2752-48fe-bb81-d0aee7bfc6a3" />
<img width="350" height="350" alt="image" src="https://github.com/user-attachments/assets/bf86682f-f974-449c-b200-32d3ea8901ed" />

---
##

### EXAMPLE 6 : BAD case statement
##

### .........................RTL DESIGN.................
<img width="700" height="300" alt="image" src="https://github.com/user-attachments/assets/1b40ebf2-ad07-4734-a6bd-bdc35afa1513" />

### .........................RTL SIMULATION.................
<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/624424f2-14b3-4865-84fc-082625eb0ac8" />

> The tool is getting confused at state 11 and hence is latching on to a value

### .........................SYNTHESIZED DESIGN.................
<img width="700" height="400" alt="image" src="https://github.com/user-attachments/assets/76f6cd1f-0073-4d38-a6a1-8504d3f287ac" />

### .........................NETLIST................
<img width="700" height="800" alt="image" src="https://github.com/user-attachments/assets/ad2013d3-a45c-4a7d-9f89-557f2e000b5f" />
<img width="350" height="300" alt="image" src="https://github.com/user-attachments/assets/c917b1ec-1279-4225-b757-d8638b497fd1" />


### .........................GLS SIMULATION.................
<img width="940" height="266" alt="image" src="https://github.com/user-attachments/assets/a3e944e8-0c69-4719-8372-3bb22b1770c2" />
> GLS does not latches whereas RTL simulation latches ……it is a case of SIMULATION – SYNTHESIS MISMATCH


---
##

### EXAMPLE 7 : MUX GENERATE : for statement
##

### .........................RTL DESIGN.................
<img width="700" height="300" alt="image" src="https://github.com/user-attachments/assets/e726e3cd-be8d-473f-8422-18dce420da33" />

### .........................RTL SIMULATION................
<img width="700" height="400" alt="image" src="https://github.com/user-attachments/assets/f904bcfd-656f-4c57-86ec-fddcd68aa5bd" />
Behaves like a 4x1 mux

### .........................SYNTHESISED DESIGN.................
<img width="700" height="630" alt="image" src="https://github.com/user-attachments/assets/1b806d47-7958-45f0-a2f5-cfba6943e52a" />

---
##

### EXAMPLE 8 : DEMUX CASE : case statement
##
### .........................RTL DESIGN.................
<img width="940" height="442" alt="image" src="https://github.com/user-attachments/assets/48d55681-78a1-4ded-8c60-3622db7bc739" />

### .........................RTL SIMULATION.................
<img width="940" height="349" alt="image" src="https://github.com/user-attachments/assets/27e5827f-5618-45ac-b887-771c4b2386c3" />

### .........................SYNTHESISED DESIGN.................
<img width="940" height="818" alt="image" src="https://github.com/user-attachments/assets/06f06454-365c-4f96-bf4c-06e8071b79b0" />

### .........................GLS SIMULATION.................
<img width="940" height="330" alt="image" src="https://github.com/user-attachments/assets/957d2991-11c2-46f9-96de-e9bb6312c183" />

GLS and RTL SIMULATIONS MATCH

---
##

### EXAMPLE 9 : DEMUX  : for statement
##

### .........................RTL DESIGN.................
<img width="940" height="367" alt="image" src="https://github.com/user-attachments/assets/4922644b-5911-4bde-92aa-e14ab812a2b2" />

### .........................RTL SIMULATION.................
<img width="940" height="343" alt="image" src="https://github.com/user-attachments/assets/52cdbd42-828a-4a7e-8415-dbba5e42d2ec" />

### .........................SYNTHESISED DESIGN.................
<img width="940" height="834" alt="image" src="https://github.com/user-attachments/assets/6421442d-337f-4f10-859f-09c3c2d6acda" />

Same response for simulation and sunthesised designs is received for both implementation for demux - case and for statements


