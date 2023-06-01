#### Author: Rhys White ([@RhysTWhite](https://twitter.com/RhysTWhite))
#### Affiliations: Institute of Environmental Science and Research - ESR

# Advanced Genomic Insights Workshop Week 01: an introduction to the command line

Table of contents
=================

<!--ts-->
   * [Prerequisites](#prerequisites)
   * [Introduction](#introduction)
   * [Let's get started...](#lets-get-started)
      * [Opening the command line](#Opening-the-command-line)
      * [Understanding the prompt](#Understanding-the-prompt)
      * [Navigating the directory structure](#Navigating-the-directory-structure)
      * [File operations](#File-operations)
      * [File viewing and text editors](#File-viewing-and-text-editors)
   * [Why have I learnt this?](#why-have-i-learnt-this)
   * [Final notes](#final-notes)
   * [Data availability](#data-availability)
<!--te-->

## Prerequisites
To fully benefit from these exercises, it is essential that you manually _type_ each exercise. Please avoid the temptation to copy and paste, as these exercises aim to train your hands, brain, and mind in the fundamental skills of reading, writing, and comprehending code. By copying and pasting, you would be depriving yourself of the valuable learning experience these lessons provide.

**You will need a computer running a command line interface (e.g., macOS Terminal, Windows Command Prompt, Linux terminal). Participants in the course are expected to have a Linux account on the HPC (High-Performance Computing) system and have their secure shell (SSH) keys set up for server connection. It is crucial to complete these steps beforehand. It is not recommended to perform these tasks on the morning of the course. If any software installation is required on laptops, it is advisable to coordinate with Desktop Support at least a week or two before the course.**

If you encounter difficulties during this session, we recommend dedicating the next week to thoroughly exploring the Unix tutorial notes available in the Software Carpentry resources. You can access them here: [https://swcarpentry.github.io/shell-novice/index.html](https://swcarpentry.github.io/shell-novice/index.html)

Please utilise Teams for any questions you may have regarding command-line usage. Remember, there is no such thing as a silly question. Learning a new language, including its vocabulary (commands) and grammar (syntax), takes time, and our team is here to assist you throughout the process. So don't hesitate to ask for help!

## Introduction
The command line, also referred to as the terminal or shell, is a robust tool for interacting with your computer through text-based commands. This tutorial is designed to provide a comprehensive understanding of command line basics and assist you in performing everyday tasks.
In this practical session, we aim to acquaint you with essential Linux/Unix commands and offer guidance on effectively utilising command line tools.

## Let's get started...
By now, you should have become acquainted with the Basic Server Usage wiki page, accessible at the following link: [https://kscprod-bioman1.esr.cri.nz/wiki/index.php/Basic_Server_Usage_FAQ](https://kscprod-bioman1.esr.cri.nz/wiki/index.php/Basic_Server_Usage_FAQ)

### Opening the command line
To begin using [MobaXterm](http://kscprod-bioman1/wiki/index.php/MobaXterm), follow these steps:
1) Launch MobaXterm
2) After logging in, you will be directed to the Linux bash shell terminal
3) By default, you will be in your home directory (`/home/username`). This directory is your central hub for file analysis, storage, and installing relevant packages and libraries for your chosen tools. Here, you will conduct training for Unix commands, practical exercises, and bioinformatics analyses. Keeping your home directory tidy is essential for maintaining organisation. One way to achieve this is by organising your work into folders, such as `week01` and so on. This practice will help you quickly locate and manage your files as you progress through your work.

It's important to note that only you and the Linux administrator have read-write, and execute permissions in your home directory. This means that only you and the administrator can create, modify, and delete files. However, other users have read permission to the files in your home directory. Hence, it is recommended to carry out your actual work in a shared location that is accessible to other members of your project group, rather than within your home directory.
You can access all your programs and files once you're in the command line interface. This allows you to carry out various tasks, such as accessing, copying, moving, and deleting them.

### Understanding the prompt
Once the command line is open, you'll see a prompt indicating it's ready to receive commands. The prompt usually consists of text followed by a cursor awaiting your input. For example, on macOS and Linux, the prompt might be a dollar sign ($).

### Navigating the directory structure
We will learn how to navigate the directory structure using basic command-line tools. We will cover creating new directories, listing files in a directory, and changing directories.

#### Creating directories
To create a new directory, you can use the `mkdir` command followed by the name of the directory. By default, the new directory will be created in your current working directory. Here's an example:

```bash
# Make a directory called week01
mkdir week01
```

To create a directory within another directory, you can specify the path to the parent directory and the name of the new directory. For example:
```bash
# Make a directory within the week01 directory, called seqs
mkdir week01/seqs
```

#### Listing files in a directory
To list files in a directory, you can use the `ls` command. By default, it will list the files and directories in your current working directory. Here are a couple of useful variations of the `ls` command:

```bash
# List all the files and directories in the current working directory
ls
```

```bash
# List all the files and directories in the current working directory with detailed information, such as permissions, owner, size, and modification date
ls -l
```

#### Changing Directories
To change to a different directory, you can use the `cd` command followed by the path to the directory you want to navigate to. Here are a couple of examples:

```bash
# Change your current working directory to the week01 directory
cd week01
```

```bash
# Move up one level in the directory tree, from the current directory to its parent directory
cd ..
```

```bash
# Change your current working directory to the seqs directory within the week01 directory
cd week01/seqs
```

```bash
# Move directly to your home directory
cd
```

Remember that the path to a directory is relative to your current working directory. If you specify a relative path, it will be resolved starting from your current location.

#### Checking the current working directory
To determine the current working directory, you can use the `pwd` command. It stands for "print working directory". When you run this command, it will display the full path of your current directory. Here's an example:

```bash
# Print the absolute path of the directory you are currently in
pwd
```

#### Accessing command manuals
Command manuals, also known as "man pages", provide detailed information about various commands. To access a command's manual page, you can use the `man` command followed by the name of the command you want to learn about. For example:

```bash
# Display the manual page for the ls command, which lists files and directories
man ls
```

The manual page typically includes a description of the command, its usage syntax, available options, and examples. You can navigate through the manual page using the arrow keys or the `Page Up` and `Page Down` keys. Press `q` to exit the manual page and return to the command prompt.

#### Activity 1
**If you haven't already, create a new directory called `week01`. Once the directory is created, enter into it and confirm that the change of directories was successful by checking the current working directory. Next, create a new subdirectory named `seqs`. After creating `seqs`, navigate back to your home directory using both absolute and relative paths.**


##### Questions

**(a) What is the absolute path to your home directory?**<br>
**(b) Why is it not recommended to use spaces in filenames?**<br>
**(c) What `ls` flag can be used to classify items as directories, executable files, or links by appending `/`, `*` or `@` characters?**<br>

#### Advanced tips and tricks
- Use the `Tab` key for auto-completion. Start typing a command, file, or directory name, and then press `Tab` to have the command line complete it for you or show you possible options. Press the `up` and `down` arrow keys to navigate through your command history.
- Use the `man` command followed by a command name to display the manual page for that command. The manual page provides detailed information and usage examples.
- Learn about wildcards (e.g., `*` and `?`) to perform advanced file matching and manipulation.

### File operations
We will explore various file operations that can be performed using command-line tools. These operations include moving files, copying files, concatenating files, removing directories and their contents, counting newlines/words/characters in a file, and viewing files without editing capabilities.

#### Move files (including renaming)
To move files or folders from one location to another, use the `mv` command followed by the source path and filename, and then the destination. For example:
```bash
mv <source path and filename> <destination>
```

To move a file or folder to the current directory, use the `.` (dot) as the destination. For example:

```bash
# Move a file to the current directory
mv <source path and filename> .
```

#### Copy files
To copy a file from one location to another, use the `cp` command followed by the source and destination. For example:

```bash
cp <source path and filename> <destination>
```

#### Concatenate files
To concatenate files and print the output to the standard output (`STDOUT`), use the `cat` command followed by the filenames. For example:

```bash
cat file1 file2 file3
```

To concatenate files and save the output as a new file, use the redirection operator `>` followed by the output filename. For example:

```bash
# Concatenate files and save the output as a new file
cat file1 file2 file3 > outputfile
```

#### Remove a directory and its contents
Be cautious while using the following command, as it deletes a directory and all its contents without a second chance.

```bash
rm -r <directory>
```

#### Count newlines, words, or characters in a file
To count the number of newline characters, words, and characters in a file, use the `wc` command followed by the filename.

```bash
wc <filename>
```

#### View files without editing capability
To view a file without editing capabilities, use the `less` command followed by the filename. This presents a read-only version of the file.

```php
less <filename>
```

While viewing the file, you can navigate to the bottom using `G`, navigate to the top using `1G`, search using `/`, and exit using `q`. Additionally, you can use the `spacebar` to page down and `b` to page up.

####  Show first and last lines of a file
To display the first and last lines of a file, use the `head` and `tail` commands, respectively. You can specify the number of lines to display using the `-n` flag.

``` php
head [-n <number_of_lines>] <filename>
tail [-n <number_of_lines>] <filename>
```

#### Activity 2
**To complete the required tasks, follow the instructions below:**<br>
**(i) Download two sequence data files as specified below**<br>

##### Getting the data
To download the data, please run these commands:

```bash
# Download the first FASTA file:
wget https://raw.githubusercontent.com/ESR-NZ/Training/main/Advanced_Genomics_Workshop/Week_01/C.pecorum_MLST.fasta
```

```bash
# Download the second FASTA file:
wget https://raw.githubusercontent.com/ESR-NZ/Training/main/Advanced_Genomics_Workshop/Week_01/SRR14141431.tar.gz
```

**(ii) Once downloaded, ensure both files are copied to the designated `seqs/` directory**<br>
**(iii) Verify the files present in your `seqs/` directory by listing them**<br>
**(iv) Create a new file named `TestFile.fasta` by concatenating two identical copies of the `C.pecorum_MLST.fasta` file**<br>
**(v) To determine the number of sequences in `TestFile.fasta`, utilise the `wc` and less commands**<br>

##### Questions

**(d) What is the size of the `SRR14141431.tar.gz` file in kilobytes?**<br>
**(e) How many sequences does `TestFile.fasta` contain? (Hint: Be mindful of the newline count)**<br>

### File viewing and text editors
Various commands and techniques exist for file viewing, text editing, searching within files, and compressing/archiving files using the command line interface. These commands are commonly used in Unix-based systems. They can enhance your productivity when working with files and directories. 

#### File viewing and text editors
##### Text editors
Use the Vim editor to open a file for editing or create a new file. For excample:

```bash
# Open a file for editing or create a new file
vim <filename>
```

When in the Vim text editor, press the `i` key to enter the insert mode and start editing the file. To save and exit Vim, press the `Escape` key to enter command mode, then type `:wq` and hit `Enter`.

Nano is a slightly easier-to-use text editor than Vim. It's recommended for beginners:

```bash
# Open a file for editing or create a new file
nano <filename>
```

When in the nano text editor, make your changes, and then press `Ctrl` `+` `O` to save the file and `Ctrl` `+` `X` to exit nano.

#### Searching in and modifying files
Use grep to search for lines in a file that match a specific pattern:

```bash
grep <search-pattern> <filename>
```

This command will display all the lines containing the pattern on the standard output (`STDOUT`). Replace `<search-pattern>` with the actual pattern you want to search for.

```bash
# finds all the lines in a file that do not contain the specified pattern
grep -v <search-pattern> <filename>
```

```bash
# Count the number of lines in a file that contain a specific pattern
grep -c <search-pattern> <filename>
```

```bash
# Find lines in a file that contain any pattern from a list of patterns specified in the 'list' file
grep -f list <filename>
```

Use  `sed` to perform substitutions within a file:

```bash
sed 's;patterntoreplace;replacement;g' <filename> > <outputfile>
```

This command replaces the first occurrence of 'patterntoreplace' with 'replacement' in the file and saves the output to `outputfile`. You can modify the pattern and replacement strings to suit your needs.

#### Compression and archiving
Compress a file using `gzip`, which produces a new file with the `.gz` extension. The original file will be replaced with the compressed version:
```bash
# Compress a file with gzip
gzip <filename>
```

Uncompress a file that has been compressed using gzip. This command will restore the original file from its compressed version: 
```bash
# Uncompress a file with gzip
gunzip <filename>
```

Create a tar archive named `archive.tar.gz` containing 'filename': 
```bash
# Compress an entire directory or a single file
tar -czvf archive.tar <filename>
```

Here’s what those switches actually mean:

-c: Create an archive<br>
-z: Compress the archive with gzip<br>
-v: Display progress in the terminal while creating the archive, also known as “verbose” mode. The v is always optional in these commands, but it’s helpful<br>
-f: Allows you to specify the filename of the archive<br>

Extract files from a tar archive named 'archive.tar.gz'
```bash
# Extract an archive
tar -xzvf archive.tar.gz
```

#### Activity 3
To uncompress the file `SRR14141431.tar.gz`, employ the `grep` command to extract all the lines containing fasta headers and store them in a text file. Then, utilise `sed` to prepend your name to the beginning of each line and save the modified output to a different file. Finally, open the file in a text editor or utilize commands like `head`, `tail`, or `less` to confirm that your changes were successfully applied.


##### Cheat sheet:
You can find a useful Unix command-line cheat sheet by following this step. Go through the list and mark off the commands you have utilised today. If you have completed all of today's activities, take this opportunity to explore additional commands from the cheat sheet.

To download the cheat sheet, please run this command:

```bash
# Download the cheat sheet file:
wget https://raw.githubusercontent.com/ESR-NZ/Training/main/Advanced_Genomics_Workshop/Week_01/Unix_Linux_CheatSheet.pdf
```

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





