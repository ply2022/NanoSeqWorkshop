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

{% comment %} UF VPN Access {% endcomment %}
<div id="vpn" markdown="1">
### Connecting to Gatorlink VPN
Gatorlink VPN is required to access University if Florida Research Computing OnDemand
service, which will provide access to HiperGator cluster.
1. Visit [UF VPN webpage (vpn.ufl.edu)](https://vpn.ufl.edu){: target="_blank"}.
2. Login using the username and password that was emailed to you.
3. In the next page, download **Cisco Anyconnect** software and install it.
4. Once installed, run **Cisco Anyconnect** and click **Connect**.
5. Enter the username and password provided and click **OK**. 
You should now be connected to Gatorlink VPN.
</div>

{% comment %} Hipergator Access {% endcomment %}
<div id="shell" markdown="1">
### Connecting to HiperGator
Make sure you are connected to Gatorlink VPN before the following steps.
1. In you web browser, navigate to [UFRC OnDemand (ood.rc.ufl.edu)](https://ood.rc.ufl.edu/){: target="_blank"}.
2. Login using provided username and password. You will be redirected to UFRC OnDemand homepage.
3. To connect to remote shell, click on **Clusters** in navbar and click 
**Hipergator Shell Access**. Note that the shell will open in new tab.
4. To view and transfer your files, click on **Files** in navbar 
in UFRC OnDemand homepage and click **/blue/general_workshop**.
Double click on directory named as your username to find your files.
</div>

## Terminal Command Line

### Terminal basics

You can tell that you are logged on because the following should be displayed:

~~~
[<username>@login1 ~]$
~~~
{: .terminal}

The &lt;username&gt; will be your personal username. The `$` symbol indicates that you can input commands. When you submit a command and the `$` symbol disappears, any future commands that you enter will not be processed until after the previous command is complete. We can avoid long lag times before submitting a command or running an application using SLURM scripts, which we will discuss in the next section.

We have removed the `$` from the beginning of each line of code below so it is easier to copy and paste into the shell.

### Recommendations for naming files in a Linux 

- Never include spaces in file or directory names. Instead, use the `_` symbol instead.
- Linux is case sensitive, so it is advisable to avoid capitalization in file and directory names to avoid errors.
- When writing out the path to a file, Linux uses `/` instead of the `\` symbol used by Windows.
- File extensions are not as important for Linux compared to an OS like Windows, and you can use your own arbitrary file extensions to help you sort files.

## Navigating 

### Displaying your current path/location

`pwd` displays your "path", or your file location on HiperGator. This workshop will include code that you can copy directly from this page and paste into the shell. Right clicking is the same as pasting in Linux, so after copying the code, you can right click in the shell to paste it.

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

Now try using `pwd` again to see your current location. Instead of copying from this script or typing it out, you can highlight `pwd` in the shell by double clicking it and then right clicking to paste it into the shell. Long or complicated file and directory names can be time consuming to type out and it is easy to introduce errors. It is much easier to list the name of the files (see below) and copy and paste them in the shell using this method.

~~~
/blue/general_workshop/
~~~
{: .output}

You can quickly go backwards from your current directory using the command `cd ..`

### Listing files in your current directory

You can use the command `ls` to display the file contents in your current folder.

~~~
ls
~~~
{: .language-bash}

~~~
***output***
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
***output***
~~~
{: .output}

You can also combine commands in Linux using the `|` symbol. For example, if you know you will want to change your directory and look up the file contents, you could use this command:

~~~
cd blue/general_workshop | ls
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

## Creating, moving and copying directories and files

### Making a directory

Use `cd` to access the workshop directory with your username.

We will create a new directory using the command `mkdir` followed by the name of the new directory `newdir`. Use `ls` or `cd` to see or navigate to your new directory

~~~
mkdir newdir 
~~~
{: .language-bash}

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

Note that we used the directory shorcut `./` to represent that the new file should be moved to your current directory. You could write out the full name of any other directory to have it moved there, or to put it in the directory you just created, you could change the location to `./newdir/file1.txt`.

You can change the name of the file to anything you want when copying it. For example, we can copy the file again with a new name, but since it is in the current directory, we only need to list the name of the file.

~~~
cp file1.txt file2.txt
ls
~~~
{: .language-bash}



Be careful when copying files that a file does not exist with the same name in that directory, or it will replace it with the new copy.

You can also use the `cp` command with the `-r` option to perform a recursive copy, which can copy entire directories and all of the subdirectories and folders.

~~~
cp -r blue/general_workshop/share/demo ./
~~~
{: .language-bash}

### Moving a file

You can move files with the command `mv`, followed by the file location and the location where you will move it. Moving a file removes it from its original location.

~~~
mv file2.txt newfile.txt
ls
~~~
{: .language-bash}

~~~
***output***
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

### Deleting directories

Aside from using the recursive option for `rm`, you can also remove directories with the command `rmdir`.

~~~
rmdir newdir
ls
~~~
{: .language-bash}

## Reading and writing files

### Reading file contents

There are a number of ways to look at some or all of the contents of a file.

The command `cat` reads the contents of a file. We can use this to view ` file1.txt`.

~~~
cat file1.txt
~~~
{: .language-bash}

~~~
***output***
~~~
{: .output}

Rather than viewing all of the content of a large file, you can use the commands `less` and `more` to view small pieces of the document at a time.

Let's start with `less`. You can use the <kbd>&uparrow;</kbd> and <kbd>&downarrow;</kbd> keys to scroll up and down the text file. You can quit using `less` and return to the command prompt by pressing the <kbd>q</kbd> key.

~~~
less file1.txt
~~~
{: .language-bash}

Now try `more`. You can scroll down the file using the <kbd>enter</kbd> key. Again, use the <kbd>q</kbd> key to return to the command line.

~~~
more file1.txt
~~~
{: .language-bash}

### Previewing the top and bottom of a file

You can preview the top of a file using the command `head`, and you can include the arguement `-n` followed by a number to specify the number of lines to show. The example below will show the first 12 lines of the file.

~~~
head -n 12 file1.txt
~~~
{: .language-bash}

~~~
***output***
~~~
{: .language-bash}

The command `tail` will show the end of the file, and can also be combined with `-n` to show a specific number of lines.

~~~
tail -n 7 file1.txt
~~~
{: .language-bash}

~~~
***output***
~~~
{: .output}

### Viewing the middle of a file

You can view lines from the middle of a file using the command `sed` along with the command `-n` followed by the number of the first and last lines you want to view separated by a `,` and with the letter `p` as a suffix.

~~~
sed -n 7,15p file1.txt
~~~
{: .language-bash}

~~~
***output***
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
***output*** file1.txt
***output*** file1.txt
***output*** file1.txt
~~~



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
***output***
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
***output***
~~~
{: .output}

Be careful not to use a file name that already exists with the `>` operator! Doing so will overwrite that file and replace it with the new content!
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
***output***
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
***output***
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
***output***
***output***
***output***
~~~
{: .output}

## Sorting and manipulating files

### Sorting

The command `sort` will sort the lines in a file in alpanumeric order. If you follow it with the command `-r`, it will reverse the alphanumeric order.

~~~
sort file1.txt
~~~
{: .language-bash}

~~~
***output***
~~~
{: .output}

The `-k` argument followed by a number can sort a specific column of data in a file. We will combine this with the `-r` agumennt.

~~~
sort -r -k2 file1.txt
~~~
{: .language-bash}

~~~
***output***
~~~
{: .output}

The argument `-n` sorts in numerical order instead of alphanumerical order. Imagine you have a list with the numbers 3 and 27.
- In alphanumerical order (the default order for `sort`), the number 27 will precede 3.
- In numerical order using the argument `-n`, the number 3 would precede the number 27.

We will repeat the last command, but use the `-n` numeric order for sorting. Additionally, instead of listing each argument separately, you can combine them together and use the command `-k2nr`.

~~~
sort -k2nr file1.txt
~~~
{: .language-bash}

~~~
***output***
~~~
{: .output}

### Extracting a column or part of a string from a file

We can extract a specific column from a delimited file using the command `cut` along with the argument `-f` followed by the column number to extract. We will extract the first column of the data.

~~~
cut -f 1 file1.txt
~~~
{: .language-bash}

~~~
***output***
~~~
{: .output}

The default is for a tab delimited file. If the file is delimited in another way, you can use the `-d` argument followed by the delimiter character in between `""`.

~~~
cut -f 1 -d "." file1.txt
~~~
{: .language-bash}

~~~
***output***
~~~
{: .output}

Notice that with the `.` delimiter, the break occurs after the decimal point in the chromosome names.

You can also extract a part of a string a certain number of characters long using the argument `-c` along with the command `cut`. Specify the number of characters after the `-c` argument.

~~~
cut -c 1-6 file1.txt
~~~
{: .language-bash}

~~~
***output***
~~~
{: .output}

This produces the same result as using the `.` as a delimiter in the example above.

## Replacing text

The `sed` command will replace text in a file and uses the format `sed 's/old/new/g'`, which would change all instances of the word `old` to `new` in the file.

~~~
sed 's/CM0084/Chr_/g' file1.txt > cleanfile.txt
cat cleanfile.txt
~~~
{: .language-bash}

~~~
***output***
~~~
{: .output}

The `sed` command is very powerful and has a wide range of complex arguments that can be used to completely reformat data. You can find more information about `sed` here: ***site***
{: .tips}

The `grep` command will find a string within `""` and return lines that match the string.

~~~
grep "downstream_gene_variant" file1.txt
~~~
{: .language-bash}

~~~
***output***
~~~
{: .output}

Rather than output the lines, you can return the number of matches using the argument `-c` preceding the string.

~~~
grep -c "downstream_gene_variant" file1.txt
~~~
{: .language-bash}

~~~
***output***
~~~
{: .output}

The `grep` command is similar to `sed` in that it has a large number of arguments that make it very powerful and versatile. You can learn more about grep here: ***site***
{: .tips}

## Piping commands

The `|` operator can be used for piping, or creating a pipeline in which information from one command flows into another command. You can pipe output in a series to as many commands as is needed.

Piping is useful because you don't need to write intermediate information to a file. We will sort the second column of `file1.txt` in numerical order and display the top 3 lines.

~~~
sort -k2 -n file1.txt | head -n 3
~~~
{: .language-bash}

~~~
***output***
~~~
{: .output}

## Variables

Variables allow you to assign a piece of information to a different name. You can assign variables by writing the shortcut name followed by an `=` sign, which is followed by the assigned name between `""`. You can use the command `echo` to call a variable that you assigned preceded by the `$` sign. In the example below, we will assign the phrase `APS` to the letter `x`, and use the command `echo` to call the variable `$x`.

Be sure not to leave any spaces before or after the `=` sign!
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

Be certain to use `""` and not `''` when using variables! The two are not interchangeable!
{: .caution}

## Loops

Rather than running the same command multiple times, you can use a loop to complete a series of tasks more efficiently. They oftentimes rely upon wildcards and variables to read multiple files or conditions.

### The 'For' loop

The 'For' loop iterates a process a number of times defined by a number, a range or an array.

Codes for loops require multiple lines of code and cannot all be written in the same line, or the commands executed one after another in sequence!
{: .caution}

To overcome this hurdle, you have a few options:
- Oftentimes code will accompany the `$` and `>` symbols so you cannot paste multiple lines directly into the command shell. Instead, you can paste them into another text editor and remove these symbols.
- Alternatively, you can use the ***commandkey*** ***workformac?*** to start a new line of code without submitting the first line and paste each line one after the other.
- Another alternative is the use of the `;` symbol, which will signify a line break without actually writing a second line of code.

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

## Learn more about unix commands
> [Software carpentary reference](https://swcarpentry.github.io/shell-novice/reference/)
{: .notes}
