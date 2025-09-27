# COMBINATIONAL LOGIC OPTIMIZATION
## opt_check.v
<img width="940" height="288" alt="image" src="https://github.com/user-attachments/assets/c7a057df-793e-4d36-bcea-1a48a144cdf7" />
 
#### To check the optimization commands to be followed after ` synth -top opt_check `
```
  opt_clean -purge   
  abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
<img width="622" height="220" alt="image" src="https://github.com/user-attachments/assets/ee416bd5-ce2d-40b6-946a-8593478fb38a" />
<img width="710" height="202" alt="image" src="https://github.com/user-attachments/assets/db1e47e9-0329-4b09-8072-a6e3bbe78498" />


## opt_check2.v
<img width="749" height="274" alt="image" src="https://github.com/user-attachments/assets/90dc8742-a0ac-4dfd-a97d-afbd590ced74" />
<img width="639" height="154" alt="image" src="https://github.com/user-attachments/assets/aad998c9-1e0d-45eb-a075-d003a99e72a7" />


## opt_check3.v
<img width="940" height="254" alt="image" src="https://github.com/user-attachments/assets/d0976f4a-ca94-43c0-933a-2857c2e7ac77" />
<img width="525" height="161" alt="image" src="https://github.com/user-attachments/assets/d9a12d77-e407-4329-b04a-aab274e8f90c" />
<img width="634" height="209" alt="image" src="https://github.com/user-attachments/assets/67ac012d-2519-4db4-8471-0109248f8def" />


## opt_check4.v
<img width="940" height="274" alt="image" src="https://github.com/user-attachments/assets/2c741680-bc17-4856-8e8a-1a3e70f31026" />
<img width="551" height="204" alt="image" src="https://github.com/user-attachments/assets/3399d384-058f-4182-8812-c53854f4cfd7" />
<img width="635" height="213" alt="image" src="https://github.com/user-attachments/assets/13925430-7cc4-4956-b9a6-2e66254060c8" />
<img width="471" height="331" alt="image" src="https://github.com/user-attachments/assets/6e829b8e-c7b5-4d21-8422-f005a3548487" />
<img width="474" height="289" alt="image" src="https://github.com/user-attachments/assets/e0c7e278-b5a5-4ffd-bdfb-ac8e26e0c301" />


## multiple_module_opt.v
<img width="940" height="466" alt="image" src="https://github.com/user-attachments/assets/a890a783-1f2d-4ca8-8e3c-3f8cf66fafdd" />
  flatten
  opt_clean -purge
  abc – liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
<img width="574" height="201" alt="image" src="https://github.com/user-attachments/assets/2af0bd35-fbd6-4b87-8070-525702e4ca72" />
<img width="636" height="679" alt="image" src="https://github.com/user-attachments/assets/586f5947-3072-4052-80c1-ab8fcfb77c53" />
<img width="574" height="679" alt="image" src="https://github.com/user-attachments/assets/4a1eed29-c8e5-4e8c-9ba9-b046e25abc13" />


# SEQUENTIAL LOGIC OPTIMIZATION
## dff_const1.v
<img width="940" height="383" alt="image" src="https://github.com/user-attachments/assets/34b7870b-e36c-4ffc-93fc-1093dc4f2fd1" />
<img width="940" height="219" alt="image" src="https://github.com/user-attachments/assets/9ac1f319-ca1a-43fc-8292-e6c8cfa0cab3" />
Tool inferred a d Ff after synthesis
<img width="497" height="324" alt="image" src="https://github.com/user-attachments/assets/54f258bb-677c-48c1-b272-ff4a74df8ae3" />
After mapping to standard cell library
<img width="520" height="169" alt="image" src="https://github.com/user-attachments/assets/09169b0b-ceb6-4e2a-95cf-acb2444111e5" />
<img width="940" height="245" alt="image" src="https://github.com/user-attachments/assets/875fd064-c215-4122-8551-5169f7748614" />

## dff_const2.v

<img width="940" height="365" alt="image" src="https://github.com/user-attachments/assets/6e98d5a3-2fd2-45ae-9019-4a3b340a3eff" />
<img width="940" height="219" alt="image" src="https://github.com/user-attachments/assets/9a66926c-5494-469e-b0d9-73c3b140ad5c" />
<img width="656" height="390" alt="image" src="https://github.com/user-attachments/assets/a59f8226-fcaf-43f4-ae16-a8794f0b5b37" />
NO FLOP INFERRED – Sequential Constant Optimization
<img width="636" height="459" alt="image" src="https://github.com/user-attachments/assets/7238b3c2-713e-4e3d-811b-c0b9a345b0d8" />





## dff_const3.v

<img width="940" height="493" alt="image" src="https://github.com/user-attachments/assets/959f6191-b54c-4eea-abdf-5616b1298dc5" />
<img width="940" height="218" alt="image" src="https://github.com/user-attachments/assets/8a90e6f0-674d-480d-b653-a23679d9d704" />
<img width="940" height="226" alt="image" src="https://github.com/user-attachments/assets/7fdf9d41-a7b9-4929-b62a-b3734b11dbcc" />
<img width="641" height="379" alt="image" src="https://github.com/user-attachments/assets/277de90d-1997-42e8-bc64-b2778b48823a" />
<img width="555" height="157" alt="image" src="https://github.com/user-attachments/assets/c31993d0-10e7-4006-95df-d6a77a868a88" />
<img width="940" height="237" alt="image" src="https://github.com/user-attachments/assets/c058749a-7bf0-473f-846b-7b954c57b7ef" />

## dff_const4.v

<img width="940" height="492" alt="image" src="https://github.com/user-attachments/assets/59b0d6b8-3511-40ed-9551-daec3c5ac904" />
<img width="940" height="214" alt="image" src="https://github.com/user-attachments/assets/e1bcfd66-4fc4-4d2e-8ff8-fd2057ba1049" />
<img width="940" height="217" alt="image" src="https://github.com/user-attachments/assets/b7b31ea5-6837-48ed-92ec-b8396bc757ec" />
<img width="645" height="343" alt="image" src="https://github.com/user-attachments/assets/da186d3e-9169-4223-ad91-ad055e730d62" />
<img width="624" height="556" alt="image" src="https://github.com/user-attachments/assets/9e477883-351a-43da-a608-64082efefb5b" />

## dff_const5.v

<img width="940" height="508" alt="image" src="https://github.com/user-attachments/assets/cee6ddb4-460f-4c7e-ac61-bd7f940f2bcf" />
<img width="940" height="240" alt="image" src="https://github.com/user-attachments/assets/dc9a94c3-04f8-40a1-91c3-7ac91adf2f42" />
<img width="940" height="219" alt="image" src="https://github.com/user-attachments/assets/5dc08fbd-9bb5-44c9-8811-5a18fd5fc9a5" />
<img width="636" height="355" alt="image" src="https://github.com/user-attachments/assets/9139f05e-e399-42a6-8b24-4e1232ffe811" />
After dfflibmap and abc command
<img width="511" height="150" alt="image" src="https://github.com/user-attachments/assets/24178baa-5c25-4755-8c16-7dfaa5aceecd" />
<img width="940" height="178" alt="image" src="https://github.com/user-attachments/assets/6a8421e2-c02e-4701-a9c6-ae906f313ac9" />


## OPTIMIZATION OF UNUSED STATES

### counter_opt.v

<img width="940" height="444" alt="image" src="https://github.com/user-attachments/assets/4701bf76-0bfa-41de-89d7-bbc05fa37862" />
<img width="671" height="383" alt="image" src="https://github.com/user-attachments/assets/bf832f32-e26a-4f01-a1c5-cf09bb45efcc" />
<img width="611" height="151" alt="image" src="https://github.com/user-attachments/assets/45629374-7473-4104-a71e-a46449e323b3" />
<img width="940" height="165" alt="image" src="https://github.com/user-attachments/assets/50e762f3-e853-4764-a6a6-c3131fceff3b" />
<img width="340" height="348" alt="image" src="https://github.com/user-attachments/assets/ad22006a-e2e8-4517-a34e-82b918986d92" />


### counter_opt2.v

<img width="940" height="448" alt="image" src="https://github.com/user-attachments/assets/71706a10-6bd4-4809-9f13-135bed02f6c2" />
<img width="940" height="255" alt="image" src="https://github.com/user-attachments/assets/1009e203-c928-4644-a1f0-65774f82a5a2" />
