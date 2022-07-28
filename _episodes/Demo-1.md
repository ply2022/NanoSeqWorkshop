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

**A sucessful sequencing starts with a high quality DNA sample.**

Assessment of DNA samples includes: quality, quantiy, and integrity. We will introduce commonly used methods to perform quality control of DNA samples. 

*Note: Here we listed the methods that our team commonly used. Alternative tools might also avalible.*

### Spectrophotometer (UV-Vis)
Absorbance measurements measure the absorbance of all molecules in the sample that absorb at the wavelength of interest. Nucleotides and proteins have highest absorbance at 260 nm and 280 nm, respectively. The ratio of absorbance at 260 nm and 280 nm is used to determine the purity of the DNA samples. The ratio of ~ 1.8 is cosidered as a pure for DNA samples. On the other hand, chemicals and organic compound (e.g., EDTA, carbohydrate, phenol) absorb at 230 nm. The ratio of 2.0-2.22 is expected as a pure for DNA sample.

<img src="{{site.baseSite}}/fig/NanoDrop_spectra.png" align="center" height="500">

**Drawbacks** 
1. Nonselctive method: could not differentiate DNA or RNA
2. pH sensitive: acid solution and basic solution decrease or increase 260/280 ratio, rescpectively. 

**Adventages**
1. Cheaper: do not need extra plastic comsumables and kits
2. Faster: no addtional reaction time

### Fluorometry
Compare to spectrophotometer, fluorometric measurement is more specific and sensitive despite the presence of contaminants due to the selective binding between flurogenic dyes and DNA. Using [Qubit dsDNA HS Assay Kit](https://assets.thermofisher.com/TFS-Assets/LSG/manuals/Qubit_dsDNA_HS_Assay_UG.pdf){: target="_blank"} as example, 


### Spectrophotometer vs. Fluorometry
So...which method is better? 
<img src="{{site.baseSite}}/fig/NanoDropVSQubit.png" align="center">


### Integrity assessment
Gel-based method


## Ligation sequencing gDNA (SQK-LSK110)


# Genomic DNA sequencing on the MinION sequencer
 
## Anatomy of the flow cell

## Performing flow cell check 

## Priming and loding the flow cell

Please review this video for demonstration (you might might start from 00:18 to skip th intro):
<iframe width="700" height="480" src="https://www.youtube.com/embed/Pt-iaemrM88" title="Priming and loading your flow cell" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

> ## Loading the flow cell
> **DO NOT** pipette while you priming or loading the flowcell. Flow cell is very >sensitive to pipetting. Pipetting might lead to pore loss. 
{: .caution}


> ## Addtional reading materials 
> 1. [Assessment of Nucleic Acid Purity-Technical Note 52646](https://assets.thermofisher.com/TFS-Assets/CAD/Product-Bulletins/TN52646-E-0215M-NucleicAcid.pdf#:~:text=Small%20changes%20in%20the%20pH%20of%20the%20solution,the%20diluted%20sample%20measured%20on%20the%20conventional%20spectrophotometer.){: target="_blank"}
> 2. [260/280 and 260/230 Rations-T042-Technical Bulletin](http://hpc.ilri.cgiar.org/beca/training/IMBB_2015/lectures/NanoDrop.pdf){: target="_blank"}
> 3. [Qubit fluorometric quantitation vs. spectrophotometer measurements](https://assets.thermofisher.com/TFS-Assets/LSG/Technical-Notes/fluorescence-UV-quantitation-comparison-tech-note.pdf){: target="_blank"}
> 4. [DNA Qualification Workflow for Next Generation Sequencing of Histopathological Samples](https://doi.org/10.1371/journal.pone.0062692){: target="_blank"}
{: .tips}

