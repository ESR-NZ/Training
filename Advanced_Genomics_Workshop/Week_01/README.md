#### Author: Rhys White ([@RhysTWhite](https://twitter.com/RhysTWhite))
#### Affiliations: Institute of Environmental Science and Research - ESR

# Advanced Genomic Insights Workshop Week 01: an introduction to the command line

Table of contents
=================

<!--ts-->
   * [Prerequisites](#prerequisites)
   * [Introduction](#introduction)
      * [What is a SAM file?](#what-is-a-sam-file)
      * [What is a BAM file?](#what-is-a-bam-file)
   * [Let's get started...](#lets-get-started)
      * [How to view a SAM or BAM file](#how-to-view-a-sam-or-bam-file)
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

To begin using [MobaXterm](http://kscprod-bioman1/wiki/index.php/MobaXterm), follow these steps:
1) Launch MobaXterm
2) After logging in, you will be directed to the Linux bash shell terminal
#0 By default, you will be in your home directory (`/home/username`). This directory is your central hub for file analysis, storage, and installing relevant packages and libraries for your chosen tools.

It's important to note that only you and the Linux administrator have read-write, and execute permissions in your home directory. This means that only you and the administrator can create, modify, and delete files. However, other users have read permission to the files in your home directory. Hence, it is recommended to carry out your actual work in a shared location that is accessible to other members of your project group, rather than within your home directory.
You can access all your programs and files once you're in the command line interface. This allows you to carry out various tasks, such as accessing, copying, moving, and deleting them.
