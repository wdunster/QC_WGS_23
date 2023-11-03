---
layout: post
title: QC_WGS_Library_Prep Log
categories: QC WGS Library Prep
tags: 
---

 ## Lib Prep 1 -- Nov 03
| Sample ID | Nextera XT index Location | Index sequence | Lib Qbit    |
|-----------|---------------------------|----------------|-------------|
| BVI-1     | A01                       |                | 11.36, 11.3 |
| BVI-2     | B01                       |                |  6.68, 6.60 |
| BVI-3     | C01                       |                |  0, 0       |
| BVI-4     | D01                       |                |  5.74, 5.72 |
| BVI-5     | E01                       |                |  0, 0       |
| BVI-6     | F01                       |                |  6.66, 6.70 |
| BVI-7     | G01                       |                |  8.84, 8.84 |
| BVI-8     | H01                       |                |  10.7, 10.7 |

Notes: 
- Using 1.5x original volumes 
- New Plate A Nextera XT Indexes
- BVI-3 and BVI-5 failed. All other samples have good (> 5ng/uL) libraries. Good libraries are stored in a box labeled "PRADA, QC_WGS LIBRARIES" in the post PCR -20C fridge. 
- Matias suggests waiting and doing the 2 failed samples last with any other failed samples over the course of the libraries. He also suggested going back to see what the [DNA] was because samples with a low [] to begin with will likely not amplify well.

Tagmentation steps: 
- Remove samples diluted to 0.5 ng/uL and thaw on ice with a paper towel under the samples 
- Clean all supplies with DNA away (2n strip tubes, 2 strip tube racks, red and green pipette and filter tipe boxes)
- Thaw 2 tubes of TD and ATM. Check NT in Nextera box for precipitates and spin down
- Invert TD and ATM aggresively with a wrist flick three times and spin down jsut before using the reagent
- Add 2.13 uL TD to each well, closing all tubes in between pipetting 
- Add 1.05 uL DNA closing all tubes between pipetting. Pipete mix 10 times
- Add 1.05 of ATM to each well and pipette mix 10 times 
- Centrifuge in the cold centrifuge at 3000rcf at 20C for 3 mins. Check for bubbles. If there are bubbles present flick them away and re centrifuge
- Place in the thermocycler on the TAG1 program (13 mins). While this is in the thermocycler, take out index plate and thaw on ice. Prepare qbit tubes
- Immediately take out the tubes once the cycle has reached the block 10C. The ATM is a tagmentation buffer that fragments and add adapter to DNA. We need to stop the tagmentation immediately 
- Add 1.05 uL NT to each well to neutralize the enzyme tragmentation. Pipette mix 10 times
- Centrifuge at 3000 rcf at 20C for 3 min. Check to make sure all bubbles have gone 
- Incubate the plate at room temp for 5 mins
- Place the plate on ice, the final volume is 4.23uL 

Amplification steps: 
- Thaw NPM on ice right before use
- Add 2.13 of planned index to the corresponding tube of tagmented DNA
- Add 3.21 uL NPM and pipette mix 10 times
- Centrifuge at 3000 rcf at 20C for 3 mins. Make sure there are no bubbles
- Place in the thermocycler PCR 15 program (~45 mins). Prep for clean-up during this time is cleaning immediately 
- Place on ice

Clean-up steps: 
- To prep make fresh ethanol (for 1 strip make 5ml Ethanol and 1.25 ml Water), warm Tris at 55C, Take beads, water, and qbit out of the fridge.
- Add 10.5 uL molec water to the DNA
- Add 20 uL beads to each well and pipette mix 10-20 times. Try to avoid making bubbles
- Spin down briefly 
- Place on the orbital shaker for 15 min at max rpm
- Place the tubes on a magnet and shake again for 2 min
- Multichannel the subernatent (~ 33 uL) of clear liquid into a waste trough  
- Add 75 uL of 80% ethanol to each well, not disturbing the beads
- Remove 75 uL of liquid from tubes and place in waste trough 
- Add 75 uL of 80% ethanol for a second washe step not disturbing the beads
- Remove 75 uL of liquid and place in waste trough
- Remove any remaining ethanol with a p20 set to 20 uL 
- Remove the tubes from the magnet
- Resuspend the beads with 17 uL of 10mM Tris HCL
- Place the tubes on the shaker for 5 mins
- Afer 5 mins place the tubes on the magnet and put back on the shaker for 2 mins
- Remove the tube from the shaker and remove 16 uL of the clear liquid into a new strip tube labeled with sample ID. This is your cleaned and tagmented DNA
- The beads are magnetic and attract everything over a certain molecular weight. Then everything else (extra nucleotides, small DNA) will be removed