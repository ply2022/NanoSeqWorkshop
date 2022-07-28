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

## Navigating 

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
$ cd blue/general_workshop | ls
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

### Creating, moving and copying directories and files

## Making a directory

Use `cd` to access the workshop directory with your username.

We will create a new directory using the command `mkdir` followed by the name of the new directory `newdir`. Use `ls` or `cd` to see or navigate to your new directory

~~~
$ mkdir newdir 
~~~
{: .language-bash}

## Linux path symbols

There are shortcuts to navigate directory paths in Linux. Here are a few:

- `..` represents the parent directory. We used this above with `cd` to move up to the parent directory from your current location.
- `.` represents the current directory.
- `/` represents the root directory.
- `~` represents the home directory.

We will use some of these shortcuts soon.

## Copying a file

You can copy a file with the command `cp` followed by the location of the file you want to copy and the location you want to put the new copy.

~~~
$ cp blue/general_workshop/share/file1.txt ./file1.txt
$ ls
~~~
{: .language-bash}

Note that we used the directory shorcut `./` to represent that the new file should be moved to your current directory. You could write out the full name of any other directory to have it moved there, or to put it in the directory you just created, you could change the location to `./newdir/file1.txt`.

You can change the name of the file to anything you want when copying it. For example, we can copy the file again with a new name, but since it is in the current directory, we only need to list the name of the file.

~~~
$ cp file1.txt file2.txt
$ ls
~~~
{: .language-bash}



Be careful when copying files that a file does not exist with the same name in that directory, or it will replace it with the new copy.

You can also use the `cp` command with the `-r` option to perform a recursive copy, which can copy entire directories and all of the subdirectories and folders.

~~~
$ cp -r blue/general_workshop/share/demo ./
~~~
{: .language-bash}

## Moving a file

You can move files with the command `mv`, followed by the file location and the location where you will move it. Moving a file removes it from its original location.

~~~
$ mv file2.txt newfile.txt
$ ls
~~~
{: .language-bash}

~~~
***output***
~~~
{: .output}

## Deleting files

You can delete files using the command `rm`. To delete a directory and all of the files contained within it, you can use the recursive option `-r`.

Be careful! Linux does not have a trash can like Windows and Mac operating systems. Once you delete a file, it is permanently erased!

~~~
$ rm newfile.txt
$ ls
~~~
{: .language-bash}

## Deleting directories

Aside from using the recursive option for `rm`, you can also remove directories with the command `rmdir`.

~~~
$ rmdir newdir
$ ls
~~~
{: .language-bash}

### Reading and writing files

## Reading file contents

There are a number of ways to look at some or all of the contents of a file.

The command `cat` reads the contents of a file. We can use this to view ` file1.txt`.

~~~
$ cat file1.txt
~~~
{: .language-bash}

~~~
***output***
~~~
{: .output}

Rather than viewing all of the content of a large file, you can use the commands `less` and `more` to view small pieces of the document at a time.



## Learn more about unix commands
> [Software carpentary reference](https://swcarpentry.github.io/shell-novice/reference/)
{: .notes}
