---
layout: post
title: QC_WGS_Library_Prep Log
categories: QC WGS Library Prep
tags: 
---

## Pooling -- Jan 16
- Pooled libraries into 2 pools 
    - Pool A = 128 uL total, 32 samples, 4 uL per sample, 10nM samples
    - Pool B - 156 uL total, 39 samples, 4 uL per sample, 10nM samples 
- Dilution to 10 nM --> pooling 4 uL per sample

![](https://github.com/wdunster/QC_WGS_23/blob/main/Images/QC_WGS_23_pool1.png)
![](https://github.com/wdunster/QC_WGS_23/blob/main/Images/QC_WGS_23_pool1.2.png)
![](https://github.com/wdunster/QC_WGS_23/blob/main/Images/QC_WGS_23_pool1.3.png)
![](https://github.com/wdunster/QC_WGS_23/blob/main/Images/QC_WGS_23_pool1.4.png)

## Lib Prep 6 -- Jan 08
Notes: 
- I redid samples that had failed previously to see if they would pass the second time. None of the FL samples improved but other samples from BVI, NI, and PR did. 

![](https://github.com/wdunster/QC_WGS_23/blob/main/Images/QC_WGS_23_lib6.png)

![](https://github.com/wdunster/QC_WGS_23/blob/main/Images/QC_WGS_23_lib6.2.png)

- [Tape Station Report](https://github.com/wdunster/QC_WGS_23/blob/main/QC_WGS_PlateA_TapeReport_01-08-24_WD.pdf)

### Final sample set 

| Location   | Num Libraries |
|------------|---------------|
| BVI        | 16            |
| FL         | 9             |
| NI         | 21            |
| PR-Fajardo | 17            |
| PR-Curazo  | 9             |



## Lib Prep 5 -- Jan 03

![](https://github.com/wdunster/QC_WGS_23/blob/main/Images/QC_WGS_23_lib5.png)

![](https://github.com/wdunster/QC_WGS_23/blob/main/Images/QC_WGS_23_lib5.2.png)

Notes: 
At this point I have 
13 BVI, 11 PR-F, 7 FL, 14 NI, 6 PR-other

I need to redo failed samples next and then go back and do a few more PR-F sample using FL or BVI indexes that failed twice

## Lib Prep 4 -- Jan 02
![](https://github.com/wdunster/QC_WGS_23/blob/main/Images/QC_WGS_23_lib4.png)
![](https://github.com/wdunster/QC_WGS_23/blob/main/Images/QC_WGS_23_lib4.2.png)

Notes: 
- The eppendorf < 1uL multichannel was giving me a lot of trouble so i switched over to single for a few things. 
- I think my plan is to clean these samples today and then quanntify them tomorrow with the other 32 samples on the plate reader. I do need to verify the plate reader because of the issues with Cassie's samples so tomorrow I will qbit column A of all rows and then plate read all the samples to compare. 

## Lib Prep 3 -- Dec 5 
![](https://github.com/wdunster/QC_WGS_23/blob/main/Images/QC_WGS_23_lib3.png)
![](https://github.com/wdunster/QC_WGS_23/blob/main/Images/QC_WGS_23_lib3.2.png)

Notes: 
This prep was about 50% effective. I am hoping to get a slightly better turn out. I think I ran into a little bit of trouble with the multichannel. Overall the PR samples worked well. The unclean FL samples did not work but the cleaned ones did even with low initial concentrations. I was surprised so many BVI samples did not work. I have a few more I can try to push through library prep next week before redoing the ones that have failed. 

## Lib Prep 2 -- Nov 09
| Sample ID | Nextera XT index Location | Index sequence | Qbit |
|-----------|---------------------------|----------------|------|
| BVI-14    | A02                       |                | 7.97 |
| BVI-15    | B02                       |                | 6.67 |
| BVI-16    | C02                       |                | 6.71 |
| BVI-17    | D02                       |                | 4.34 |
| BVI-23    | E02                       |                | 0    |
| BVI-24    | F02                       |                | 0    |
| BVI-25    | G02                       |                | 6.31 |
| BVI-26    | H02                       |                | 2.09 |

Notes: 
- Matias still believes this is a good outcome. He suggested that the library prep quality will vary greatly from sample to sample. I will try to do a larger batch next time as I would like to start library prepping in plate format. I think this will save me a lot of time and energy in the long run. I think I should do a dry run through of using a plate as it will definetly be more complicated. I also will not be able to use a whole plate because I have already used 2 columns of the Plate A index. 


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