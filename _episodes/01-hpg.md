---
title: "Connecting to Hipergator and Using Basic Linux Commands"
teaching: 10
exercises: 20
questions:
- How can I navigate the HPC environment and view data there?
objectives:
- Run HiperGator and learn to navigate the HPC environment with basic Linux commands
keypoints:
- Navigate using pwd, ls and cd
- Navigate text files using cat, more and less
- Search and extract data using sed and grep
- Combine commands for complex tasks
- You can maximize efficiency with loops

---

The HiperGator shell is a program with a command line interface that you will use using keyboard commands instead of a graphical interface. It is important to learn a few basic Linux commands to navigate HiperGator, and these simple commands can be combined in complex ways to produce very powerful results.

## Connecting to HiperGator

### Connecting to Gatorlink VPN
Gatorlink VPN is required to access University if Florida Research Computing OnDemand
service, which will provide access to HiperGator cluster.
1. Visit [UF VPN webpage (vpn.ufl.edu)](https://vpn.ufl.edu){: target="_blank"}.
2. Login using the username and password that was emailed to you.
3. In the next page, download **Cisco Anyconnect** software and install it.
4. Once installed, run **Cisco Anyconnect** and click **Connect**.
5. Enter the username and password provided and click **OK**. 
You should now be connected to Gatorlink VPN.

### Connecting to HiperGator
Make sure you are connected to Gatorlink VPN before the following steps.
1. In you web browser, navigate to [UFRC OnDemand (ood.rc.ufl.edu)](https://ood.rc.ufl.edu/){: target="_blank"}.
2. Login using provided username and password. You will be redirected to UFRC OnDemand homepage.
3. To connect to remote shell, click on `Clusters` in navbar and click `Hipergator Shell Access`. Note that the shell will open in new tab.
4. To view and transfer your files, click on `Files` in navbar in UFRC OnDemand homepage and click `/blue/general_workshop`. Double click on directory named as your username to find your files.

## Terminal Command Line

### Terminal basics

You can tell that you are logged on because the following should be displayed:

~~~
[<username>@login1 ~]$
~~~
{: .terminal}

The &lt;username&gt; will be your personal username. The `$` symbol indicates that you can input commands. When you submit a command and the `$` symbol disappears, any future commands that you enter will not be processed until after the previous command is complete. We can avoid long lag times before submitting a command or running an application using SLURM scripts, which we will discuss in the next section.

We have removed the `$` from the beginning of each line of code below so it is easier to copy and paste into the shell.

### Help! I made a typo or executed a large command!

We have provided commands in this workshop that you can copy and paste directly into the shell to avoid typos. But when you are using the shell on your own, or you accidently don't copy a portion of a command, your shell can execute a process indefinitely. Similarly, if you open an entire 100 Gb text file, the shell will print all of the text from the file and scroll from the top to the bottom until it is finished, which will take a very long time. You can avoid these situations by using the command <kbd>Ctrl</kbd> <kbd>c</kbd> command to terminate the last command you sent to the shell. Remember this command because eventually this happens to everyone.

### Recommendations for naming files in a Linux 

- Never include spaces in file or directory names. Instead, use the `_` symbol instead.
- Linux is case sensitive, so it is advisable to avoid capitalization in file and directory names to avoid errors.
- When writing out the path to a file, Linux uses `/` instead of the `\` symbol used by Windows.
- File extensions are not as important for Linux compared to an OS like Windows, and you can use your own arbitrary file extensions to help you sort files.

## Navigating 

### Displaying your current path/location

`pwd` displays your "path", or your file location on HiperGator. This workshop will include code that you can copy directly from this page and paste into the shell. Right clicking is the same as pasting in Linux, but your shell through UFRC On Demand does not support this feature. Instead, you will need to use the usual keyboard shortcut to paste with your operating system.

> We are using UFRC On Demand for this workshop because all users can use the same Secure SHell (SSH) application. There are many differnt free SSH applications for every operation system that combine the shell with a Graphical User Interface (GUI), and these support features such as editing files in separate color coded text editors outside of the shell and easy file transfers and directory navigation. If you plan on doing a lot of bioinformatics work, we strongly recommend finding an application that works for your operating system. There are a number that are both free, but you should ensure it supports features required for accessing HiPerGator outside of UFRC On Demand, such as 2-factor authentication.
{: .tips}

~~~
pwd
~~~
{: .language-bash}

### Changing directories

`cd` followed by a directory name will change your current directory. Try navigating to the folder we will use for the workshop.

~~~
cd /blue/general_workshop/
~~~
{: .language-bash}

Now try using `pwd` again to see your current location. Instead of copying from this script or typing it out, you can highlight `pwd` in the shell by double clicking it and then pasting it into the shell. Long or complicated file and directory names can be time consuming to type out and it is easy to introduce errors. It is much easier to list the name of the files (see below) and copy and paste them in the shell using this method. Outside of UFRC On Demand, this is as simple as a double click followed by a right click.

~~~
/blue/general_workshop/
~~~
{: .output}

Try changing the directory to 

~~~
/blue/general_workshop/<username>
pwd
~~~
{: .language-bash}

~~~
/blue/genera_workshop/<username>
~~~
{: .output}


You can quickly go backwards from your current directory using the command `cd ..`

~~~
cd ..
pwd
~~~
{: .language-bash}

~~~
/blue/genera_workshop
~~~
{: .output}

### Listing files in your current directory

You can use the command `ls` to display the file contents in your current folder.

~~~
ls
~~~
{: .language-bash}

~~~
guest.12682  guest.12685  guest.12689  guest.12693  guest.12697  guest.12701  guest.12706  guest.12709  guest.12712  share
guest.12683  guest.12687  guest.12690  guest.12694  guest.12699  guest.12704  guest.12707  guest.12710  guest.12713
guest.12684  guest.12688  guest.12691  guest.12696  guest.12700  guest.12705  guest.12708  guest.12711  plyu
~~~
{: .output}

The command `ls -l` will provide a list of additional information about the files such as the permissions, the size and the date it was created.


~~~
ls -l
~~~
{: .language-bash}

~~~
***output***
~~~
{: .output}

### Moving back in a directory and combining commands

You can easily go back one directory from your current location using the command `cd ..` and then `pwd` to display your location

~~~
cd ..
pwd
~~~
{: .language-bash}

~~~
drwxr-s--- 2 guest.12682 general_workshop 4096 Aug  1 16:39 guest.12682
drwxr-s--- 2 guest.12683 general_workshop 4096 Aug  1 16:41 guest.12683
drwxr-s--- 2 guest.12684 general_workshop 4096 Aug  1 16:42 guest.12684
drwxr-s--- 2 guest.12685 general_workshop 4096 Aug  1 16:44 guest.12685
drwxr-s--- 2 guest.12687 general_workshop 4096 Aug  1 16:46 guest.12687
drwxr-s--- 2 guest.12688 general_workshop 4096 Aug  1 16:47 guest.12688
drwxr-s--- 2 guest.12689 general_workshop 4096 Aug  1 16:48 guest.12689
drwxr-s--- 2 guest.12690 general_workshop 4096 Aug  1 16:48 guest.12690
drwxr-s--- 2 guest.12691 general_workshop 4096 Aug  1 16:49 guest.12691
drwxr-s--- 2 guest.12693 general_workshop 4096 Aug  1 16:51 guest.12693
drwxr-s--- 2 guest.12694 general_workshop 4096 Aug  1 16:52 guest.12694
drwxr-s--- 2 guest.12696 general_workshop 4096 Aug  1 16:53 guest.12696
drwxr-s--- 2 guest.12697 general_workshop 4096 Aug  1 16:54 guest.12697
drwxr-s--- 2 guest.12699 general_workshop 4096 Aug  1 16:56 guest.12699
drwxr-s--- 2 guest.12700 general_workshop 4096 Aug  1 16:56 guest.12700
drwxr-s--- 2 guest.12701 general_workshop 4096 Aug  1 16:57 guest.12701
drwxr-s--- 2 guest.12704 general_workshop 4096 Aug  1 16:59 guest.12704
drwxr-s--- 2 guest.12705 general_workshop 4096 Aug  1 17:00 guest.12705
drwxr-s--- 2 guest.12706 general_workshop 4096 Aug  1 17:01 guest.12706
drwxr-s--- 2 guest.12707 general_workshop 4096 Aug  1 17:02 guest.12707
drwxr-s--- 2 guest.12708 general_workshop 4096 Aug  1 17:03 guest.12708
drwxr-s--- 2 guest.12709 general_workshop 4096 Aug  1 17:03 guest.12709
drwxr-s--- 2 guest.12710 general_workshop 4096 Aug  1 17:04 guest.12710
drwxr-s--- 2 guest.12711 general_workshop 4096 Aug  1 17:05 guest.12711
drwxr-s--- 2 guest.12712 general_workshop 4096 Aug  2 23:52 guest.12712
drwxr-s--- 2 guest.12713 general_workshop 4096 Aug  1 17:07 guest.12713
drwxr-sr-x 5 plyu        general_workshop 4096 Aug  3 02:09 plyu
drwxrws--- 5 jhuguet     general_workshop 4096 Aug  3 02:16 share
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

## Creating, moving and copying directories and files

### Making a directory

Use `cd` to access the workshop directory with your username.

We will create a new directory using the command `mkdir` followed by the name of the new directory `newdir`. Use `ls` or `cd` to see or navigate to your new directory

~~~
cd /blue/general_workshop/<username>
mkdir newdir
ls
~~~
{: .language-bash}

~~~
newdir
~~~
{: .output}

### Linux path symbols

There are shortcuts to navigate directory paths in Linux. Here are a few:

- `..` represents the parent directory. We used this above with `cd` to move up to the parent directory from your current location.
- `.` represents the current directory.
- `/` represents the root directory.
- `~` represents the home directory.

We will use some of these shortcuts soon.

### Copying a file

You can copy a file with the command `cp` followed by the location of the file you want to copy and the location you want to put the new copy.

~~~
cp blue/general_workshop/share/file1.txt ./file1.txt
ls
~~~
{: .language-bash}

~~~
file1.txt  newdir
~~~
{: .output}

Note that we used the directory shorcut `./` to represent that the new file should be moved to your current directory. You could write out the full name of any other directory to have it moved there, or to put it in the directory you just created, you could change the location to `./newdir/file1.txt`.

You can change the name of the file to anything you want when copying it. For example, we can copy the file again with a new name, but since it is in the current directory, we only need to list the name of the file.

~~~
cp file1.txt file2.txt
ls
~~~
{: .language-bash}

~~~
file1.txt  file2.txt  newdir
~~~
{: .output}

Be careful when copying files that a file does not exist with the same name in that directory, or it will replace it with the new copy.

You can also use the `cp` command with the `-r` option to perform a recursive copy, which can copy entire directories and all of the subdirectories and folders. We will copy a directory of scripts that we will use later on.

~~~
cp -r blue/general_workshop/share/bash_files  ./bash_files
ls
~~~
{: .language-bash}

~~~
bash_files  file1.txt  file2.txt  newdir
~~~
{: .output}

### Moving a file

You can move files with the command `mv`, followed by the file location and the location where you will move it. Moving a file removes it from its original location.

~~~
mv file2.txt newfile.txt
ls
~~~
{: .language-bash}

~~~
bash_files  file1.txt  newdir  newfile.txt
~~~
{: .output}

### Deleting files

You can delete files using the command `rm`. To delete a directory and all of the files contained within it, you can use the recursive option `-r`.

Be careful! Linux does not have a trash can like Windows and Mac operating systems. Once you delete a file, it is permanently erased!

~~~
rm newfile.txt
ls
~~~
{: .language-bash}

~~~
bash_files  file1.txt  newdir
~~~
{: .output}

### Deleting directories

Aside from using the recursive option for `rm`, you can also remove directories with the command `rmdir`.

~~~
rmdir newdir
ls
~~~
{: .language-bash}

~~~
bash_files  file1.txt
~~~

## Reading and writing files

### Reading file contents

There are a number of ways to look at some or all of the contents of a file.

The command `cat` reads the contents of a file. We can use this to view ` file1.txt`.

~~~
cat file1.txt
~~~
{: .language-bash}

~~~
CM008455.1      27261226        C       A       intergenic region
CM008455.1      289184259       AT      A       intron_variant
CM008455.1      32223878        A       AT      intergenic_region
CM008456.1      23376349        C       A       intergenic_region
CM008458.1      43844223        A       C       intergenic_region
CM008458.1      158326634       C       T       intergenic_region
CM008458.1      58282344        A       G       intergenic_region
CM008459.1      8234325823      C       T       intron_variant
CM008460.1      88293626        G       A       intergenic_region
CM008461.1      38822332        A       T       intron_variant
CM008461.1      92442234        AT      G       intergenic_region
CM008455.1      277261226       C       A       intergenic region
CM008455.1      281842259       AT      A       intron_variant
CM008455.1      38782342        A       AT      intergenic_region
CM008456.1      23373492        C       A       intergenic_region
CM008458.1      438423s3        A       C       intergenic_region
CM008458.1      15856323        C       T       intergenic_region
CM008458.1      58284234        A       G       intergenic_region
CM008459.1      8234532823      C       T       intron_variant
CM008460.1      8825932s        G       A       intergenic_region
CM008461.1      38832322        A       C       intron_variant
CM008461.1      924223543       AT      G       intergenic_region
CM008455.1      272612626       C       A       intergenic region
CM008455.1      2891864259      AT      A       intron_variant
CM008455.1      38783232        A       AT      intergenic_region
CM008456.1      23374492        C       A       intergenic_region
CM008458.1      43826323        A       C       intergenic_region
CM008458.1      158523234       C       T       intergenic_region
CM008458.1      58284234        A       G       intergenic_region
CM008459.1      8233432823      C       T       intron_variant
CM008460.1      88932234        G       A       intergenic_region
CM008461.1      38822238        A       C       intron_variant
CM008461.1      92223845        AT      G       intergenic_region
CM008455.1      27262265        C       A       intergenic region
CM008455.1      28918425        AT      A       intron_variant
CM008455.1      38746748        A       AT      intergenic_region
CM008456.1      23746639        C       A       intergenic_region
CM008458.1      43345723        A       C       intergenic_region
CM008458.1      18532342        C       T       intergenic_region
CM008458.1      58223434        A       G       intergenic_region
CM008459.1      82342823        C       T       intron_variant
CM008460.1      82932345        G       A       intergenic_region
CM008461.1      38232545        A       C       intron_variant
CM008461.1      92423734        AT      G       intergenic_region
CM008455.1      27612265        C       A       intergenic region
CM008455.1      28984259        AT      A       intron_variant
CM008455.1      3873835453      A       T       intergenic_region
CM008456.1      2334923453      C       A       intergenic_region
CM008458.1      38272333        A       C       intergenic_region
CM008458.1      15832356544     C       T       intergenic_region
CM008458.1      58283456544     A       G       intergenic_region
CM008459.1      8233286423      C       T       intron_variant
CM008460.1      882393232       G       A       intergenic_region
CM008461.1      388213332       A       C       intron_variant
CM008461.1      92427434        AT      G       intergenic_region
~~~
{: .output}

Rather than viewing all of the content of a large file, you can use the commands `less` and `more` to view small pieces of the document at a time.

Let's start with `less`. You can use the <kbd>&uparrow;</kbd> and <kbd>&downarrow;</kbd> keys to scroll up and down the text file. You can quit using `less` and return to the command prompt by pressing the <kbd>q</kbd> key.

~~~
less file1.txt
~~~
{: .language-bash}

~~~
CM008455.1      27261226        C       A       intergenic region
CM008455.1      289184259       AT      A       intron_variant
CM008455.1      32223878        A       AT      intergenic_region
CM008456.1      23376349        C       A       intergenic_region
CM008458.1      43844223        A       C       intergenic_region
CM008458.1      158326634       C       T       intergenic_region
CM008458.1      58282344        A       G       intergenic_region
CM008459.1      8234325823      C       T       intron_variant
CM008460.1      88293626        G       A       intergenic_region
CM008461.1      38822332        A       T       intron_variant
CM008461.1      92442234        AT      G       intergenic_region
CM008455.1      277261226       C       A       intergenic region
CM008455.1      281842259       AT      A       intron_variant
CM008455.1      38782342        A       AT      intergenic_region
CM008456.1      23373492        C       A       intergenic_region
CM008458.1      438423s3        A       C       intergenic_region
CM008458.1      15856323        C       T       intergenic_region
CM008458.1      58284234        A       G       intergenic_region
CM008459.1      8234532823      C       T       intron_variant
CM008460.1      8825932s        G       A       intergenic_region
CM008461.1      38832322        A       C       intron_variant
CM008461.1      924223543       AT      G       intergenic_region
CM008455.1      272612626       C       A       intergenic region
CM008455.1      2891864259      AT      A       intron_variant
CM008455.1      38783232        A       AT      intergenic_region
CM008456.1      23374492        C       A       intergenic_region
CM008458.1      43826323        A       C       intergenic_region
CM008458.1      158523234       C       T       intergenic_region
CM008458.1      58284234        A       G       intergenic_region
CM008459.1      8233432823      C       T       intron_variant
CM008460.1      88932234        G       A       intergenic_region
CM008461.1      38822238        A       C       intron_variant
CM008461.1      92223845        AT      G       intergenic_region
CM008455.1      27262265        C       A       intergenic region
CM008455.1      28918425        AT      A       intron_variant
CM008455.1      38746748        A       AT      intergenic_region
CM008456.1      23746639        C       A       intergenic_region
CM008458.1      43345723        A       C       intergenic_region
CM008458.1      18532342        C       T       intergenic_region
CM008458.1      58223434        A       G       intergenic_region
CM008459.1      82342823        C       T       intron_variant
file1.txt
~~~
{: .output}

Now try `more`. You can scroll down the file using the <kbd>enter</kbd> key. Again, use the <kbd>q</kbd> key to return to the command line.

~~~
more file1.txt
~~~
{: .language-bash}

~~~
CM008455.1      27261226        C       A       intergenic region
CM008455.1      289184259       AT      A       intron_variant
CM008455.1      32223878        A       AT      intergenic_region
CM008456.1      23376349        C       A       intergenic_region
CM008458.1      43844223        A       C       intergenic_region
CM008458.1      158326634       C       T       intergenic_region
CM008458.1      58282344        A       G       intergenic_region
CM008459.1      8234325823      C       T       intron_variant
CM008460.1      88293626        G       A       intergenic_region
CM008461.1      38822332        A       T       intron_variant
CM008461.1      92442234        AT      G       intergenic_region
CM008455.1      277261226       C       A       intergenic region
CM008455.1      281842259       AT      A       intron_variant
CM008455.1      38782342        A       AT      intergenic_region
CM008456.1      23373492        C       A       intergenic_region
CM008458.1      438423s3        A       C       intergenic_region
CM008458.1      15856323        C       T       intergenic_region
CM008458.1      58284234        A       G       intergenic_region
CM008459.1      8234532823      C       T       intron_variant
CM008460.1      8825932s        G       A       intergenic_region
CM008461.1      38832322        A       C       intron_variant
CM008461.1      924223543       AT      G       intergenic_region
CM008455.1      272612626       C       A       intergenic region
CM008455.1      2891864259      AT      A       intron_variant
CM008455.1      38783232        A       AT      intergenic_region
CM008456.1      23374492        C       A       intergenic_region
CM008458.1      43826323        A       C       intergenic_region
CM008458.1      158523234       C       T       intergenic_region
CM008458.1      58284234        A       G       intergenic_region
CM008459.1      8233432823      C       T       intron_variant
CM008460.1      88932234        G       A       intergenic_region
CM008461.1      38822238        A       C       intron_variant
CM008461.1      92223845        AT      G       intergenic_region
CM008455.1      27262265        C       A       intergenic region
CM008455.1      28918425        AT      A       intron_variant
CM008455.1      38746748        A       AT      intergenic_region
CM008456.1      23746639        C       A       intergenic_region
CM008458.1      43345723        A       C       intergenic_region
CM008458.1      18532342        C       T       intergenic_region
CM008458.1      58223434        A       G       intergenic_region
CM008459.1      82342823        C       T       intron_variant
--More--(74%)
~~~
{: .output}

### Previewing the top and bottom of a file

You can preview the top of a file using the command `head`, and you can include the arguement `-n` followed by a number to specify the number of lines to show. The example below will show the first 12 lines of the file.

~~~
head -n 12 file1.txt
~~~
{: .language-bash}

~~~

CM008455.1      27261226        C       A       intergenic region
CM008455.1      289184259       AT      A       intron_variant
CM008455.1      32223878        A       AT      intergenic_region
CM008456.1      23376349        C       A       intergenic_region
CM008458.1      43844223        A       C       intergenic_region
CM008458.1      158326634       C       T       intergenic_region
CM008458.1      58282344        A       G       intergenic_region
CM008459.1      8234325823      C       T       intron_variant
CM008460.1      88293626        G       A       intergenic_region
CM008461.1      38822332        A       T       intron_variant
CM008461.1      92442234        AT      G       intergenic_region
CM008455.1      277261226       C       A       intergenic region
~~~
{: .language-bash}

The command `tail` will show the end of the file, and can also be combined with `-n` to show a specific number of lines.

~~~
tail -n 7 file1.txt
~~~
{: .language-bash}

~~~
CM008458.1      38272333        A       C       intergenic_region
CM008458.1      15832356544     C       T       intergenic_region
CM008458.1      58283456544     A       G       intergenic_region
CM008459.1      8233286423      C       T       intron_variant
CM008460.1      882393232       G       A       intergenic_region
CM008461.1      388213332       A       C       intron_variant
CM008461.1      92427434        AT      G       intergenic_region
~~~
{: .output}

### Viewing the middle of a file

You can view lines from the middle of a file using the command `sed` along with the command `-n` followed by the number of the first and last lines you want to view separated by a `,` and with the letter `p` as a suffix.

~~~
sed -n 7,15p file1.txt
~~~
{: .language-bash}

~~~
CM008458.1      58282344        A       G       intergenic_region
CM008459.1      8234325823      C       T       intron_variant
CM008460.1      88293626        G       A       intergenic_region
CM008461.1      38822332        A       T       intron_variant
CM008461.1      92442234        AT      G       intergenic_region
CM008455.1      277261226       C       A       intergenic region
CM008455.1      281842259       AT      A       intron_variant
CM008455.1      38782342        A       AT      intergenic_region
CM008456.1      23373492        C       A       intergenic_region
~~~
{: .output}

### Checking the length of a file

The command `wc` can be combined with several arguments to measure the length of a file.

- `-l` outputs the number of lines
- `-c` outputs the number of characters
- `w` outputs the number of words

Try one of the three combinations below:

~~~
wc -l file1.txt
wc -c file1.txt
wc -w file1.txt
~~~
{: .language-bash}

~~~
55 file1.txt
2309 file1.txt
280 file1.txt
~~~
{: .output}


## Writing or concatenating output to files

### Writing files

A command followed by the `>` operator will send that output to a new file name after the symbol.

~~~
head -n 8 file1.txt > top.txt
tail -n 8 file1.txt > bottom.txt
ls
~~~
{: .language-bash}

~~~
bash_files  bottom.txt  file1.txt  top.txt
~~~
{: .output}

The command `echo` will send text to the command prompt. This is more useful when it is combined with the `>` operator or other functions.

~~~
echo hello
~~~
{: .language-bash}

~~~
hello
~~~
{: .output}

Now use this function with the `>` operator. We will include `.move` in the name to demonstrate how you can use extensions to manipulate files later on.

~~~
echo hello > hi.move.txt
echo goodbye > bye.move.txt
ls
~~~
{: .language-bash}

~~~
bash_files  bottom.txt  bye.move.txt  file1.txt  hi.move.txt  top.txt
~~~
{: .output}

~~~
Be careful not to use a file name that already exists with the `>` operator! Doing so will overwrite that file and replace it with the new content!
~~~
{: .caution}

### Concatenating files

The `>>` operator concatenates data in a file instead of writing a new file. If that file does not exist, it will create one with the name after the `>>` operator. We will combine the two files we just created using `echo` in the last step. We are using `cat` to send the file contents to the command prompt and concatenating that information to a file.

~~~
cat hi.move.txt >> aloha.move.txt
cat bye.move.txt >> aloha.move.txt
cat aloha.move.txt
~~~
{: .language-bash}

~~~
hello
goodbye
~~~
{: .output}

The two lines are now combined in one text file. Consider how `tail`, `head` and `sed` can all be combined to send information to new files or combine lines of interest into one file.

## Wildcards

### Overview of some wildcards

Wildcards provide tremendous power and flexibility when writing commands. Here are a few examples:
- `*` represents any one or many characters. `ls *.txt` will list all of the files ending in the extension `.txt`
- `?` represents any one character
- `[0-9]` represents any one number
- `[a-z]` represents any single lowercase letter
- `[A-Z]` represents any single capital letter
- `[!x]` represents any character except `x`, where the `x` can be substituted with any letter or number
- `{x,y} represents either the character x or y, where either letter can be substituted with any letter or number

### Using wildcards

We can use a wildcard to write all of the files ending in the extension `.txt` to a file. This can be a useful command because this file could be used as future input for a script to move many large files from one directory to another. We will combine the `ls` command with the `>` operator to send a list of files ending in `.txt` to a new file.

~~~
ls *.txt > movelist.txt
cat movelist.txt
~~~
{: .language-bash}

~~~
aloha.move.txt
bash_files
bottom.txt
bye.move.txt
file1.txt
hi.move.txt
top.txt
~~~
{: .output}

We have written several files with the extension `.move.txt`. Let's say we want to move these files, but not all of the files with the `.txt` extension. Wildcards provide a perfect solution to this problem.

~~~
mkdir aloha
ls
mv *.move* aloha/
ls
ls aloha/
~~~
{: .language-bash}

~~~
aloha  aloha.move.txt  bash_files  bottom.txt  bye.move.txt  file1.txt  hi.move.txt  movelist.txt  top.txt
aloha  bash_files  bottom.txt  file1.txt  movelist.txt  top.txt
aloha.move.txt  bye.move.txt  hi.move.txt
~~~
{: .output}

## Sorting and manipulating files

### Sorting



The command `sort` will sort the lines in a file in alpanumeric order. If you follow it with the command `-r`, it will reverse the alphanumeric order. The command will load the contents of the sorted file into the shell. To reduce the data sent to the shell, we will use the `>` operator to create a shorter file.


~~~
head file1.txt -n 8 > file2.txt
ls
~~~
{: .language-bash}

~~~
aloha  bash_files  bottom.txt  file1.txt  file2.txt  movelist.txt  top.txt
~~~
{: .output}

Now we can begin sorting this shortened file:

~~~
sort file2.txt
~~~
{: .language-bash}

~~~
CM008455.1      27261226        C       A       intergenic region
CM008455.1      289184259       AT      A       intron_variant
CM008455.1      32223878        A       AT      intergenic_region
CM008456.1      23376349        C       A       intergenic_region
CM008458.1      158326634       C       T       intergenic_region
CM008458.1      43844223        A       C       intergenic_region
CM008458.1      58282344        A       G       intergenic_region
CM008459.1      8234325823      C       T       intron_variant
~~~
{: .output}

The `-k` argument followed by a number can sort a specific column of data in a file. We will combine this with the `-r` agumennt.

~~~
sort -r -k2 file2.txt
~~~
{: .language-bash}

~~~
CM008459.1      8234325823      C       T       intron_variant
CM008458.1      58282344        A       G       intergenic_region
CM008458.1      43844223        A       C       intergenic_region
CM008455.1      32223878        A       AT      intergenic_region
CM008455.1      289184259       AT      A       intron_variant
CM008455.1      27261226        C       A       intergenic region
CM008456.1      23376349        C       A       intergenic_region
CM008458.1      158326634       C       T       intergenic_region
~~~
{: .output}

The argument `-n` sorts in numerical order instead of alphanumerical order. Imagine you have a list with the numbers 3 and 27.
- In alphanumerical order (the default order for `sort`), the number 27 will precede 3.
- In numerical order using the argument `-n`, the number 3 would precede the number 27.

We will repeat the last command, but use the `-n` numeric order for sorting. Additionally, instead of listing each argument separately, you can combine them together and use the command `-k2nr`.

~~~
sort -k2nr file2.txt
~~~
{: .language-bash}

~~~
CM008459.1      8234325823      C       T       intron_variant
CM008455.1      289184259       AT      A       intron_variant
CM008458.1      158326634       C       T       intergenic_region
CM008458.1      58282344        A       G       intergenic_region
CM008458.1      43844223        A       C       intergenic_region
CM008455.1      32223878        A       AT      intergenic_region
CM008455.1      27261226        C       A       intergenic region
CM008456.1      23376349        C       A       intergenic_region
~~~
{: .output}

### Extracting a column or part of a string from a file

We can extract a specific column from a delimited file using the command `cut` along with the argument `-f` followed by the column number to extract. We will extract the first column of the data.

~~~
cut -f 1 file2.txt
~~~
{: .language-bash}

~~~
CM008455.1
CM008455.1
CM008455.1
CM008456.1
CM008458.1
CM008458.1
CM008458.1
CM008459.1
~~~
{: .output}

The default is for a tab delimited file. If the file is delimited in another way, you can use the `-d` argument followed by the delimiter character in between `""`.

~~~
cut -f 1 -d "." file2.txt
~~~
{: .language-bash}

~~~
CM008455
CM008455
CM008455
CM008456
CM008458
CM008458
CM008458
CM008459
~~~
{: .output}

Notice that with the `.` delimiter, the break occurs after the decimal point in the chromosome names.

You can also extract a part of a string a certain number of characters long using the argument `-c` along with the command `cut`. Specify the number of characters after the `-c` argument. Since only the last two digits differ in the first column, we will extract those to make the column look neater.

~~~
cut -c 7-8 file2.txt
~~~
{: .language-bash}

~~~
55
55
55
56
58
58
58
59
~~~
{: .output}

This produces the same result as using the `.` as a delimiter in the example above.

## Replacing text

The `sed` command will replace text in a file and uses the format `sed 's/old/new/g'`, which would change all instances of the word `old` to `new` in the file. We will change the chromosome name in the first column of the file.

~~~
sed 's/CM0084/Chr_/g' file2.txt > cleanfile.txt
cat cleanfile.txt
~~~
{: .language-bash}

~~~
Chr_55.1        27261226        C       A       intergenic region
Chr_55.1        289184259       AT      A       intron_variant
Chr_55.1        32223878        A       AT      intergenic_region
Chr_56.1        23376349        C       A       intergenic_region
Chr_58.1        43844223        A       C       intergenic_region
Chr_58.1        158326634       C       T       intergenic_region
Chr_58.1        58282344        A       G       intergenic_region
Chr_59.1        8234325823      C       T       intron_variant
~~~
{: .output}

> The `sed` command is very powerful and has a wide range of complex arguments that can be used to completely reformat data. You can find more information about `sed` here: [sed manual](https://www.gnu.org/software/sed/manual/sed.html)
{: .tips}

The `grep` command will find a string within `""` and return lines that match the string.

~~~
grep "intron_variant" file2.txt
~~~
{: .language-bash}

~~~
CM008455.1      289184259       AT      A       intron_variant
CM008459.1      8234325823      C       T       intron_variant
~~~
{: .output}

Rather than output the lines, you can return the number of matches using the argument `-c` preceding the string.

~~~
grep -c "intron_variant" file2.txt
~~~
{: .language-bash}

~~~
2
~~~
{: .output}

> The `grep` command is similar to `sed` in that it has a large number of arguments that make it very powerful and versatile. You can learn more about grep here: [grep manual](https://www.gnu.org/software/grep/manual/grep.html)
{: .tips}

## Piping commands

The `|` operator can be used for piping, or creating a pipeline in which information from one command flows into another command. You can pipe output in a series to as many commands as is needed.

Piping is useful because you don't need to write intermediate information to a file. We will sort the second column of `file1.txt` in numerical order and display the top 3 lines.

~~~
sort -k2 -n file2.txt | head -n 3
~~~
{: .language-bash}

~~~
CM008456.1      23376349        C       A       intergenic_region
CM008455.1      27261226        C       A       intergenic region
CM008455.1      32223878        A       AT      intergenic_region
~~~
{: .output}

## Variables

Variables allow you to assign a piece of information to a different name. You can assign variables by writing the shortcut name followed by an `=` sign, which is followed by the assigned name between `""`. You can use the command `echo` to call a variable that you assigned preceded by the `$` sign. In the example below, we will assign the phrase `APS` to the letter `x`, and use the command `echo` to call the variable `$x`.

~~~
Be sure not to leave any spaces before or after the `=` sign!
~~~
{: .caution}

~~~
x="APS"
echo $x
~~~
{: .language-bash}

~~~
APS
~~~
{: .output}

Variables can be combined to form new variables, and variable names can be longer than a single letter.

~~~
y=" Rocks"
zebra="$x$y"
echo $zebra
~~~

~~~
APS Rocks
~~~
{: .output}

~~~
Be certain to use `""` and not `''` when using variables! The two are not interchangeable!
~~~
{: .caution}

## Loops

Rather than running the same command multiple times, you can use a loop to complete a series of tasks more efficiently. They oftentimes rely upon wildcards and variables to read multiple files or conditions.

> It is good to remember the <kdb>Ctrl</kbd> <kbd>c</kbd> shortcut to terminate a command in the shell in case you accidently write an infinite loop that will never end!
{: .tips}

### The 'For' loop

The 'For' loop iterates a process a number of times defined by a number, a range or an array.

~~~
Codes for loops require multiple lines of code and cannot all be written in the same line, or the commands executed one after another in sequence!
~~~
{: .caution}

To overcome this hurdle, you have a few options:
- Oftentimes code will accompany the `$` and `>` symbols so you cannot paste multiple lines directly into the command shell. Instead, you can paste them into another text editor and remove these symbols.
- Alternatively, you can use the <kbd>ctrl</kbd> to start a new line of code without submitting the first line and paste each line one after the other. Unfortunately, UFRC On Demand does not support this feature.
- As an alterative, we will use the `;` symbol, which will signify a line break without actually writing a second line of code.

We will write out the code twice, first to show how it can be written one line at a time, and second using the the `;` symbol and pasting all of the code in one line, which is recommended for this workshop. However, normally if you are writing a script, it is easier to read and understand the loop if each part is written on a separate line.

~~~
for x in {0..5}
> do
>   echo $x
done
~~~
{: .language-bash}

~~~
for x in {0..3}; do echo $x; done
~~~
{: .language-bash}

~~~
0
1
2
3
~~~
{: .output}

### The `While` loop

Rather than iterating a process a set number of times, the `while` loop will execute a loop until a specific condition is met. In the loop below, we will set the variable x to 0, and then repeatedly perform a loop that adds +1 to that variable until it is equal to or more than 4. You do not need to use numbers as the input. You can use text or complex arguments as the criteria to fulfill a `while` loop.

~~~
x=0; while(($x < 4)); do; echo $x; ((++x)); done
~~~
{: .language-bash}

~~~
0
1
2
3
~~~
{: .output}

That is the end of this lesson, next we will learn how we can have HiPerGator execute complex commands in the background using scripts so that we have more time to do other work.

## Learn more about unix commands
> [Software carpentary reference](https://swcarpentry.github.io/shell-novice/){: target="_blank"}
{: .notes}
