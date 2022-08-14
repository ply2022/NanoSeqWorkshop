---
title: "Overview of Sequencing Results"
teaching: 5
exercises: 15
questions:
- How to interpret and use sequencing results?
objectives:
- Learn about the fast5 format.
keypoints:
- The fast5 files output by Nanopore are not readable, and we will use a basecaller to interpret the results in the next section.

---

# Overview of Sequencing Results

## The fast5 file

First go to your directory and then follow the code below to copy a fast5 file to that location:

~~~
cp /blue/general_workshop/share/Suwannee/20200808_1457_MN33357_FAL75110_031a874e/fast5/FAL75110_0b0467ce_152.fast5 ./FAL75110_0b0467ce_152.fast5
~~~
{: .language-bash}

Nanopore sequencing will produce a large number of fast5 files. You cannot view fast5 files like a normal file, and it requires a special application to read the file encoding. You can view one now if you like, or just look at the output below.

~~~
head FAL75110_0b0467ce_152.fast5
~~~
{: .language-bash}

~~~

▒HDF
                                                                                                                                                            ▒▒▒▒▒▒▒▒XWR!▒▒▒▒▒▒▒▒`▒▒
▒PTREE▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒HEAP▒PXTREE▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒HEAP▒P▒▒                                                                                                    P@
▒TREE▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒HEAPP
H                                                                                                                                                             TREE▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒HEAP▒
▒'▒/▒7▒?▒▒▒▒    ▒        PFSSE▒ ▒▒▒▒▒▒
▒-▒xqR▒.▒▒/▒O▒_▒o1▒▒▒C▒= B▒ 9{FHIBu▒▒L▒▒L▒▒L▒▒L▒▒L▒▒L▒▒L▒▒L▒▒L▒▒L▒LűLɱLѱLٱL▒L▒]nq]~q]▒q▒
      ▒R
        ֌%▒6h▒MK▒▒6▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒08-09T04:12:13Z
8                                                                                                                                                            model_type
         transducerP`TREE▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒HEAP▒P▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒                                                                                              Summary@h▒
return_statusFailedForceSkipped▒`TREE▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒HEAPP▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒                                                                                   Summary▒▒▒
return_statusFailedForceSkipped ▒TREE▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒HEAP▒Pu▒u
▒▒#TREE▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒HEAP▒P
file_version2.2
8                                                                                                                                                           file_type
       multi-read8Hh
asic_id
[jkonkol@login2 fast5]$ xterm-256colorxterm-256colorxterm-256colorxterm-256colorxterm-256colorxterm-256color
-bash: xterm-256colorxterm-256colorxterm-256colorxterm-256colorxterm-256colorxterm-256color: command not found
~~~
{: .output}

> If you accidently tried to look at the fast5 file using the command `cat`, your shell will continuously scroll through the random characters for a long period of time. Remember that you can press <kbd>Ctrl</kbd> <kdb>c</kbd> to terminate processes in the shell.
{: .tips}

You can instead use the application HDF5 to read the encoded file. You can load applications using the `module load` command followed by the application name. HiPerGator and other HPCs have a large number of applications installed that you can use. You can view a full list of applications available on HiPerGator here: https://help.rc.ufl.edu/doc/Applications

Each application listed has a help site with usage information, or at the very least a link to the applications manual. You can view the HDF5 help site here: https://help.rc.ufl.edu/doc/HDF5

~~~
module load hdf5
~~~
{: .language-bash}

You won't receive any output from this command, but if you do not get an error message, you have successfully loaded the application. Now we can call commands used by hdf5 in the shell. The command `h5dump` followed by the file name will list all of the contents of the fast5 file. Instead, we will just view the top few lines first:

~~~
h5dump FAL75110_0b0467ce_152.fast5 | head -n 10
~~~
{: .language-bash}

~~~

HDF5 "FAL75110_0b0467ce_152.fast5" {
GROUP "/" {
   ATTRIBUTE "file_type" {
      DATATYPE  H5T_STRING {
         STRSIZE 11;
         STRPAD H5T_STR_NULLTERM;
         CSET H5T_CSET_ASCII;
         CTYPE H5T_C_S1;
      }
      DATASPACE  SCALAR
      DATA {
      (0): "multi-read"
      }
   }
   ATTRIBUTE "file_version" {
      DATATYPE  H5T_STRING {
         STRSIZE 4;
         STRPAD H5T_STR_NULLTERM;
         CSET H5T_CSET_ASCII;
         CTYPE H5T_C_S1;
      }
      DATASPACE  SCALAR
      DATA {
      (0): "2.2"
      }
   }
   GROUP "read_0007076c-cdb5-4926-a23f-e4d10ed961ef" {
      ATTRIBUTE "pore_type" {
         DATATYPE  H5T_STRING {
            STRSIZE 8;
~~~
{: .output}

Fast5 files are divided into `Groups` or `Datasets`, which oftentimes have `Attributes` associated with them. The first GROUP is the root directory `"/`, followed by the individual read groups, such as `read_0007076c-cdb5-4926-a23f-e4d10ed961ef`. We will view a portion of the data associated with this individual reads using the command `h5ls` and the option `-d`:

~~~
h5ls -d FAL75110_0b0467ce_152.fast5/read_0007076c-cdb5-4926-a23f-e4d10ed961ef/Raw/Signal | head
~~~
{: .language-bash}

~~~
Signal                   Dataset {3624/Inf}
    Data:
        (0) 580, 527, 531, 502, 520, 507, 498, 510, 482, 491, 490, 495, 508, 436, 437, 440, 433, 442, 442, 427, 435, 440, 441, 436, 447, 431, 459, 422, 436,
        (29) 423, 439, 457, 443, 431, 435, 430, 423, 425, 429, 450, 458, 430, 429, 434, 426, 433, 437, 442, 452, 441, 438, 431, 434, 442, 450, 439, 442, 443,
        (58) 439, 430, 443, 440, 441, 433, 431, 425, 437, 425, 442, 433, 457, 463, 464, 465, 457, 441, 431, 445, 428, 437, 455, 423, 440, 427, 455, 445, 449,
        (87) 442, 450, 454, 451, 430, 439, 413, 415, 424, 436, 435, 435, 437, 436, 411, 420, 421, 437, 426, 431, 450, 436, 433, 430, 412, 429, 437, 428, 420,
        (116) 411, 427, 418, 454, 429, 438, 435, 444, 420, 442, 425, 429, 422, 434, 440, 421, 427, 427, 434, 427, 433, 439, 446, 425, 450, 442, 433, 416, 429,
        (145) 426, 435, 429, 426, 429, 439, 429, 449, 427, 443, 427, 427, 414, 425, 417, 434, 430, 445, 432, 443, 429, 415, 438, 438, 435, 443, 432, 434, 439,
        (174) 434, 432, 441, 430, 417, 438, 447, 436, 432, 432, 424, 436, 438, 434, 445, 443, 445, 434, 436, 435, 442, 444, 448, 428, 439, 429, 451, 449, 458,
        (203) 453, 463, 465, 449, 446, 451, 445, 451, 441, 438, 434, 441, 437, 436, 441, 432, 428, 439, 438, 452, 449, 440, 447, 450, 436, 450, 418, 436, 424,
~~~
{: .output}

These are the individual pico-amp signals measured by the Nanopore device, but unfortunately, this information is not very informative, especially considering that this data is provided for each read in the file. We can measure the number of reads in the file with the command `h5ls`, but instead of listing them all we will count the number of lines in the output:

~~~
h5ls FAL75110_0b0467ce_152.fast5 | wc -l
~~~
{: .language-bash}

~~~
4000
~~~
{: .output}

This file contains 4,000 different reads, each with a large amount of data associated with the electrical signals read by the Nanopore device. However, this is not the only fast5 file with raw data. Nanopore separates the results across a number of different files. Let's check how many files are in the directory:

~~~
ls /blue/general_workshop/share/Suwannee/20200808_1457_MN33357_FAL75110_031a874e/fast5/ | wc -l
~~~
{: .language-bash}

~~~
265
~~~
{: .output}

Overall, this is a tremendous amount of data that we really are not able to read. In the next section, we will use the application `Guppy` to convert these electrical signals into genetic information in a fastQ format. This format provides information about the actual sequence nucleotides `ATGC`, and it includes a code to indicate the certainty of this read by the basecaller. Sometimes sequence signals are difficult to interpret, so it is helpful for bioinformatics applications to know how certain the basecalls are. We will cover basecaller results and quality in the next section.
