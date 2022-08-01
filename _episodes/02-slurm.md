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

You can use `nano` by typing the command `nano` followed by either the file name you want to create, or the location of a text file you want to edit. We will write our own script called `slurm.sh`. The `sh` extension is used for SLURM scripts.

~~~
$ nano slurm.sh
~~~
{: .language-bash}

~~~
--------------------------------------------------------------------------------
GNU nano 2.3.1            File: slurm.sh
--------------------------------------------------------------------------------






--------------------------------------------------------------------------------
^G Get Help     ^O WriteOut     ^R Read File     ^Y Prev Page     ^K Cut Text       ^C Cur Pos
^X Exit         ^J Justify      ^W Where Is      ^V Next Page     ^U UnCut Text     ^T To Spell
--------------------------------------------------------------------------------
~~~
{: .output}

This has begun the nano application and you can now begin writing your script. Note the commands at the bottom of the screen. Use the <kbd>Ctrl</kbd> key instead of the `^` indicated at the bottom followed by the lower case letter on your keyboard to execute the command. For example, you can use <kbd>Ctrl</kbd> <kbd>o</kbd> to save the file, or `WriteOut`.

Next we will paste in a text block for the script. Afterwards we will review what each part of the script does.

~~~
#!/bin/bash
#
#SBATCH --job-name=slurm_job_test    # Job name, anything you want to track the job
#SBATCH --account=general_workshop    # Account to run the computational task
#SBATCH --qos=general_workshop        # Account allocation
#SBATCH --mail-type=END,FAIL          # Mail events (NONE, BEGIN, END, FAIL, ALL)
#SBATCH --mail-user=<email_address>   # Where to send mail  
#SBATCH --ntasks=1                    # Run on a single CPU
#SBATCH --cpus-per-task=1             # Can increase above 1 CPU for multi-threaded processes
#SBATCH --mem=10mb                    # Job memory request
#SBATCH --time=00:05:00               # Time limit hrs:min:sec
#SBATCH --output=slurm_test_%j.log   # Standard output and error log

# Return current date and time every 10 seconds for 6 times
for i in {0..5}; do printf '%s %s\n' "$(date)" ; sleep 10s; done
~~~
{: .terminal}

### SLURM script anatomy

The `#!/bin/bash` header tells the shell that it is running a bash script.

The `#` symbol represents a comment. These could be special notes for the SLURM scheduler, or they can be comments for the user. You can start a line with a comment or you can add a comment to the end of a command to include notes for yourself about how the command works.

The `#SBATCH --` comment format provides the SLURM scheduler with information to run your job. This includes the resources you want to request, your account information and which work group's resources you will be allocating for the job. There are more options than we have included here, but these are the most commonly used. Let's quickly review what these options do.

`--job-name` can be any name you want to use to identify your job.

`--account` is your user name on the HPC.

`--qos` refers to the work group whose resources you are using. These could be the same as your username, or they could be under a different account used by a lab or a research group.

`--mail-type` tells SLURM when to send you email updates about your job. This could be when the job begins, ends or if it fails. We have it set to email us when the job is complete or if it fails due to an error. It is always a good idea to ask for an email when a job ends when you first run a script, because this email will include information about the resource usage for the job. You may have requested 32 Gb of memory to run a job, but the report may inform you that only 2 Gb were needed. It is a good idea to check 

`--mail-user` is your email address where the `--mail-type` will be sent. Be sure to set this to your own email address in the script above.

`--ntasks` represents he number of tasks being performed. Usually this will be `1`, unless you are running multi-processing scripts e.g. with python, which allow you to split up tasks for faster processing.

`--cpus-per-task` is the number of CPUs you are allocacting for your job. Many bioinformatics applications allow for multi-threading, and will allow you to use multiple CPUs to increase performance speed. Using more CPUs does NOT always increase performance speed, and using a large number of CPUs when they are not needed will actually DECREASE the processing speed of a job. You should read the manual for new bioinformatics applications to determine if they support multi-threading, and if they have limits or recommendations on how many CPUs to use.

`--mem` is the amount of memory you are allocating for your job in the format of a number followed by `mb` for megabytes or `gb` for gigabytes. Including extra memory will not decrease performance, but you should not request resources that you will not use on the HPC because it is wasteful, and it could delay running your job because SLURM will not execute it until all of the resources you requested are available. If you request all of the memory allocated to your group, SLURM will make you wait until every job submitted prior to yours is complete before executing yours, which could delay it for days. Check the email report the first time you complete a job to determine how much memory was actually used, and include a little extra as a buffer for future jobs.

`--time` sets a time limit for the job in the format hours:minutes:seconds. HiPerGator and other HPCs can use the estimated time for a job to avoid scheduling maintenance in the middle of your job and interrupting long pipelines. After the first time you run a script, you can check the email with the log at the end of the process to determine how much time is needed with some extra time added as a buffer.

`--output` sets the name of an output file and error log. Some applications will include important information in the output log, and oftentimes this is the only place that will log the cause of an error if a script fails, so it is incredibly important. You can name the log anything you want, but the `%j` suffix before the `.log` extension will include the id number the job was assigned after you sent it to SLURM. You can send the logs to a specific directory if you want by including the directory name in the prefix, such as a directory called logs: `logs/serial_test_%j.log`. However, you must be sure that the directory exists, or your job will fail and you will not receive any feedback as to the reason why.

### Submitting a job

Now that you understand how the script works, we can go ahead and run the script you pasted into Nano. A comment indicates how the script works, which uses the `;` instead of line breaks to execute a series of commands.

Save the script with nano to return to the shell.

Submit the script to SLURM using the command `sbatch` followed by the name of the script.

~~~
$ sbatch slurm.sh
~~~
{: .language-bash}

~~~
Submitted batch job <job_id>
~~~
{: .output}

If you submit multiple jobs with the same name, the job id provided by SLURM can be used to track your jobs.

### Checking the status of a SLURM job

The command `squeue` allows you to check the status of a job. The `-u` argument followed by your username will list all of the jobs you have submitted. The `-A` argument lists all of the jobs running under a single account. If you have submitted a job and it has not run for an extended period of time, you should use the `-A` argument to determine if there are a large number of jobs scheduled ahead of yours.

~~~
$ squeue -u <username>
~~~
{: .language-bash}

~~~
    JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
  <jobid> hpg2-comp serial_j   <user>  R       0:07      1 c29a-s2
~~~
{: .output}

The status listed under `ST` will either be `R` for running, or `PD` for pending. The last column will include reasons a job is pending.
- `None` means that the SLURM scheduler is preparing to process the job
- `Priority` means that jobs with higher priority that were submitted before yours are running
- `QOSMaxCpuPerUserLimit` means that the user is requesting more CPUs than what are available.

Now lets look at all of the jobs submitted by users in this workshop.

~~~
$ squeue -A general_workshop
~~~
{: .language-bash}

The results will vary, but will look something like:

~~~
    JOBID PARTITION     NAME      USER  ST       TIME  NODES NODELIST(REASON)
  <jobid> hpg2-comp serial_j   <user1>  PD       0:00      1 c29a-s2
  <jobid> hpg2-comp serial_j   <user2>   R       1:12      1 c15a-s1
  <jobid> hpg2-comp serial_j   <user3>   R       0:46      1 c09a-s4
~~~
{: .output}

You can view a log of all of the jobs that you have submitted in one day using the command `sacct`. This will give you a list that includes jobs that are running, completed or have failed. If you are running a large number of jobs it is useful as a quick status check.

~~~
$ sacct
~~~

Now we can look at the results of our SLURM script. List the files in your directory and choose the log file from the script you submitted. Don't copy the last line provided below, instead use ls to find the name of the file because the `<jobid>` will be different for each user.

~~~
$ ls
$ cat slurm_test_<jobid>.log
~~~
{: .langage-bash}

~~~
Sat Sep 06  02:04:05 EDT 2022
Sat Sep 06 02:04:15 EDT 2022
Sat Sep 06 02:04:25 EDT 2022
Sat Sep 06 02:04:35 EDT 2022
Sat Sep 06 02:04:45 EDT 2022
Sat Sep 06 02:04:55 EDT 2022
~~~
{: .output}

The output of our commands were written to the log file. Applications may submit results in the log file, but this will vary by application, and some may not write anything to it. Including some header and footer information for each job can be helpful. For example, you can include the command `pwd; hostname; date` at the beginning of a script, and `date` at the end of the script to frame the results in the log file. Then, if an application does not include any feedback, you will at least see if the script was completed.

