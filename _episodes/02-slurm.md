---
title: "SLURM workload manager"
teaching: 10
exercises: 20
questions:
- How can I submit job to the cluster computer?
objectives:
- Run unix commands and scripts on Hipergator
keypoints:
- Don't forget to edit the memory, CPU requested, and email in the SLURM request. 
- You can submit jobs that are multithreaded, hybrid, with a GPU, and with an array depending on the job you are running.

---

## HiPerGator and SLURM

### What is SLURM?

High performance computing clusters have resources that are accessed by many different users at the same time. Oftentimes, the resource demands will exceed those available on the computer cluster, so a scheduling framework is used to arbitrate resource management. HiPerGator uses a scheduler called SLURM, which stands for Simple Linux Utility for Resource Management. Other scheduling systems exist, but we won't be covering their usage.

SLURM executes jobs from scripts, which we will learn how to format. Scripts are simply small text files that tell the scheduler who you are, what resources you are requesting for use, and a list of commands to submit that require large amounts of computational resources.

The previous exercise only required a small amount of computational resources, so we received results immediately after we submitted the command. Many bioinformatics applications use lots of resources, such as multiple CPUs and large amounts of memory for hours or even days. Learning to work with a SLURM scheduler has a number of benefits:

1. After you submit a job with a SLURM script, you can continue to use the HPC and submit additional jobs. Using the shell, you must wait for a task to complete before submitting another one.
2. You can submit a job to SLURM and disconnect from the HPC and the scheduler will automatically run the job while you do other work.
3. You can run jobs that take days to complete without tying up the resources of a personal computer.
4. You can access large numbers of CPUs, memory and GPUs that are not typically available on a personal computer.

### Using Nano to write scripts

In the previous section, we covered creating and manipulating files, but we have not yet produced our own files from scratch. Linux has a basic text editing application called `nano` that we will use to produce SLURM scripts.

You can use `nano` by typing the command `nano` followed by either the file name you want to create, or the location of a text file you want to edit. We will write our own 
