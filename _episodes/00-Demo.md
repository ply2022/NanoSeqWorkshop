---
title: "Overview of library preparation and demonstration"
teaching: 30
exercises: 15
questions:
- How to perform sequencing on the MinION sequencer?
objectives:
- Learn the process of the library preparation 
- Load the MinION flow cell and learn how to run sequencing 
keypoints:
- Avoid introduce the bubbles when laoding the flow cell. 

---

# Library preparation 

In this workshop, we will introduce you the [Ligation Sequecning Kit](https://store.nanoporetech.com/us/ligation-sequencing-kit110.html){: target="_blank"}, which is a genome sequencing kit. We are going to use the protocol [Ligation sequencing gDNA (SQK-LSK110)](https://community.nanoporetech.com/docs/prepare/library_prep_protocols/genomic-dna-by-ligation-sqk-lsk110/v/gde_9108_v110_revv_10nov2020){: target="_blank"}.
In the community, you can review the process in detail. Due to the time and equipemnts limitation. We **will not** perform actual library preparation process. We will focus on the flow cell loading and setting the MinKonw software.

## Quality controls of DNA samples

**A successful sequencing starts with a high-quality DNA sample.**

Assessment of DNA samples includes: quality, quantiy, and integrity. We will introduce commonly used methods to perform quality control of DNA samples. 

*Note: Here we listed the methods that our team commonly used. Alternative tools might also avalible.*

### Spectrophotometer (UV-Vis)
Absorbance measurements measure the absorbance of all molecules in the sample that absorb at the wavelength of interest. Nucleotides and proteins have highest absorbance at 260 nm and 280 nm, respectively. The ratio of absorbance at 260 nm and 280 nm is used to determine the purity of the DNA samples. The ratio of ~ 1.8 is considered as a pure for DNA samples. On the other hand, chemicals and organic compound (e.g., EDTA, carbohydrate, phenol) absorb at 230 nm. The ratio of 2.0-2.22 is expected as a pure for DNA sample.

<img src="{{site.baseSite}}/fig/NanoDrop_spectra.png" align="center" width="700">

**Figure**. Spectral profiles of pure DNA (A), and pure DNA with guanidine (B) and phenol (C).

**Drawbacks** 
1. Nonselective method: could not differentiate DNA or RNA.
2. pH sensitive: acid solution and basic solution decrease or increase 260/280 ratio, respectively.
 

**Adventages**
1. Cheaper: do not need extra plastic consumables and kits.
2. Faster: no additional reaction time

### Fluorometry
Compare to spectrophotometer, fluorometric measurement is more specific and sensitive despite the presence of contaminants due to the selective binding between fluorogenic dyes and DNA. Dyes emit fluorenes only when specific binding happens, and the fluorenes will be detected by fluorometer. Concentration of the nucleic acid is calculated using standard curve. Using [Qubit dsDNA HS Assay Kit](https://assets.thermofisher.com/TFS-Assets/LSG/manuals/Qubit_dsDNA_HS_Assay_UG.pdf){: target="_blank"} as an example, it can measure sample concentration ranging from 10 pg/μL to 100 ng/μL. Yet, fluorogenic dyes can not measure the purity of the DNA. 

**Drawbacks** 
1. Time consuming: reagent-sample reaction takes 2 min required for DNA assay
2. Costly: reagents and single-use plastic consumables (Qubit assay tubes) are required

### Spectrophotometer vs. Fluorometry
So...which method is better? 
<img src="{{site.baseSite}}/fig/NanoDropVSQubit.png" align="center" width="700">

**Figure A**. Comparisons of repeated measurements (n = 10) between commercial kits (Qubit) and spectrophotometer (NanoDrop). Serial dilution of DNA samples ranging from 50-0.5 ng/µl was used to evaluate two methods. 

**Figure B**. Comparisons of sample concentration ranges among different methods. 

### Integrity assessment
To evaluate integrity of DNA samples, we need gel electrophoresis systems, such as [Agilent 2200 TapeStation Systems](https://www.agilent.com/en/product/automated-electrophoresis/tapestation-systems/tapestation-software/2200-tapestation-software-228269#support). The software provides numerical number of DNA integrity called DNA Integrity Number (DIN) and sample concentration.  

<img src="{{site.baseSite}}/fig/TapeStation.png" align="center" width="700">

**Figure A**. The gel image with DNA Integrity Number (DIN). A1: DNA marker; B1-H2: degradation series of 15 g DNA samples (60 ng/µl).

**Figure B**. A electropherogram of 15 samples from Figure A. Highly intact gDNA 
samples (H2) has a narrow peak in the electropherogram above the largest ladder peak (48,500 bp). 

## Ligation sequencing gDNA (SQK-LSK110)
You can find step by step guide including third party reagents, equipments and consumables in the [protocol](https://community.nanoporetech.com/protocols/genomic-dna-by-ligation-sqk-lsk110/checklist.pdf?devices=minion) for MinION (R9.4.1). Here we will provide a overview of the workflow. 

### Optional fragmentation 
When should I performe fragmentation of genomic DNA (gDNA)?
Although the protocol recommends starting DNA sample concentration is 1 µg, fragemntation might be useful for low input amount of DNA. Also, you might benefit from optimal fragmentation when you are handling very high molecular weight gDNA. [Oxford Nanopore Technologies (ONT)](https://community.nanoporetech.com/extraction_method_groups/optional-fragmentation-of-gdna) provides a review of how factors including fragment size, loding amount, and blocking affect the flow cell output. Here we will discuss how fragmentation affects read N50. 

The following figures demonstrate that fragemnt length and read N50 do not have linear relationship. 
<img src="{{site.baseSite}}/fig/Fragmentation&N50.png" align="center" width="700">

**Figure A**. Sheared Human gDNA samples were sheared with different speed. The fragment size was analyzed. Increasing shearing speed leads to more fragmented DNA. 

**Figure B**. Unsheared and sheared DNA was used for library preparation using ligation sequencing kit and sequenced on MinION. Fragmentation speed between 23 to 30 increases read N50 when compared with unsheared gDNA. Yet, excessive shearing might have deleterious effect on flow cell output. 

> ## Read N50
> Definition of read N50 is "the sequence length of the shortest read at 50% of the total sequencing dataset sorted by read length" [(Logsdon et al., 2020)](https://doi.org/10.1038/s41576-020-0236-x).
> <img src="{{site.baseSite}}/fig/N50.svg" align="center" width="700">
{: .tips}

### First step: DNA repair and end-repair
Histopathological or histology samples are preserved with formalin fixed paraffin embedded (FFPE) method. However FFPE significantly compromises the DNA quality from the specimens.[NEBNext FFPE DNA Repair Mix](https://www.neb.com/products/m6630-nebnext-ffpe-dna-repair-mix#Product%20Information), a cocktail of enzymes, is used to repair DNA. 

DNA fragments do not have homogeneousn blunt-ended fragments, so [NEBNext Ultra II End Repair/dA-Tailing Module](https://www.neb.com/products/e7546-nebnext-ultra-ii-end-repair-da-tailing-module#Product%20Information) is used to repair overhangs by 5' Phosphorylation (P). Adding non-template dAMP (dA) to the 3′ end of blunt-ended fragments prevents a continous DNA molecule (concatemer) formation in the following ligation step and permits subsequent ligation of adapter that has complementary 3′-dT overhangs. 

### Second step: Adapter ligation 

Prepared DNA fragments is ligated with sequencing adapters (red) tagged with mortor protein (brown). Both template and complementary strands are tagged with mortor protein, so both strand will translocate the nanopore. Mortor protein has helicase activity and it also control the DNA translocation speed. Once the sequencing adapter inserts into the nanopore, double-strand DNA will be first unwound by motor protein. Eletric current drives negatively charged DNA pass through the pore (~450 bases per second).

<img src="{{site.baseSite}}/fig/LibraryPrep.svg" align="center" width="800">

# Genomic DNA sequencing on the MinION sequencer

## MinION Mk1B and flow cell

### The MinION Mk1B

The MinION Mk1B instrument connects the user’s PC and the nanopore sensor. It provides power to application-specific integrated circuit (ASIC), performs temperature control, protects sensor from electronic noise, and transfer data to the user's PC. The MinION Mk1B does not have active heating element. The heat generated from ASIC and control of fan speed maintain a steady temperatue at 37 °C. 

<img src="{{site.baseSite}}/fig/MinION_Mk1B.png" align="center" width="700">

### Configuration test cell (CTC)

CTC is used to test the communication between the MinION Mk1B and sequencing software, MinKNOW, on user's PC. 

<img src="{{site.baseSite}}/fig/CTC.png" align="center" width="700">

### Anatomy of flow cell

The flow cell is a consumable that contains a fluidic interface allowing DNA samples being sequenced. Sensor chip is one of the major components of the flow cell. Sensor array is located on the front of sensor chip. MinION Mk1B flow cell carries (ideally) 2048 active wells (nanopores) on the surface of sensor array. 

<img src="{{site.baseSite}}/fig/SpotON_FlowCell.png" align="center" width="700">
<img src="{{site.baseSite}}/fig/FlowCellChips.png" align="center" width="700">


### Storage buffer

The flow cells are shipped and preserved with the storage buffer (yellow). The storage buffer serves different roles: 1) maintaining osmotic balance, 2) allowing current to run through the nanopores, 3) identifying functional pores during flow cell checking.  

<img src="{{site.baseSite}}/fig/StorageBuffer.png" align="center" width="700">

The flow cell is flushed with priming mix (Flush Tether + Flush Buffer; colored in blue) through priming pore to replace the storage buffer in the bulk section before library loading. Priming mix contains tether proteins guiding DNA fragments toward nanopores. DNA library is loaded to sensor array through SpotON sample port.  

<img src="{{site.baseSite}}/fig/Priming.png" align="center" width="700">

## Flow cell check 

### Purpose of the [flow cell check](https://community.nanoporetech.com/docs/prepare/library_prep_protocols/flow-cell-check/v/pqe_1004_v1_revag_06jan2016)
ONT offers a warranty within 3 months from purchase of MinION flow cell. If the active pore number below the warranty number, you can request a replacement. 

<img src="{{site.baseSite}}/fig/FlowCellQC.png" align="center" width="700">

### Running the flow cell check
Before you run the flow cell check, you need to have a PC connected with internet and MinION Mk1B device and your PC also need to be intalled with MinKNOW software. 

[Step 1] Open the sequencer lid and insert the flow cell. First slide flow cell under the clip, and then press down the flow cell. **Note**: Do not touch the bottom of the flow cell. 

<img src="{{site.baseSite}}/fig/IncertFlowCell.png" align="center" width="700">

[Step 2] Connect assembled sequencer to your PC. You will see the lights and hear sound of fan if the connection is successful. 

<img src="{{site.baseSite}}/fig/ConnectedMinION.png" align="center" width="700">

[Step 3] Double-click the MinKNOW icon on your desktop, and log into software. 

<img src="{{site.baseSite}}/fig/MinION_login.png" align="center" width="700">

[Step 4] If you insert the new flow cell, the overview will show that the flow cell has not been checked (orange question mark). 

<img src="{{site.baseSite}}/fig/MinKNOW_overview.png" align="center" width="700">

[Step 5] Click start and select **Flow cell check**.
<img src="{{site.baseSite}}/fig/StartFlowCellCheck.png" align="center" width="700">

[Step 6] The system detects MiinION Flow cell ID automatically. Click **Start**. 
<img src="{{site.baseSite}}/fig/FlowCellCheck_1.png" align="center" width="700">

[Step 7] It will takes about 5 minutes to finish flow cell check.
Two types of output:

**Yellow exclamation mark**: The numbe of active pores is below warranty number.

**Green tick**: The number of active proes is above warranty number. **[Need a picture here]**
<img src="{{site.baseSite}}/fig/FlowCellCheck_out.png" align="center" width="700">

Do not panic when you see a yellow exclamation mark in this workshop. We provided eveybody **used** flow cell. 

## Priming and loding the flow cell
Please review this video for demonstration (you might might start from 00:18 to skip th intro):
<iframe width="700" height="480" src="https://www.youtube.com/embed/Pt-iaemrM88" title="Priming and loading your flow cell" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

> ## Loading the flow cell
> **DO NOT** pipette while you priming or loading the flowcell. Pipetting might lead to irreversibly damage to the membrane.
{: .caution}

## [Start sequencing]
Due to the space and time limitations. Participants ***will not** initiate sequencing. Yet, instructor will demonstrate the sequencing process. 

[Step 0] Running the flow cell check with the new or washed flow cell.
<img src="{{site.baseSite}}/fig/Sequencing_overview1.png" align="center" width="700"> 

[Step 1] At the homepage, click **Start** and then select **Start sequencing**

<img src="{{site.baseSite}}/fig/SeqExp_1.png" align="center" width="700">

[Step 2] Fill in the following information:
**(1)** Experiment name
**(2)** Sample ID

Click **Continue to kit selection**. 
<img src="{{site.baseSite}}/fig/SeqExp_2.png" align="center" width="700">

[Step 3] Select the **Ligation sequencing gDNA (SQK-LSK110)** used for this experiment. Select options to narrow down the list of kits.
<img src="{{site.baseSite}}/fig/SeqExp_3.png" align="center" width="700">

[Step 4] Select run options. Change Time and read length.
Click **Continue to basecalling**
<img src="{{site.baseSite}}/fig/SeqExp_4.png" align="center" width="700">

[Step 5] Choose basecalling, barcoding and alignment options. To save the disk space, we will not perform real-time basecalling. 
Click **Continue to output**
<img src="{{site.baseSite}}/fig/SeqExp_5.png" align="center" width="700">

[Step 6] Select the output data location, format and filtering options.
Click **Continue to final review**
<img src="{{site.baseSite}}/fig/SeqExp_6.png" align="center" width="700">

[Step 7] This Review page is an overview of the selected options.
Click **Start**
<img src="{{site.baseSite}}/fig/SeqExp_7.png" align="center" width="700">

[Step 8] You will be redirected to the sequencing overview once sequencing starts. A report of flow cell pore health will be provided after first multiplexer (pore) scan.
<img src="{{site.baseSite}}/fig/SeqExp_8.png" align="center" width="700">

<img src="{{site.baseSite}}/fig/SeqExp_9.png" align="center" width="700">

[Step 9] Click **Experiments**. You can access to the realtime report of the sequencing report. 
<img src="{{site.baseSite}}/fig/SeqExp_10.png" align="center" width="700">

<img src="{{site.baseSite}}/fig/SeqExp_11.png" align="center" width="700">

<img src="{{site.baseSite}}/fig/SeqExp_12.png" align="center" width="700">

<img src="{{site.baseSite}}/fig/SeqExp_13.png" align="center" width="700">


> ## The multiplexer (MUX) scan
> Once the sequencing protocol is initiated, a pore scan will start before sequencing. MUX run allows MinKNOW to divide active pores into four groups, which let MinION to prioritise the order of the pores to be used for sequencing. Group 1 will be used in the first 8-hour run. Pore prioritising maximize the data output. 
> To have maximum number of pores that are avalible for sequencing, you should selction **Active channel selection**. When **Active channel selection** is on, MinKNOW will try to switch to a new pore once the pore is in the 'Saturated' or 'Multiple' state. 
{: .tips}

> ## Addtional reading materials 
> 1. [Assessment of Nucleic Acid Purity-Technical Note 52646](https://assets.thermofisher.com/TFS-Assets/CAD/Product-Bulletins/TN52646-E-0215M-NucleicAcid.pdf#:~:text=Small%20changes%20in%20the%20pH%20of%20the%20solution,the%20diluted%20sample%20measured%20on%20the%20conventional%20spectrophotometer.){: target="_blank"}
> 2. [260/280 and 260/230 Rations-T042-Technical Bulletin](http://hpc.ilri.cgiar.org/beca/training/IMBB_2015/lectures/NanoDrop.pdf){: target="_blank"}
> 3. [Qubit fluorometric quantitation vs. spectrophotometer measurements](https://assets.thermofisher.com/TFS-Assets/LSG/Technical-Notes/fluorescence-UV-quantitation-comparison-tech-note.pdf){: target="_blank"}
> 4. [DNA Qualification Workflow for Next Generation Sequencing of Histopathological Samples](https://doi.org/10.1371/journal.pone.0062692){: target="_blank"}
> 5. [DNA Integrity Number (DIN) with the Agilent 2200 TapeStation System and the Agilent Genomic DNA ScreenTape Assay](https://www.agilent.com/cs/library/applications/5991-5258EN.pdf){: target="_blank"}
> 6. [Community-Online crouses](https://community.nanoporetech.com/nanopore_learning/courses/introduction-to-nanopore-sequencing){: target="_blank"}
> 7. [Community-Documentation](https://community.nanoporetech.com/docs){: target="_blank"}
> 8. [Priming and loading the SpotON flow cell for MinION](https://community.nanoporetech.com/docs/prepare/library_prep_protocols/genomic-dna-by-ligation-sqk-lsk110/v/gde_9108_v110_revv_10nov2020/priming-and-loading-the-spoton-flow-cell?devices=minion)
{: .tips}


