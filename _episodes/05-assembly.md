---
title: "Genome assembly"
teaching: 45
exercises: 30
questions:
- How can I assemble a genome with the reads generated from the nanopore sequencers.
objectives:
- Hybrid genome assembly using Nanopore and Illumina reads.
- Scaffolding assembled contigs using Ragtag.
- Accessing genome assembly quality using BBMap and BUSCO.
keypoints:
- Don't forget to edit the email in the SLURM scripts. 
- There are several other ways to submit the job. Some software support multithread and you can write the array scripts request resources. 
---

# Genome assembly

## An overview of Genome Assembly
 Once we have the "clean" Nanopore reads, we are ready to assemble those reads into contiguous sequences (contigs). There are many tools deployed different algorithms to assemble genomes with nanopore reads. Due to the nature of the Long-Read technologies, the pipeline of assembling error-prone Nanopore reads incooprate error correction to facilitate identifying ovelapping reads and improve assembly. 

### Genome assembly using SMARTdenovo
We will assemble the draft assembly using *de novo* assembler for our Nanopore reads. This software does not include error correction steps. SMARTdenovo deploys 
overlap-layout-consensus (OLC) algorithm. 

> ## Overlap-Layout-Consensus (OLC) algorith
> An assembly methods finds overlaps among all the reads, and then build the overlap graph to sort into the contigs. Align those contigs to make a concensus sequence.
> <img src="{{site.baseSite}}/fig/OLC.svg" align="center" width="400">
{: .tips}

### SLURM scripts
To get started with SMARTdenovo, lets create new directory in your work directory and copy a submission script template from /blue/general_workshop/share/bash_files directory.

~~~
$ cd /blue/general_workshop/<username>

$ mkdir SMARTdenovo

$ cd SMARTdenovo

$ cp /blue/general_workshop/share/bash_files/Smartdenovo.sh ./Smartdenovo.sh
~~~
{: .language-bash}

Note: `.sh` is commonly used extension for shell scripts. Using a extension is not mandatory.

### Adding information to SLURM script

We have to modify some information in the template to provide more information to SLURM about the job.

We can use a small text editor program called `nano` to edit a file.
This will open a basic text editor.

~~~
$ nano Smartdenovo.sh
~~~
{: .language-bash}

~~~
-----------------------------------------------------------------------------------------------
 GNU nano 3.3 beta 02                     File: Smartdenovo.sh
-----------------------------------------------------------------------------------------------
#!/bin/bash
#SBATCH --job-name=Smartdenovo        # Job name
#SBATCH --account=general_workshop    # Account to run the computational task
#SBATCH --qos=general_workshop        # Account allocation
#SBATCH --mail-type=END,FAIL          # Mail events (NONE, BEGIN, END, FAIL, ALL)
#SBATCH --mail-user=<email_address>   # You need provide your email address
#SBATCH --ntasks=1                    # Run on a single CPU
#SBATCH --cpus-per-task=1
#SBATCH --mem=80gb                    # Job memory request
#SBATCH --time 12:00:00               # Time limit hrs:min:sec
#SBATCH --output=SuwSamrtdenovo_%j.out   # Standard output and error log

pwd; hostname; date

module load smartdenovo/20180219

# Run genome assembly
smartdenovo.pl -p Suw -c 1 /blue/general_workshop/share/Suwannee/Suw2_filtered_3000bp_60X.fastq > Suw.mak

make -f Suw.mak


-----------------------------------------------------------------------------------------------
^G Get Help     ^O WriteOut     ^R Read File     ^Y Prev Page     ^K Cut Text       ^C Cur Pos
^X Exit         ^J Justify      ^W Where Is      ^V Next Page     ^U UnCut Text     ^T To Spell
-----------------------------------------------------------------------------------------------
~~~
{: .terminal}

### Checking usage of `smartdenovo.pl`
~~~
$ module load smartdenovo/20180219  
$ smartdenovo.pl
~~~
{: .language-bash}

~~~
Usage: smartdenovo.pl [options] <reads.fa>
Options:
  -p STR     output prefix [wtasm]
  -e STR     engine of overlaper, compressed kmer overlapper(zmo), dot matrix overlapper(dmo), [dmo]
  -t INT     number of threads [8]
  -k INT     k-mer length for overlapping [16]
             for large genome as human, please use 17
  -J INT     min read length [5000]
  -c INT     generate consensus, [0]
~~~
{: .output}

> ## Make: software build automation tool
> The make program is an intelligent utility and works based on the changes you do in your source file (makefile), which is `Suw.mak`.
> Makfile simplifies the process of building program. 
{: .tips}

Change the &lt;email_address&gt; to your email address where you can check email.
Once you are done, press <kbd>Ctrl</kbd>+<kbd>x</kbd> to return to bash prompt.
Press <kbd>Y</kbd> and <kbd>Enter</kbd> to save the changes made to the file.

> ## Editing in nano
> **nano** is a commandline editor. You can only move your cursor with 
> arrow keys: <kbd>↑</kbd>, <kbd>↓</kbd>, <kbd>←</kbd> and <kbd>→</kbd>.
> Clicking with mouse does not change the position of the cursor. 
> Be careful, you may be editing in wrong place.
>
> If you accidentally edited in wrong place, exit nano, 
> delete the script `slurm.sh` and copy again from share directory.
{: .caution}

## Running a job in SLURM

### Submitting a SMARTdenovo job

To submit the job to SLURM, `sbatch` command is used.

~~~
$ sbatch Smartdenovo.sh
~~~
{: .language-bash}

~~~
Submitted batch job <jobid>
~~~
{: .output}

### Checking status of a SLURM job

You can check the status of the job using the command `squeue`. 
`-u` argument accepts &lt;username&gt; and displays all jobs by the user.
`-A` argument accepts account name and displays all jobs 
using the resources allocated to that account.

~~~
$ squeue -u <username> 
~~~
{: .language-bash}

~~~
             JOBID PARTITION     NAME     USER        ST       TIME  NODES NODELIST(REASON)
          43807246 hpg-milan Smartden     <username>   R       0:14      1 c0709a-s3
~~~
{: .output}

> If you do not see your job, it may have already been completed.
> Run the job again and check within a minute.
> Also, check your email to see if you received any messages from HiperGator.
{: .tips}

~~~
$ squeue -A general_workshop
~~~
{: .language-bash}

~~~
    JOBID PARTITION        NAME    USER  ST       TIME  NODES NODELIST(REASON)
  <jobid> hpg2-comp    Smartden <user1>   R       0:07      1 c29a-s2
  <jobid> hpg2-comp    Smartden <user2>   R       1:32      1 c15a-s1
  <jobid> hpg2-comp    Smartden <user3>   R       2:01      1 c09a-s4

~~~
{: .output}

> ## Understanding Job Status
> Under status `ST`, `R` stands for Running and `PD` stands for pending.
> If the job is pending, a reason may be provided in last column. Eg:
> - **None**: Just taking a while before running.
> - **Priority**: Higher priority jobs exist in this partition.
> - **QOSMaxCpuPerUserLimit**: The user is already using max number of CPU that they are allowed to use.
{: .tips}

### Checking the log file

The SLURM submission script containas a line `#SBATCH --output Samrtdenovo_%j.out`. Thus the output for this job with be in the file `Samrtdenovo_<jobid>.log`.

The job might takes about a day, so we **will not** have the output by the end of today. Let's copy the log file from `/blue/general_workshop/share/Suwanee2/Smartdenovo` directory.

~~~
$ cp /blue/general_workshop/share/Suwannee/Smartdenovo/SuwSmartdenovo_58583802.out .
~~~
{: .language-bash}

~~~
$ ls
~~~
{: .language-bash}

~~~
Smartdenovo.sh  Suw.fa.gz  SuwSamrtdenovo_<jobid>.out
Suw.dmo.ovl     Suw.mak    SuwSmartdenovo_58583802.out
~~~
{: .output}

~~~
$ head SuwSmartdenovo_58583802.out
~~~
{: .language-bash}

~~~
/blue/jeremybrawner/smithk/F_Cir/Suwannee2/Smartdenovo # wroking directory
c5a-s23.ufhpc # hostname (a label that is assigned to a device connected to a computer network)
Sat Sep 12 12:02:30 EDT 2020 (Date)
/apps/smartdenovo/20180219/bin/wtpre -J 5000 /blue/jeremybrawner/smithk/F_Cir/Suwannee2/Suw2_filtered_3000bp_60X.fastq | gzip -c -1 > Suw.fa.gz
/apps/smartdenovo/20180219/bin/wtzmo -t 8 -i Suw.fa.gz -fo Suw.dmo.ovl -k 16 -z 10 -Z 16 -U -1 -m 0.1 -A 1000
[Sat Sep 12 12:04:43 2020] loading long reads
[Sat Sep 12 12:05:39 2020] Done, 93129 reads (length >= 0)
[Sat Sep 12 12:05:39 2020] sorted sequences by length dsc
[Sat Sep 12 12:05:39 2020] calculating overlaps, 8 threads
[Sat Sep 12 12:05:39 2020] indexing 1/1
~~~
{: .output}

~~~
$ tail SuwSmartdenovo_58583802.out
~~~
{: .language-bash}

~~~
103 tips, 7 bubbles, 0 chimera, 5 non-bog, 0 recoveries
[Sun Sep 13 07:02:30 2020] repair 115 bog elements
3 tips, 0 bubbles, 0 chimera, 1 non-bog, 0 recoveries
[Sun Sep 13 07:02:30 2020] repair 4 bog elements
0 tips, 0 bubbles, 0 chimera, 0 non-bog, 0 recoveries
[Sun Sep 13 07:02:30 2020] generated 2067 unitigs
[Sun Sep 13 07:02:30 2020] recover 16 edges inter unitigs
[Sun Sep 13 07:03:00 2020] output 21 independent unitigs
[Sun Sep 13 07:03:00 2020] Done
/apps/smartdenovo/20180219/bin/wtcns -t 8 Suw.dmo.lay > Suw.dmo.cns 2> Suw.dmo.cns.log
~~~
{: .output}

The end of log file provides a important information that Nanopore reads were assembled into **21 unitgs**. 

> ## Unitig vs Contig
> Contig is a set of overlapped DNA fragements. While unitig contains multiple contigs.
{: .tips}

# Polishing contigs using Racon

The SMARTdenovo does not include error correction/polishing steps. Racon aims to generate consensus sequences which is similar or better quality compared to the output generated from other assembly algorithms deploying error correction or consensus steps. Racon support data generated from different Long-read technologies. 

## Mapping of Nanopore reads to the assembly
To get started with Racon, we need to map the Nanopore reads to our genome assembly. 

Burrows-Wheeler Alignment (BWA) tool aligns DNA fragments to genome assembly/reference genome. Three algorithm are avalible: BWA-backtrack (short-read alignment only), BWA-SW and BWA-MEM. To process the long reads, BWA-MEM is faster and more accurate than BWA-SW. 

Let's create new directory in your work directory and copy a submission script template from /blue/general_workshop/share/bash_files directory.

~~~
$ cd /blue/general_workshop/<username>

$ mkdir Polishing

$ cd Polishing

$ cp /blue/general_workshop/share/bash_files/bwa.sh ./bwa.sh
~~~
{: .language-bash}

Once you copy the script to your working directory, we need to edit the script. 

~~~
$ nano bwa.sh
~~~
{: .language-bash}

~~~
-----------------------------------------------------------------------------------------------
 GNU nano 3.3 beta 02                     File: bwa.sh
-----------------------------------------------------------------------------------------------
#!/bin/bash
#SBATCH --job-name=BWA                # Job name
#SBATCH --account=general_workshop    # Account to run the computational task
#SBATCH --qos=general_workshop        # Account allocation
#SBATCH --mail-type=END,FAIL          # Mail events (NONE, BEGIN, END, FAIL, ALL)
#SBATCH --mail-user=<email_address>   # You need provide your email address
#SBATCH --ntasks=1                    # Run on a single CPU
#SBATCH --cpus-per-task=1
#SBATCH --mem=25gb                    # Job memory request
#SBATCH --time 12:00:00               # Time limit hrs:min:sec
#SBATCH --output=Suw_index_%j.out     # Standard output and error log

pwd; hostname; date

module load bwa/0.7.17

# First generate index file of assembly
bwa index /blue/general_workshop/share/Suwannee/Smartdenovo/Suw.utg.fa

# Align filtered Nanopore reads to the assembly
bwa mem -t 14 -x ont2d /blue/general_workshop/share/Suwannee/Smartdenovo/Suw.utg.fa \
/blue/general_workshop/share/Suwannee/Suw2_filtered_3000bp_60X.fastq > Suw.sam


-----------------------------------------------------------------------------------------------
^G Get Help     ^O WriteOut     ^R Read File     ^Y Prev Page     ^K Cut Text       ^C Cur Pos
^X Exit         ^J Justify      ^W Where Is      ^V Next Page     ^U UnCut Text     ^T To Spell
-----------------------------------------------------------------------------------------------
~~~
{: .terminal}

### Checking usage of `bwa mem`
~~~
$ module load bwa
$ bwa mem
~~~
{: .language-bash}

~~~
Usage: bwa mem [options] <idxbase> <in1.fq> [in2.fq]

Algorithm options:
      
       -t INT        number of threads [1]

Scoring options:

-x STR        read type. Setting -x changes multiple parameters unless overridden [null]
                     pacbio: -k17 -W40 -r10 -A1 -B1 -O1 -E1 -L0  (PacBio reads to ref)
                     ont2d: -k14 -W20 -r10 -A1 -B1 -O1 -E1 -L0  (Oxford Nanopore 2D-reads to ref)
                     intractg: -B9 -O16 -L5  (intra-species contigs to ref)
~~~
{: .output}

**Note**: Due to the page size limitation, only partial options are shown.

Change the &lt;email_address&gt; to your email address where you can check email. Once you are done, press <kbd>Ctrl</kbd>+<kbd>x</kbd> to return to bash prompt. Press <kbd>Y</kbd> and <kbd>Enter</kbd> to save the changes made to the file.

> ## A faster option fo mapping
> [minimap2](https://github.com/lh3/minimap2) has relaced BWA-MEM for PacBio or Nanopore reads alignment. minimap2 retians major features of BWA-MEM, but abou 50 times faster, more accurate. 
{: .tips}

### Submitting a BWA job
Before we submit a new job, we need to cancel your previous job. 
~~~
$ squeue -u <username>
$ scancel <jobid> 
$ squeue -u <username>
~~~
{: .language-bash}

To submit the job to SLURM, `sbatch` command is used

~~~
$ sbatch bwa.sh 
~~~
{: .language-bash}

~~~
Submitted batch job <jobid>
~~~
{: .output}

### Checking the log file
Mapping will take about 3 hours. Let’s copy the output file from /blue/general_workshop/share/Suwannee/Polishing directory.

~~~
$ cp /blue/general_workshop/share/Suwannee/Polishing/Suw_index_58628076.out .
~~~
{: .language-bash}

~~~
$ ls
~~~
{: .language-bash}

~~~
bwa.sh  Suw_index_<jobid>.out  Suw_index_58628076.out 
~~~
{: .output}

~~~
$ head -n 17 Suw_index_58628076.out
~~~
{: .language-bash}

~~~
/blue/jeremybrawner/smithk/F_Cir/Suwannee2/mapping
c6a-s2.ufhpc
Mon Sep 14 18:36:05 EDT 2020
[bwa_index] Pack FASTA... 0.86 sec
[bwa_index] Construct BWT for the packed sequence...
[BWTIncCreate] textLength=93800066, availableWord=18599936
[BWTIncConstructFromPacked] 10 iterations done. 30680818 characters processed.
[BWTIncConstructFromPacked] 20 iterations done. 56678786 characters processed.
[BWTIncConstructFromPacked] 30 iterations done. 79781826 characters processed.
[bwt_gen] Finished constructing BWT in 37 iterations.
[bwa_index] 34.53 seconds elapse.
[bwa_index] Update BWT... 0.54 sec
[bwa_index] Pack forward-only FASTA... 0.46 sec
[bwa_index] Construct SA from BWT and Occ... 13.23 sec
[main] Version: 0.7.17-r1188
[main] CMD: bwa index /blue/jeremybrawner/smithk/F_Cir/Suwannee2/Smartdenovo/Suw.utg.fa
[main] Real time: 50.028 sec; CPU: 49.637 sec
~~~
{: .output}

~~~
$ tail Suw_index_58628076.out
~~~
{: .language-bash}

~~~
[M::process] read 3594 sequences (140002026 bp)...
[M::mem_process_seqs] Processed 3628 reads in 2522.069 CPU sec, 318.417 real sec
[M::process] read 3692 sequences (140067494 bp)...
[M::mem_process_seqs] Processed 3594 reads in 2457.334 CPU sec, 310.467 real sec
[M::process] read 2507 sequences (99040690 bp)...
[M::mem_process_seqs] Processed 3692 reads in 2483.281 CPU sec, 317.457 real sec
[M::mem_process_seqs] Processed 2507 reads in 1751.574 CPU sec, 224.243 real sec
[main] Version: 0.7.17-r1188
[main] CMD: bwa mem -t 14 -x ont2d /blue/jeremybrawner/smithk/F_Cir/Suwannee2/Smartdenovo/Suw.utg.fa /blue/jeremybrawner/smithk/F_Cir/Suwannee2/Suw2_filtered_3000bp_60X.fastq
[main] Real time: 8212.070 sec; CPU: 64626.020 sec
~~~
{: .output}

You will see how long does the program take to finish aligning Nnaopore reads to our assembly.

### Checking the output files

Constructing the index from the assembly only takes about a minute but alignment will take about an hours. Therefore, we will use pre-computed result to proceed to the next step.

~~~
$ head /blue/general_workshop/share/Suwannee/Polishing/Suw.sam
~~~
{: .language-bash}

~~~
@SQ     SN:utg1 LN:2979294
@SQ     SN:utg4 LN:3616438
@SQ     SN:utg7 LN:3191532
@SQ     SN:utg8 LN:4361335
@SQ     SN:utg10        LN:4900582
@SQ     SN:utg16        LN:791247
@SQ     SN:utg17        LN:1362414
@SQ     SN:utg20        LN:6384136
@SQ     SN:utg28        LN:349949
@SQ     SN:utg31        LN:1440739
~~~
{: .output}

## Run Racon
Let's copy a submission script template from /blue/general_workshop/share/bash_files directory. 

Check your current working directory. 
~~~
$ pwd
~~~
{: .language-bash}

If for some reasons you are not at /blue/general_workshop/username/Polishing, navigate yourself to the right directory.
~~~
$ cd /blue/general_workshop/<username>/Polishing
~~~
{: .language-bash}

~~~
$ cp /blue/general_workshop/share/bash_files/Racon.sh ./Racon.sh
~~~
{: .language-bash}

Once you copy the script to your working directory, we need to edit the script. 

~~~
$ nano Racon.sh
~~~
{: .language-bash}

~~~
-----------------------------------------------------------------------------------------------
 GNU nano 3.3 beta 02                     File: Racon.sh
-----------------------------------------------------------------------------------------------
#!/bin/bash
#SBATCH --job-name=Racon              # Job name
#SBATCH --account=general_workshop    # Account to run the computational task
#SBATCH --qos=general_workshop        # Account allocation
#SBATCH --mail-type=END,FAIL          # Mail events (NONE, BEGIN, END, FAIL, ALL)
#SBATCH --mail-user=<email_address>   # You need provide your email address
#SBATCH --ntasks=1                    # Run on a single CPU
#SBATCH --cpus-per-task=1
#SBATCH --mem=70gb                    # Job memory request
#SBATCH --time 12:00:00               # Time limit hrs:min:sec
#SBATCH --output=Racon_Suw_%j.out     # Standard output and error log

pwd; hostname; date

module load racon

racon -t 14 /blue/general_workshop/share/Suwannee/Suw2_filtered_3000bp_60X.fastq \
/blue/general_workshop/share/Suwannee/Polishing/Suw.sam \
/blue/general_workshop/share/Suwannee/Smartdenovo/Suw.utg.fa\
> ./SuwRacon.fasta


-----------------------------------------------------------------------------------------------
^G Get Help     ^O WriteOut     ^R Read File     ^Y Prev Page     ^K Cut Text       ^C Cur Pos
^X Exit         ^J Justify      ^W Where Is      ^V Next Page     ^U UnCut Text     ^T To Spell
-----------------------------------------------------------------------------------------------
~~~
{: .terminal}

### Checking usage of `racon`
~~~
$ racon -h
~~~
{: .language-bash}

~~~
usage: racon [options ...] <sequences> <overlaps> <target sequences>

    #default output is stdout
    <sequences>
        input file in FASTA/FASTQ format (can be compressed with gzip)
        containing sequences used for correction
    <overlaps>
        input file in MHAP/PAF/SAM format (can be compressed with gzip)
        containing overlaps between sequences and target sequences
    <target sequences>
        input file in FASTA/FASTQ format (can be compressed with gzip)
        containing sequences which will be corrected

    options:
        -t, --threads <int>
            default: 1
            number of threads
        -h, --help
            prints the usage
~~~
{: .output}

Change the &lt;email_address&gt; to your email address where you can check email. Once you are done, press <kbd>Ctrl</kbd>+<kbd>x</kbd> to return to bash prompt. Press <kbd>Y</kbd> and <kbd>Enter</kbd> to save the changes made to the file.

### Submitting a Racon job

Before we submit a new job, we need to cancel your previous job. 
~~~
$ squeue -u <username>
$ scancel <jobid> 
$ squeue -u <username>
~~~
{: .language-bash}

To submit the job to SLURM, `sbatch` command is used
~~~
$ sbatch Racon.sh 
~~~
{: .language-bash}

~~~
Submitted batch job <jobid>
~~~
{: .output}

### Checking the log file
Correction will take about 30 minutes. Let’s copy the output file from /blue/general_workshop/share/Suwanee/Polishing directory.

~~~
$ cp /blue/general_workshop/share/Suwannee/Polishing/Racon_Suw_58629660.out .
~~~
{: .language-bash}

~~~
$ ls
~~~
{: .language-bash}

~~~
bwa.sh    Racon_Suw_<jobid>.out   Suw_index_<jobid>.out   SuwRacon.fasta
Racon.sh  Racon_Suw_58629660.out  Suw_index_58628076.out  Suw.sam
~~~
{: .output}

~~~
$ cat Racon_Suw_58629660.out
~~~
{: .language-bash}

~~~
/blue/jeremybrawner/smithk/F_Cir/Suwannee2/mapping
c2a-s22.ufhpc
Mon Sep 14 21:03:13 EDT 2020
[racon::Polisher::initialize] loaded target sequences 0.962586 s
[racon::Polisher::initialize] loaded sequences 57.437520 s
[racon::Polisher::initialize] loaded overlaps 40.405221 s
[racon::Polisher::initialize] aligning overlaps [====================] 19.403566 s
[racon::Polisher::initialize] transformed data into windows 15.280284 s
[racon::Polisher::polish] generating consensus [====================] 1307.944380 s
[racon::Polisher::] total = 1442.492319 s
~~~
{: .output}

Racon will take about 30 minutes to finish correcting our assembly. We might not have enough time to finish the racon program. That is alright. We have also provided pre-computed output (corrected draft assembly) at /blue/general_workshop/share/Suwanee2/Polishing.

# Assemblely polishing using Pilon
[Pilon](https://github.com/broadinstitute/pilon/wiki) is a software aims to automatically improve draft assembly. Pilon requires two inputs: a FASTA file of assembly and BAM file of Illumina reads aligned to the assembly. Pilon identifies inconsistencies between the assembly and reads. Pilon aims to improve the input assembly including: 1) Single base differences, 2) Small indels, 3) Larger indel or block substitution events, 4) Gap filling, and 5) Identification of local misassemblies. Outputs from Pilon tool are: a FASTA file of improved assembly and optional VCF file to visulaize the discrepancy between FASTA file of assembly and Illumina reads.

## Polishing Racon-correcred assembly using Pilon
Three major steps are involved: 
1. Align Illumina reads to Racon-correcred assembly using BWA 
2. Covert SAM file to BAM file and sort the bam file based on reads position in the Racon assembly
3. Correcting assembly using Pilon using sorted BAM file consisting of Illumina paired-end alignments, aligned to the Racon-corrected assembly.

### Run Pilon 
Let's create new directory in your work directory and copy a submission script template from /blue/general_workshop/share/bash_files directory.

~~~
$ cd /blue/general_workshop/<username>/Polishing

$ mkdir Pilon

$ cd Pilon

$ cp /blue/general_workshop/share/bash_files/pilonRound1.sh .
~~~
{: .language-bash}

Once you copy the script to your working directory, we need to edit the script. 

~~~
$ nano pilonRound1.sh
~~~
{: .language-bash}

~~~
-----------------------------------------------------------------------------------------------
 GNU nano 3.3 beta 02                     File: pilonRound1.sh
-----------------------------------------------------------------------------------------------
#!/bin/bash
#SBATCH --job-name=Pilon                    # Job name
#SBATCH --account=general_workshop          # Account to run the computational task
#SBATCH --qos=general_workshop              # Account allocation
#SBATCH --mail-type=END,FAIL                # Mail events (NONE, BEGIN, END, FAIL, ALL)
#SBATCH --mail-user=<email_address>         # You need provide your email address
#SBATCH --ntasks=1                          # Run on a single CPU
#SBATCH --cpus-per-task=1
#SBATCH --mem=25gb                          # Job memory request
#SBATCH --time 12:00:00                     # Time limit hrs:min:sec
#SBATCH --output=SuwGenome_pilon_%j.out     # Standard output and error log

pwd; hostname; date

module load bwa/0.7.17 samtools/1.15 pilon/1.24

# First create index of Racon-corrected assembly.
bwa index /blue/general_workshop/share/Suwannee/Polishing/SuwRacon.fasta

# Two steps here: 
# 1. mapping illumina reads to Racon-corrected assembly
# 2. sorted outout SAM file into BAM file sorted by the mapping evidence
bwa mem -t 14 /blue/general_workshop/share/Suwannee/Polishing/SuwRacon.fasta \
/blue/general_workshop/share/Suwannee/TrimFastX_R1.fq \
/blue/general_workshop/share/Suwannee/TrimFastX_R2.fq | 
samtools view - -Sb | samtools sort - -@14 -o pilon1.sorted.bam

# Create index of mapping evidence
samtools index pilon1.sorted.bam

# Specifying Java options and system properties
export _JAVA_OPTIONS="-Xmx45g"

# Run pilon 
pilon --genome /blue/general_workshop/share/Suwannee/Polishing/SuwRacon.fasta \
--fix all --changes --frags pilon1.sorted.bam --threads 8 --output Suw_pilon1 \
--outdir /blue/general_workshop/<username>/Pilon



-----------------------------------------------------------------------------------------------
^G Get Help     ^O WriteOut     ^R Read File     ^Y Prev Page     ^K Cut Text       ^C Cur Pos
^X Exit         ^J Justify      ^W Where Is      ^V Next Page     ^U UnCut Text     ^T To Spell
-----------------------------------------------------------------------------------------------
~~~
{: .terminal}

### Checking usage of `samtools view`
~~~
$ module load samtools 
$ samtools view
~~~
{: .language-bash}

~~~
Usage: samtools view [options] <in.bam>|<in.sam>|<in.cram> [region ...]
Output options:
  -b, --bam                  Output BAM
General options:
  -S           Ignored (input format is auto-detected)
~~~
{: .output}
**Note**: Due to the page size limitation, only partial options are shown.

~~~
samtools sort 
~~~
{: .language-bash}

~~~
Usage: samtools sort [options...] [in.bam]
Options:
-o FILE    Write final output to FILE rather than standard output
-@, --threads INT
               Number of additional threads to use [0]
~~~
{:. output}
**Note**: Due to the page size limitation, only partial options are shown.

~~~
module load pilon 
pilon --help
~~~
{: .language-bash}

~~~
Usage: pilon --genome genome.fasta [--frags frags.bam] [--jumps jumps.bam] [--unpaired unpaired.bam]
                 [...other options...]
--genome genome.fasta
              The input genome we are trying to improve, which must be the reference used
              for the bam alignments.  At least one of --frags or --jumps must also be given.
--frags frags.bam
              A bam file consisting of fragment paired-end alignments, aligned to the --genome
              argument using bwa or bowtie2.  This argument may be specifed more than once.
 --changes
              If specified, a file listing changes in the <output>.fasta will be generated.
--fix fixlist
              A comma-separated list of categories of issues to try to fix:
                "snps": try to fix individual base errors;
                "indels": try to fix small indels;
                "gaps": try to fill gaps;
                "local": try to detect and fix local misassemblies;
                "all": all of the above (default);
                "bases": shorthand for "snps" and "indels" (for back compatibility);
                "none": none of the above; new fasta file will not be written.
~~~
{:. output}

Change the &lt;email_address&gt; to your email address where you can check email. Once you are done, press <kbd>Ctrl</kbd>+<kbd>x</kbd> to return to bash prompt. Press <kbd>Y</kbd> and <kbd>Enter</kbd> to save the changes made to the file.

### Submitting a Pilon job
Before we submit a new job, we need to cancel your previous job. 
~~~
$ squeue -u <username>
$ scancel <jobid> 
$ squeue -u <username>
~~~
{: .language-bash}

To submit the job to SLURM, `sbatch` command is used

~~~
$ sbatch pilonRound1.sh
~~~
{: .language-bash}

~~~
Submitted batch job <jobid>
~~~
{: .output}


## Further polishing Pilon assembly using Pilon
Simpliy repeat the previous steps. Mapping Illumina reads to 1st Pilon-polished assembly to obtained 2nd Pilon-polished assembly. 

Pilon program takes about 30 minutes. We might not be able to finish. We can copy the pre-computed Pilon-corrected assembly from 

# Scaffolding: mapping the polished assembly to the reference genome
RagTag is a collection of software tools for scaffolding and improving genome assemblies. RagTag performs:
1. Homology-based misassembly correction
2. Homology-based assembly scaffolding and patching
3. Scaffold merging

> ## RagTag
> <img src="https://raw.githubusercontent.com/malonge/RagTag/master/logo/descriptive_diagram.svg" align="center" width="900">
> 1. Correction: Ragtag identies and correct protential misaassembles in the query assembly using referecne genome.
> 2. Scaffold: Ragtag orders and orients draft query assembly into longer sequences using 
> 3. Patch:
> 4. Merge: 
{: .tips}


[Rephrase: Scaffold]
Scaffolding is the process of ordering and orienting draft assembly (query) sequences into longer sequences. Gaps (stretches of "N" characters) are placed between adjacent query sequences to indicate the presence of unknown sequence. RagTag uses whole-genome alignments to a reference assembly to scaffold query sequences. RagTag does not alter input query sequence in any way and only orders and orients sequences, joining them with gaps.

[Rephrase: Patch]
RagTag 'patch' uses one genome assembly to "patch" another genome assembly. We define two types of patches: Fills and Joins:

1. Fills are patches that fill assembly gaps. This process is like traditional gap-filling, though it uses an assembly instead of WGS sequencing reads.

2. Joins are patches that join distinct contigs. This is essentially scaffolding and gap-filling in a single step.

[Rephase: Merge]
RagTag merge is a tool to merge and reconcile different scaffoldings of the same assembly. In this way, one can leverage the advantages of multiple techniques to synergistically improve scaffolding.






# Quality assessment of assemblies
## Computing metrics of assemblies using QUAST
To perform assembly evaluation, we will run QUAST. QUAST computes serveral common metrics including misassemblies, contig N50, genome fraction (aligned based in the assemblies/reference genome). QUAST provides several outputs including report.txt, assessement summary in plain text file, and HTML file, a report including interactive plots. You can read the complete manual [here](http://quast.sourceforge.net/docs/manual.html#sec3). 




## Evaluate assembly completeness using BUSCO
A measure for quantitative assessment of genome assembly and annotation completeness based on evolutionarily informed expectations of gene content was proposed. A oopen-source software, with sets of Benchmarking Universal Single-Copy Orthologs, named BUSCO, is avalible [(Simao et al., 2015)](https://doi.org/10.1093/bioinformatics/btv351). 

### Run BUSCO analysis
First, create a BUSCO folder at your home folder, then copy the configuration of BUSCO containing all the required dependencies from /blue/general_workshop/share/BUSCO/augustus. 

~~~
$ mkdir BUSCO
$ cd BUSCO
$ cp -r /blue/general_workshop/share/BUSCO/augustus /blue/general_workshop/<username>/
$ cp /blue/general_workshop/share/bash_files/busco.sh ./busco.sh
~~~
{: .language-bash}

### Adding information to SLURM script

We have to modify some information in the template to provide more information to SLURM about the job.

~~~
$ nano busco.sh
~~~
{: .language-bash}

~~~
-----------------------------------------------------------------------------------------------
 GNU nano 3.3 beta 02                     File: busco.sh
-----------------------------------------------------------------------------------------------
#!/bin/bash
#SBATCH --job-name=BUSCO              # Job name
#SBATCH --account=general_workshop    # Account to run the computational task
#SBATCH --qos=general_workshop        # Account allocation
#SBATCH --mail-type=END,FAIL          # Mail events (NONE, BEGIN, END, FAIL, ALL)
#SBATCH --mail-user=<email_address>   # You need provide your email address
#SBATCH --ntasks=1                    # Run on a single CPU
#SBATCH --cpus-per-task=1
#SBATCH --mem=2gb                    # Job memory request
#SBATCH --time 1:00:00               # Time limit hrs:min:sec
#SBATCH --output=BUSCO_%j.out         # Standard output and error log

pwd; hostname; date

module load busco/5.3.0

busco -f -i /blue/general_workshop/share/Suwannee/Polishing/ragtag/ragtag_output/ragtag.scaffolds.fasta\
-o BUSCO_out_Suw --lineage_dataset hypocreales_odb10 -m genome 




-----------------------------------------------------------------------------------------------
^G Get Help     ^O WriteOut     ^R Read File     ^Y Prev Page     ^K Cut Text       ^C Cur Pos
^X Exit         ^J Justify      ^W Where Is      ^V Next Page     ^U UnCut Text     ^T To Spell
-----------------------------------------------------------------------------------------------
~~~
{: .terminal}

### Checking usage of `busco`
~~~
$ module load busco/5.3.0  
$ busco -h
~~~
{: .language-bash}

~~~
usage: busco -i [SEQUENCE_FILE] -l [LINEAGE] -o [OUTPUT_NAME] -m [MODE] [OTHER OPTIONS]

optional arguments:
  -i FASTA FILE, --in FASTA FILE
                        Input sequence file in FASTA format. Can be an assembled genome or transcriptome (DNA), or protein sequences from an annotated gene set.
-o OUTPUT, --out OUTPUT
                        Give your analysis run a recognisable short name. Output folders and files will be labelled with this name. WARNING: do not provide a path
-l LINEAGE, --lineage_dataset LINEAGE
                        Specify the name of the BUSCO lineage to be used.
-m MODE, --mode MODE  Specify which BUSCO analysis mode to run.
                        There are three valid modes:
                        - geno or genome, for genome assemblies (DNA)
~~~
{: .output}


Change the &lt;email_address&gt; to your email address where you can check email.
Once you are done, press <kbd>Ctrl</kbd>+<kbd>x</kbd> to return to bash prompt.
Press <kbd>Y</kbd> and <kbd>Enter</kbd> to save the changes made to the file.

## Running a job in SLURM

### Submitting a BUSCO job

To submit the job to SLURM, `sbatch` command is used.

~~~
$ sbatch busco.sh
~~~
{: .language-bash}

~~~
Submitted batch job <jobid>
~~~
{: .output}

BUSCO analysis will take about an hour, so we prepared the pre-computed output. 

We will copy the output of BUSCO analyses from our assembly,refernece genome (GCA_024047395.1) and previous reference genome avalibled on NCBI (GCA_000497325.3). 
~~~
$ cp /blue/general_workshop/share/BUSCO/BUSCO_out_Suw/short_summary.specific.hypocreales_odb10.BUSCO_out_Suw.txt .
$ cp /blue/general_workshop/share/BUSCO/BUSCO_out_Fc_ref/short_summary.specific.hypocreales_odb10.BUSCO_out_Fc_ref.txt .
$ cp 
~~~
{: .language-bash}

~~~
$ cat short_summary.specific.hypocreales_odb10.BUSCO_out_Suw.txt
~~~
{: .output}

~~~
$ cat short_summary.specific.hypocreales_odb10.BUSCO_out_Fc_ref.txt
~~~
{: .output}

~~~
$ cat 
~~~
{: .output}

How to interpret the output?




> ## References and addtional reading
> 1. [SMARTdenovo](https://github.com/ruanjue/smartdenovo)
> 2. [Overlap Layout Consensus assembly](https://www.cs.jhu.edu/~langmea/resources/lecture_notes/assembly_olc.pdf)
> 3. [BWA](https://github.com/lh3/bwa)
> 4. [Racon](https://github.com/isovic/racon)
> 5. [Bowtie2](http://bowtie-bio.sourceforge.net/bowtie2/index.shtml)
> 6. [Pilon](https://github.com/broadinstitute/pilon/wiki)
> 7. [Ragtag](https://github.com/malonge/RagTag)
> 8. [QUAST](https://sourceforge.net/projects/quast/)
> 9. [BUSCO](https://gitlab.com/ezlab/busco#how-to-cite-busco)
> 10. [Comparative Genomics of Fusarium circinatum Isolates Used to Screen Southern Pines for Pitch Canker Resistance](https://doi.org/10.1094/mpmi-10-21-0247-r)
{: .tips}