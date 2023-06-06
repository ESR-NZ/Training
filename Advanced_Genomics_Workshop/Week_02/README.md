#### Author: Rhys White ([@RhysTWhite](https://twitter.com/RhysTWhite))
#### Affiliations: Institute of Environmental Science and Research - ESR

# Advanced Genomic Insights Workshop Week 02: conquering the command line with confidence

Table of contents
=================

<!--ts-->
   * [Prerequisites](#prerequisites)
   * [Introduction](#introduction)
   * [Let's get started...](#lets-get-started)
      * [File permissions](#File-permissions)
      * [Basic text manipulation](#Basic-text-manipulation)
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
Regular expressions, often called regex or regexp, are patterns used for matching and manipulating text. They provide a concise and flexible way to search, extract, and replace specific patterns of characters within a larger text. You can use regular expressions in various programming languages and tools, such as Python, JavaScript, Perl, and many others. They are widely used for tasks like data validation, parsing, search and replace operations, and text manipulation. Regular expressions are supported by various Linux tools, including `grep`, `sed`, `awk`, and others.

However, there are a few important concepts that you should know about before we dive into regular expressions. In this tutorial, we will cover the basics of regular expressions and how to use them in Linux.

## Let's get started...
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

**Remember, it's crucial to assign permissions carefully to maintain the security and integrity of your files. Grant only the necessary permissions to ensure proper access control while safeguarding sensitive data.**

#### Editing a file before and after removing write permission:
To understand the impact of file permissions on editing, we will attempt to edit a file both before and after removing write permission.

**Exercise**:
**Before removing write permission:**
**- Use an editor of your choice to create and open a file**
**- Make changes to the file as desired**
**- Save and exit the editor**

**After removing write permission:**
**- Use the `chmod` command to remove write permission for the appropriate target (user, group, or others)**
**- Attempt to open the file with the editor again**
**- Notice that you cannot save any changes due to the removed write permission**

When working with files from shared folders, other users, or downloading from online repositories, it is usually necessary to use `chmod` to adjust their permissions. You'll frequently need to make a shell script executable before it can be executed on your system.

#### Making a shell script executable:
When working with shell scripts, you might need to make them executable before running them on your system. This can be done using the `chmod` command like this:

```bash
# Add execute permission (+x) to the file script.sh
chmod +x script.sh
```

### Basic text manipulation














