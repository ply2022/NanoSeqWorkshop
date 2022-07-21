---
title: "Connecting to Hipergator and Using Basic Linux Commands"
teaching: 10
exercises: 20
questions:
- How can I submit job to the cluster computer?
objectives:
- Run HiperGator and learn to navigate the HPC with basic Linux commands
keypoints:
- There are many different ways to log on the HPC depending on your OS.
- Navigate using pwd, ls and cd
- Navigate text files using cat, more and less
- Search and extract data using sed and grep
- Combine commands for complex tasks
- Use Nano to edit text and write scripts
- You can maximize efficiency with loops

---

The HiperGator shell is a program with a command line interface that you will use using keyboard commands instead of a graphical interface. It is important to learn a few basic Linux commands to navigate HiperGator, and these simple commands can be combined in complex ways to produce very powerful results.

## Connecting to HiperGator

How are they doing this?

## Terminal Command Line

### Terminal basics

You can tell that you are logged on because the following should be displayed:

~~~
[<username>@login1 ~]$
~~~
{: .terminal}

The &lt;username&gt; will be your personal username. The `$` symbol indicates that you can input commands. When you submit a command and the `$` symbol disappears, any future commands that you enter will not be processed until after the previous command is complete. We can avoid long lag times before submitting a command or running an application using SLURM scripts, which we will discuss in the next section.

When using the scripts in this workshop, you should not copy the `$` symbol when it is present at the beginning of the line.

### Recommendations for naming files in a Linux 

- Never include spaces in file or directory names. Instead, use the `_` symbol instead.
- Linux is case sensitive, so it is advisable to avoid capitalization in file and directory names to avoid errors.
- When writing out the path to a file, Linux uses `/` instead of the `\` symbol used by Windows.
- File extensions are not as important for Linux compared to an OS like Windows, and you can use your own arbitrary file extensions to help you sort files.

## Basic Unix Commands

### Displaying your current path/location

`pwd` displays your "path", or your file location on HiperGator.

~~~
$ pwd
~~~
{: .language-bash}

### Changing directories

`cd` followed by a directory name will change your current directory. Try navigating to the folder we will use for the workshop.

~~~
$ cd /blue/***workshopname***/
~~~
{: .language-bash}

> Do not copy the `$` sign!
{: .caution}

Now try using `pwd` again to see your current location.

~~~
/blue/***workshopname***/
~~~
{: .output}

You can quickly go backwards from your current directory using the command `cd ..`

### Listing files in your current directory

You can use the command `ls` to display the file contents in your current folder.

~~~
$ ls
~~~
{: .language-bash}

~~~
***output***
~~~
{: .output}

The command `ls -l` will provide a list of additional information about the files such as the permissions, the size and the date it was created.


~~~
$ ls -l
~~~
{: .language-bash}

~~~
***output***
~~~
{: .output}

### Moving back in a directory and combining commands

You can easily go back one directory from your current location using the command `cd ..` and then `pwd` to display your location

~~~
$ cd ..
$ pwd
~~~
{: .language-bash}

~~~
***output***
~~~
{: .output}

You can also combine commands in Linux using the `|` symbol. For example, if you know you will want to change your directory and look up the file contents, you could use this command:

~~~
$ cd ***workshopname*** | ls
~~~
{: .language-bash}

~~~
***output***
~~~
{: .output}

## Permissions in Linux

You can determine a file's permissions by using the `ls -l` command provided above.

Linux permissions look like the following:  `d r w x r w - r - -`

The first letter indicates if the content is a file `-`, directory `d` or a link `l`.

There are three clusters of three characters that follow the first letter. These indicate different permissions, which are:
- read `r`, or the ability to view content in a folder or file.
- write `w`, or the ability to write content to a folder or file.
- execute `x`, or the ability run a folder or file. This could mean running a script or opening a folder.
- permission denied `-`, which can be substituted for any of the three listed above to indicate the invidual does not have permission for this type of access.

The three different clusters represent:
- 1) The permission of the owner of the file
- 2) The permission of a working group, such as a members of a lab or a research group
- 3) Permission for others outside your working group

It is important to look at permissions if you are having trouble accessing a file or folder. Sometimes content from workshops or files downloaded from the internet have permissions that can restrict your access.

### Creating directories

