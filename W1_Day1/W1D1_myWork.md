## Checking the library and file paths
<img width="940" height="204" alt="image" src="https://github.com/user-attachments/assets/a4a46720-083b-4e17-92a3-3dcdd4fe3085" />
<img width="940" height="258" alt="image" src="https://github.com/user-attachments/assets/f583b0df-e513-4d9a-ad73-7e16e58d07b0" />
<img width="940" height="307" alt="image" src="https://github.com/user-attachments/assets/409043b8-fc43-402e-a0da-cad49fcb1a89" />
<img width="940" height="431" alt="image" src="https://github.com/user-attachments/assets/b2a80f70-be5c-4a81-ba5b-a7ee7c263e48" />
<img width="940" height="651" alt="image" src="https://github.com/user-attachments/assets/e38ea66e-b459-4937-af44-d6cdd6aedd44" />
<img width="940" height="168" alt="image" src="https://github.com/user-attachments/assets/38383c40-98b0-490c-8237-c5ed40e46b6b" />

## Lab 1 : GOOD MUX ..
### Sel = 1  
<img width="940" height="465" alt="image" src="https://github.com/user-attachments/assets/806cd077-c165-4925-aaff-b55d570d4574" />

### Sel = 0   
<img width="940" height="474" alt="image" src="https://github.com/user-attachments/assets/de364891-cf81-4644-a29c-6bf5969b732a" />

### Invoke Yosys
**Read the liberty file**
<img width="940" height="722" alt="image" src="https://github.com/user-attachments/assets/f2c316ca-2c7b-4fb5-94f8-49db3d860736" />
**Read Design (Verilog file) to be synthesized**
<img width="940" height="266" alt="image" src="https://github.com/user-attachments/assets/1428ed14-869d-4c96-a5ea-a45260deffd6" />
**Synthesize the top module**
<img width="940" height="163" alt="image" src="https://github.com/user-attachments/assets/9bf6106a-b175-453a-841b-183571322341" />
**Result window of synth command**   
abc command – To Link the liberty file with the design to be synthesized 
<img width="940" height="706" alt="image" src="https://github.com/user-attachments/assets/c099d5b0-3c28-4752-8f93-5c900c21d421" />
**Standard cells inferred by abc command**
<img width="940" height="156" alt="image" src="https://github.com/user-attachments/assets/53be46f6-b1e4-4fdb-9f47-500603ec7544" />
**To generate a graphical view of the netlist generated**
<img width="940" height="150" alt="image" src="https://github.com/user-attachments/assets/dccbe976-c033-494e-99e7-fc4bad26fe7f" />
**Alternate at times required is**      
Yosys> show multiple_modules
<img width="940" height="522" alt="image" src="https://github.com/user-attachments/assets/8c0065be-e877-45a9-b1a8-1a402a3f94be" />
**Write the netlist generated to  .v file**
!gvim - open the file to read in Vim module
<img width="940" height="276" alt="image" src="https://github.com/user-attachments/assets/51b6c915-c033-491c-85c5-ad3d64bad2cd" />
<img width="940" height="584" alt="image" src="https://github.com/user-attachments/assets/df2a0a5e-00c6-473c-b6af-2eb4a63a977a" />
**For a more cleaner readability .. the .v file is created with -noattr – No Attributes**
<img width="940" height="188" alt="image" src="https://github.com/user-attachments/assets/f99d096f-83b3-44ea-a888-959746d7ffb9" />
<img width="940" height="657" alt="image" src="https://github.com/user-attachments/assets/3d599d1a-a534-475d-8cc7-960296ccfda7" />
