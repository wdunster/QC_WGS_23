---
layout: post
title: E.Z.N.A. Mollusc DNA Kit
categories: Protocol QC WGS
tags: 
---

# E.Z.N.A. Mollusc DNA Kit

## Contents:
- [**Preparing Reagents**][#prep]    
- [**Digestion**](#dig)     
- [**Separating DNA**](#sep)      
- [**Precipitating DNA**](#prec)    
- [**Clean Up**](#clean)
- [**Quantify**](#quant)

## <a name="prep"></a> **Preparing Reagents**

All reagents are aliqoted to reduce the chance of cross contamination

**Reagents**

Kit includes: 

- HiBind DNA Mini Columns
- 2 mL Collection Tubes
- ML 1 Buffer
- BL Buffer
- Proteinase K Solution
- RNase A
- DNA Wash Buffer
- Elution Buffer

Supplied by PRADA Lab: 

- 100% Ethanol
- 100% isopropanol
- Chloroform:isoamyl alcohol (24:1)
- 3M NaOH (1M solid --> 3M liquid)


**Reagent Prep**

- Dilute Buffers 

| Reagent         | Dilution                 | 
|-----------------|--------------------------|
| HBC Buffer      | 32mL 100% isopropanol    | 
| DNA Wash Buffer | 100mL / bottle 100% EtOH | 


- Aliqot reagents 

| Reagent                    | 1 rxn (uL) | 8 rxn (uL) | 10% error | Total aliquot (uL)|
|----------------------------|------------|------------|-----------| ------------------|
| ML1 Buffer                 | 350 uL     | 2800       | 280       | 3080              |
| Proteinase K               | 25 uL      | 20         | 20        | 220               |
| Chloroform:isoamyl alcohol | 350 uL     | 2800       | 280       | 3080              |
| BL Buffer                  | 1 vol      | 2800       | 280       | 3080              |
| RNase A                    | 10 uL      | 80         | 8         | 88                |
| 100% EtOH                  | 1 vol      | 2800       | 280       | 3080              |
| 3M NaOH                    | 100 uL     | 800        | 80        | 880               |
| HBC Buffer                 | 500 uL     | 4000       | 400       | 4400              |
| DNA Wash Buffer            | 2x 700 uL  | 1120       | 1120      | 12,320            |
| Elution Buffer             | 50-100 uL  | 800        | 80        | 880               |



## <a name="dig"></a> **Digestion**

Day 1:


Prep: 

- Molecular water and a beaker for washing samples in DMSO
- n scalpal blades
- n 1.5 mL microcentrifuge tubes
- preheat bead bath to 56C


Steps: 

1. Submerge samples preserved in DMSO in Pure Molecular Water to remove salt

2. Skip homogenization step as QC tissue is very muscular and will not present the issue typically associated with mollusc DNA

3. Using a scalpal, cut a small 2x2 cm cube of tissue from the original sample and place in a 1.5mL microcentrifuge tube. This should be around 30 mg of tissue

4. dd 350 uL of ML1 Buffer AND 25 uL Proteinase K Solution. Votex to mix

5. Incubate 15 hrs at 56C in thermomixer

6. Clean scalpal blades as follows: 3x rinse with 80% bleach; 2x rinse with 70% EtOH; 2x rinse with DI 2
 


## <a name="sep"></a> **Separatng DNA**

Day 2: 


Prep: 
- Heat bead bath to 70C
- obtain n 1.5 mL microcentrifuge tubes


Steps: 

7. Add 350 uL Chloroform:isoamyl alcohol (24:1). Vortex to mix

8. Centrifuge 10,000g for 2 mins at room temp

9. Transfer the upper aqueous phase to a clean 1.5 mL microcentrifuge tube. Avoid the milky interface

10. Add 1 volume BL Buffer (equal to the amount of liquid from the upper aqueous phase) and 10 uL RNase A. Vortex at max speed for 15 sec

11. Incubate at 70C for 10 mins

12. Place samples on the bench to cool to room temperature



## <a name="prec"></a> **Precipitating DNA**

Day 2 cont: 


Prep: 
- Obtain n HiBind Mini Columns
- 2n 2mL Collection Tubes
- Preheat Elution Buffer to 70C


Steps: 

13. Add one volume 100% EtOH (equal to the amount of liquid from the upper aqueous phase). Vortex at max speed for 15 secs

14. Insert 1 HiBind DNA Mini Column into a 2mL Collection Tube 

15. To equilibrate the columns (optional) add 100 uL 3M NaOH to the minicolumn, centrifuge at max speed for 60 sec, discard the filtrate and reuse the collection tube

16. Transfer 750 uL of sample from 1.5 mL microcentrifuge tube to the minicolumn (include any precipitates)

17. Centrifuge at 10,000g for 1 min

18. Discard the filtrate and reuse the collection tube

19. Repeat steps 16-18 until all sample volumne has been used

20. Discard the filtrate and collection tubes 

21. Insert minicolumn into a new 2mL collection tube

22. Add 500 uL HBC Buffer to the minicolumn 

23. Centrifuge at 10,000g for 30 sec

24. Discard the filtrate and reuse the collection tube

25. Add 700 uL DNA Wash Buffer to the minicolumn 

26. Centrifuge at 10,000g for 1 min

27. Discard the filtrate and reuse the collection tube

28. Repeat steps 25-27 for a second DNA Wash Buffer wash step

29. Centrifuge the empty minicolumn at max speed for 2 min to dry the membrane

30. Transfer the minicolumn to a nuclease-free 1.5 or 2 mL microcentrifuge tube 

31. Add 50-100 uL Elution Buffer to the minicolumn (preheated to 70C; you do not want > 200uL of elution volume) 

32. Let sit at room temperature for 2 min

33. Centrifuge at 10,000g for 1 min

34. Repeat steps 31-33 for a second elution step

35. Quantify DNA with [Qubit](https://github.com/meschedl/PPP-Lab-Resources/blob/master/Protocols_and_Lab_Resources/DNA_Quality_Control/Biotium_Prada_DNA_Qubit.md)

36. Store DNA at -20C


## <a name="clean"></a> **Clean Up**

Matias has found a clean up using [Zymo Clean and Concentrate Kit](https://files.zymoresearch.com/protocols/_d4003t_d4003_d4004_d4013_d4014_dna_clean_concentrator_-5.pdf) after the extraction gives higher yields downstream

Steps: 

1. Warm DNA elution buffer (10mM Tris HCL) to 70C

2. Transfer 20-100 uL DNA into a 1.5 mL tube

3. Add double the DNA volume of DNA binding buffer to each tube

4. Votex briefly

5. Transfer the mixture to a Zymo-Spin Colummn in a Collection Tube

6. Cetrifuge for 30 sec at 3500g and 

7. Discard flow through 

8. Add 200uL DNA Wash Buffer to the column   

9. Centrifuge for 30 sec at 3,500 g 

10. Discard flow through

10. Repeat steps 8-9

11. Transfer the column to a new 1.5 mL microcentrifuge tube

12. Add 20 - 100uL of warmed elution buffer to the column matrix 

13. Incubate at room temperature for 1 min

14. Centrifuge for 30 sec at 3,500 g to elute the DNA 


## <a name="quant"></a> **Quantify**

Quantify DNA concentration using [Qbit](https://github.com/meschedl/PPP-Lab-Resources/blob/master/Protocols_and_Lab_Resources/DNA_Quality_Control/Biotium_Prada_DNA_Qubit.md)

