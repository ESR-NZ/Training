#### Author: Rhys White ([@RhysTWhite](https://twitter.com/RhysTWhite))
#### Affiliations: Institute of Environmental Science and Research - ESR

# Advanced Genomic Insights Workshop Week 02:<br> Conquering the command line with confidence

Table of contents
=================

<!--ts-->
   * [Prerequisites](#prerequisites)
   * [Wrapping up Week 01](#wrapping-up-week-01)
      * [File permissions](#file-permissions)
      * [Text manipulation basics](#text-manipulation-basics)
   * [Introduction](#introduction)
   * [Let's get started...](#lets-get-started)
      * [Basic regular expressions](#Basic-regular-expressions)
      * [sed stream editing essentials](#sed-stream-editing-essentials)
   * [Why have I learnt this?](#why-have-i-learnt-this)
   * [Final notes](#final-notes)
   * [Data availability](#data-availability)
<!--te-->

## Prerequisites
To fully benefit from these exercises, it is essential that you manually _type_ each exercise. Please avoid the temptation to copy and paste, as these exercises aim to train your hands, brain, and mind in the fundamental skills of reading, writing, and comprehending code. By copying and pasting, you would be depriving yourself of the valuable learning experience these lessons provide.

**You will need a computer running a command line interface (e.g., macOS Terminal, Windows Command Prompt, Linux terminal). Participants in the course are expected to have a Linux account on the HPC (High-Performance Computing) system and have their secure shell (SSH) keys set up for server connection. It is crucial to complete these steps beforehand. It is not recommended to perform these tasks on the morning of the course. If any software installation is required on laptops, it is advisable to coordinate with Desktop Support at least a week or two before the course.**

If you encounter difficulties during this session, we recommend dedicating the next week to thoroughly exploring the Unix tutorial notes available in the Software Carpentry resources. You can access them here: [https://swcarpentry.github.io/shell-novice/index.html](https://swcarpentry.github.io/shell-novice/index.html)

Please utilise Teams for any questions you may have regarding command-line usage. Remember, there is no such thing as a silly question. Learning a new language, including its vocabulary (commands) and grammar (syntax), takes time, and our team is here to assist you throughout the process. So don't hesitate to ask for help!

## Wrapping up Week 01
There are a few important concepts that you should know about before we dive into regular expressions.<br>
First, ensure that you are currently located within the `week02` directory, which you can create as per the instructions provided in [Week 01: an introduction to the command line](https://github.com/ESR-NZ/Training/tree/main/Advanced_Genomics_Workshop/Week_01).

### File permissions
File permissions are essential for controlling who can read, write, and execute a file. Let's break down the key concepts:

##### Permission types
- Read (r): Allows users to view the contents of a file
- Write (w): Permits users to modify or delete a file
- Execute (x): Grants users the ability to run or execute a file as a program

##### Permission groups
- Owner (u): The user who owns the file. Typically, the creator of the file
- Group (g): The group assigned to the file. Multiple users can be part of the same group
- Other (o): Users who are neither the owner nor in the assigned group
- All (a): All users within the system, regardless of their affiliation

##### Permission modifiers
- Granting (+): Adds the specified permission to the file for the designated group
- Revoking (-): Removes the specified permission from the file for the designated group

By combining these file permissions, we can control precisely who can perform certain actions on a file.

#### Viewing file permissions
To check file permissions, we can use the `ls -alt` command. This command displays a detailed list of files and directories, including their permissions, in reverse chronological order. For example:
```bash
# Detailed list of files and directories, with the most recently modified files appearing at the top
ls -alt
```

#### Modifying file permission
To modify file permissions, you'll use a command like `chmod` command (change mode) in Unix-like systems. This tool is followed by a three-digit code or a symbolic representation, and can allow you to set permissions for each group, granting or revoking access as needed. For example, we either give or revoke all users read and write access using the following commands (symbolic representation):

```bash
# Grant all users full access with read and write execute permissions
chmod a+rw <filename>
```

```bash
# Revoke write permission for all users
chmod a-w <filename>
```

##### Understanding the three-digit representation
The three-digit representation in `chmod` assigns permissions to three different entities: 
- the file owner
- group
- others

Each entity can be assigned read (r), write (w), and execute (x) permissions, which are represented by numeric values. The numeric values associated with these permissions are as follows:
- Read (r): 4
- Write (w): 2
- Execute (x): 1

To assign permissions using the three-digit representation, you will need to construct a three-digit code where each digit represents the permissions for the file owner, group, and others, respectively.

**For example**:
Consider the file `myfile.txt` that you want to modify the permissions for. Let's say you want to assign read, write, and execute permissions to the file owner, read and execute permissions to the group, and only read permission to others. Here's how you can do it using the three-digit representation:

```bash
chmod 754 myfile.txt
```

The first digit (7) represents the permissions for the file owner, which is a combination of read (4), write (2), and execute (1)<br>
The second digit (5) represents the permissions for the group, consisting of read (4) and execute (1)<br>
The third digit (4) represents the permissions for others, which only includes read (4)<br>

The resulting permissions for `myfile.txt` will be:
- The file owner has read, write, and execute permissions
- The group has read and execute permissions
- Others have only read permission

_Remember, it's crucial to assign permissions carefully to maintain the security and integrity of your files. Grant only the necessary permissions to ensure proper access control while safeguarding sensitive data._

#### Editing a file before and after removing write permission:
To understand the impact of file permissions on editing, we will attempt to edit a file both before and after removing write permission.

**Exercise**:<br>
**Before removing write permission:**<br>
**- Use an editor of your choice to create and open a file**<br>
**- Make changes to the file as desired**<br>
**- Save and exit the editor**<br>

**After removing write permission:**<br>
**- Use the `chmod` command to remove write permission for the appropriate target (user, group, or others)**<br>
**- Attempt to open the file with the editor again**<br>
**- Notice that you cannot save any changes due to the removed write permission**<br>

When working with files from shared folders, other users, or downloading from online repositories, it is usually necessary to use `chmod` to adjust their permissions. You'll frequently need to make a shell script executable before it can be executed on your system.

#### Making a shell script executable:
When working with shell scripts, you might need to make them executable before running them on your system. This can be done using the `chmod` command like this:

```bash
# Add execute permission (+x) to the file script.sh
chmod +x script.sh
```

### Text manipulation basics
Next, we will explore three essential commands for text file manipulation: `cut`, `sort`, and `uniq`. These commands are commonly used in Unix-based systems and provide powerful functionalities to extract, sort, and remove duplicates from text files. Additionally, we will briefly discuss file redirection using the `>` and `>>` redirection operators and the pipe `|` operator to connect commands.<br>

Remember, to access a command's manual page, you can use the man command followed by the name of the command you want to learn about. For example:

```bash
# Display the manual page for the ls command, which lists files and directories
man sort
```

The manual page typically describes the command, its usage syntax, available options, and examples. You can navigate the manual page using the `arrow keys` or the `Page Up` and `Page Down` keys. Press `q` to exit the manual page and return to the command prompt.

#### The cut command
The cut command allows you to extract specific sections or fields from a file based on a specified delimiter. Here’s the basic syntax:

```bash
# Basic cut command
cut -f <field(s)> -d <delimiter> <filename>
```
- The `-f` option specifies the field or column number(s) you want to extract
- The `-d` option specifies the delimiter used in the file

#### The sort command
The sort command is used to sort the lines of a text file. It provides options to control the sorting behaviour. Here's the basic syntax:

```bash
# Basic sort command
sort <filename>
```
- By default, the sort command performs a lexicographical sort in ascending order

Options:
- `-n`: Sorts lines in numeric order
- `-r`: Sorts lines in reverse order (descending)
- `-u`: Outputs only the first occurrence of each line (removes duplicates)

#### The uniq command
The `uniq` command reports or omits repeated lines in a file. Here's the basic syntax:

```bash
# Basic uniq command
uniq <filename>
```
- By default, uniq only compares adjacent lines to identify duplicates

Options:<br>
- `-c`: Precedes each line with the number of occurrences<br>
- `-f`: Skips the first "num" fields before checking for duplicates<br>
- `-d`: Only prints duplicate lines<br>
- `-u`: Only prints unique lines regardless of order<br>

#### Activity 1
  
**Using the `echo` command, generate a file called `mytable.txt` consisting of six lines with the following contents:**

```bash
Tauranga Moana,32,1,2
Te Whanganui-a-Tara,25,4,5
Kirikiriroa,18,7,8
Te Whanganui-a-Tara,25,4,5
Ahuriri,12,8,9
Te Whanganui-a-Tara,25,4,5
```

**Step 1: Checking the contents of mytable.txt**<br>
**To begin, we'll use the `less` command to view the contents of `mytable.txt` in the terminal.**<br>

**Step 2: Displaying the second column of `mytable.txt`**<br>
**Next, we'll use the `cut` command to extract and display only the second column of `mytable.txt`.**<br>

**Step 3: Using a pipe**<br>
**Now, let's combine the previous two steps using a pipe (`|`) operator.**<br>
  
**Step 4: Showing only unique lines with `uniq`**<br>
**To show only the unique lines of `mytable.txt`, we can use the `uniq` command.**<br>
  
**Step 5: Showing only unique lines with `sort`**<br>
**Alternatively, we can use the `sort` command to achieve the same result.**<br>
  
**Step 6: Using `sort` and `uniq` in a pipe**<br>
**Finally, let's combine `sort` and `uniq` using a pipe to show only the unique lines of `mytable.txt`.** <br>

## Introduction
Regular expressions, often called regex or regexp, are patterns used for matching and manipulating text. They provide a concise and flexible way to search, extract, and replace specific patterns of characters within a larger text. You can use regular expressions in various programming languages and tools, such as Python, JavaScript, Perl, and many others. They are widely used for tasks like data validation, parsing, search and replace operations, and text manipulation. Regular expressions are supported by various Linux tools, including `grep`, `sed`, `awk`, and others. In this tutorial, we will cover the basics of regular expressions and how to use them in Linux.

## Let's get started...
### Basic concepts
Regular expressions are incredibly flexible and powerful tools for text pattern matching. You can use them in various programming languages, text editors, and command-line tools to perform complex searches and transformations on text data.<br> 
Let's start with some common patterns and their meanings:
  
The dot (`.`) matches any character.<br>
**Example**: The pattern `p.t` matches "pat", "pot", "pit", and so on...

Square brackets `[]` allow you to specify a range of characters.<br>
`[ABC]` matches either "A", "B", or "C"<br>
`[abc]` matches either "a", "b", or "c"<br>
`[A-Z]` matches any capital letter<br>
`[a-z]` matches any lowercase letter<br>
`[A-Za-z]` matches any letter, regardless of case<br>
`[0-9]` matches any digit

The pipe character (`|`) serves as an "OR" operator, allowing you to specify multiple alternatives for matching.<br>
**Example**: The pattern `[cat|dog]` matches either "cat" or "dog".
  
Adding the `^` modifier at the beginning of a pattern negates the match.<br>
**Example**: The pattern `[^0-9]` matches any character that is not a digit.

#### Now let's explore modifiers that allow you to control the number of matches

The question mark (`?`) makes the preceding item optional (matches 0 or 1 times).<br>
**Example**: The pattern `colou?r` matches both "color" and "colour".

The plus sign (`+`) makes the preceding item greedy (matches 1 or more times).<br>
**Example**: The pattern `go+d` matches "god", "good", "gooood", and so on.

The asterisk (`*`) is a combination of the plus and question mark (matches 0 or more times).<br>
**Example**: The pattern `g*d` matches "gd", "god", "good", "gooood", and so on.

#### Curly brackets (`{}`) allow you to specify an exact number of matches

`{n}` matches exactly "n" times.<br>
**Example**: The pattern `go{2}d` matches "good" but not "god" or "gd".

`{n,}` matches "n" or more times.<br>
**Example**: The pattern `go{2,}d` matches "good", "gooood", and so on.

`{n,m}` matches between "n and m" times.<br>
**Example**: The pattern `go{2,4}d` matches "good", "gooood", and "goooood", but not "god" or "gd".

#### Finally, let's discuss anchors to match specific parts of a line

The caret (`^`) matches the beginning of a line.<br>
**Example**: The pattern `^Start` matches lines that start with "Start".

The dollar sign (`$`) matches the end of a line.<br>
**Example**: The pattern `end$` matches lines that end with "end".

**Remember, if you want to interpret a special character literally, you can use the backslash (`\`) to remove its special meaning. For example, `$` matches an actual dollar sign, not the end of a line.**

Take a look at this comprehensive regex [cheatsheet](https://ryanstutorials.net/linuxtutorial/cheatsheetgrep.php), which not only covers the mentioned information but also provides additional details in a user-friendly format. If you're interested in delving deeper, you can also explore other online resources for more examples.  
  
### Basic regular expressions
We will focus on using `egrep` and `sed` commands for exploring how regular expressions work and how they can be used to search for specific patterns in text files. Although it's important to note that regex is widely used in various other commands and programming languages.
  
#### Using regular expressions with `egrep`
The `egrep` command is a powerful tool that enables us to search for patterns within text files using regular expressions. To use `egrep`, run the following command:

```bash
egrep "pattern" <filename>
```  

#### Activity 2
**To complete the required tasks, follow the instructions below:**<br>
 <br>
**(i) Download the sequence data fasta file as specified below**<br>

##### Getting the data
Note: The data is ~23 MB. To download the data, please run these commands:

```bash
# Download the first FASTA file:
wget https://raw.githubusercontent.com/ESR-NZ/Training/main/Advanced_Genomics_Workshop/Week_02/SRR14141431.fasta
```
**(ii) Once downloaded, ensure the fasta file is moved to the designated `week02/` directory**<br>
**(iii) Verify the files present in your `week02/` directory by listing them**<br>
**(iv) Use the `grep` command along with regular expressions to analyse the contents of the `SRR14141431.fasta` file to answer the questions below.**

##### Questions

**(a) Try and construct a command that matches all headers beginning with either "@SRR14141431.1." or "@SRR14141431.2."?**<br>
**Please write a command that uses regular expressions and another command that doesn't rely on regex.**<br>
 <br>
**(b) Which command, using regular expressions, can be used to match all headers containing "length=xxx," where xxx falls within the range of 140 to 145?**<br>




  
  
  
  
  
 

### sed stream editing essentials
The `sed` command, short for stream editor, is another useful tool for manipulating text using regular expressions. Like `grep`, the `sed` command allows us to search for patterns and perform actions such as substitution or deletion. Let's dive into the different usage scenarios and how to handle large outputs effectively.
  
  
#### Output a file with sed
To use sed to process a file directly, you can follow this syntax:
```bash
sed <inputfile>
```
Here, the `sed` command will read the file, perform the specified operations, and output the result to the console (STDOUT).
  
#### Piping text into `sed`
Alternatively, you can use `sed` as the recipient of text via a pipe. Here's an example:
```bash  
cat <inputfile> | sed
```
In this case, the `cat` command is used to read the contents of `<inputfile>`, which are then piped (`|`) to `sed` for processing. Again, the resulting output will be directed to the console (STDOUT).

#### Handling large outputs
When dealing with large outputs, it is often helpful to view only a portion of the output at a time. Here are a few techniques:

##### Using head and tail
To view the beginning of the output, you can pipe it to the head command like this:
```bash  
sed <inputfile> | head
```
  
Similarly, to see the end of the output, you can pipe it to the tail command:
```bash
sed <inputfile> | tail
```
  
##### Using `less` for paging
If the output is too long to fit in your console window, you can use the `less` command to view it page by page. Here's an example:
```bash
sed <inputfile> | less
```

##### Redirecting output to a file
If you want to save the `sed` output to a file, you can use the redirection operator `>` followed by the desired output file name. For example:
```bash
sed <inputfile> > output.txt
```
  
This command will execute `sed`, process the contents of `<inputfile>`, and save the output to a file named output.txt.

##### Handling never-ending text streams
In case you have a never-ending stream of text, it's essential to know how to terminate the `sed` command. You can use the `CTRL-C` shortcut to stop the command execution and return to the command prompt.

  
 Rhys to finish
  
  
```bash
  sed 's;pattern;replacement;g' <filename>
```

Remember that regular expressions offer a wide range of possibilities, allowing for complex pattern matching and manipulation. By mastering them, you'll gain a powerful toolset for working with text in various contexts, including scripting, programming, and data processing. So dive in, experiment, and unlock the full potential of regular expressions!





  ## Why have I learnt this?

You have learned this information to comprehensively understand command-line basics and how to perform everyday tasks using the command line interface. The tutorial guides essential Linux/Unix commands and their effective utilisation. By acquiring these skills, you can effectively use the command line interface for file manipulation, navigation, and text editing tasks. In addition, this knowledge is valuable for working with command line tools, scripting, and bioinformatics analyses.

## Whakamihi! You did it!

That's it for this tutorial on a crash course on the command line! You've comprehensively understood command-line basics and learned various tasks using command-line tools. In addition, by practising exercises and familiarising yourself with Linux/Unix commands, you've honed your skills in navigating directories, managing files, counting lines/words/characters, and more. You've also discovered advanced tips like tab auto-completion, accessing command manuals, and using wildcards for file manipulation. These techniques boost productivity and efficiency.

Remember, mastery of the command line takes practice. If you have any further questions, feel free to ask.

  ###### Final notes

<sup> This tutorial uses example short-read sequencing data from our phylogenomic study of *Chlamydia pecorum* sequence type (ST)23. I invite you to read our publication if you find this data intriguing: <sup>

<sub> &emsp; Jelocnik M, **White RT**, Clune T, O’Connell J, Foxwell J, Hair S, Besier S, Tom L, Phillips N, Robbins A, Bogema D. Molecular characterisation of the Australian and New Zealand livestock *Chlamydia pecorum* strains confirms novel but clonal ST23 in association with ovine foetal loss. *Veterinary Microbiology*. 2023:109774 doi: [https://doi.org/10.1016/j.vetmic.2023.109774](https://doi.org/10.1016/j.vetmic.2023.109774)<sub> 

###### Data availability

<sub> Sequence read data has been submitted to the National Center for Biotechnology Information (NCBI) Sequence Read Archive (SRA) under BioProject [PRJNA719674](https://www.ncbi.nlm.nih.gov/bioproject/?term=PRJNA719674) <sub>

<sub> Raw Illumina sequence read data have been deposited to the NCBI SRA under the accession numbers [SRS8634730 to SRS8634733](https://www.ncbi.nlm.nih.gov/bioproject/?term=PRJNA719674) <sub>

<sub> In addition, the high-quality assembly has been deposited to the NCBI GenBank database under the accession numbers [JAGKIP000000000 to JAGKIQ000000000](https://www.ncbi.nlm.nih.gov/bioproject/?term=PRJNA719674) <sub> 









