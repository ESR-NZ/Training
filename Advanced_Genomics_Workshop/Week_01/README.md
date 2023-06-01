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
      * [Navigating the directory structure](Navigating-the-directory-structure)
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

#### Creating Directories
To create a new directory, you can use the `mkdir` command followed by the name of the directory. By default, the new directory will be created in your current working directory. Here's an example:

```bash
# Make a directory called week01:
mkdir week01
```

To create a directory within another directory, you can specify the path to the parent directory and the name of the new directory. For example:
```bash
# Make a directory within the week01 directory, called seqs:
mkdir week01/seqs
```

#### Listing files in a directory
To list files in a directory, you can use the `ls` command. By default, it will list the files and directories in your current working directory. Here are a couple of useful variations of the ls command:

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
# Change your current working directory to the seqs directory within the week01 directory.
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












Conclusion
In this tutorial, we covered the basics of navigating the directory structure using the command line. You learned how to create new directories, list files in a directory, and change directories. These fundamental commands will help you navigate and organize your files efficiently.









