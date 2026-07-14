# Bioinformatics Data Skills

Vince Bufalo 

## Bioinformatics Data Skills

by Vince Buffalo 

Copyright © 2015 Vince Buffalo. All rights reserved. 

Printed in the United States of America. 

Published by O’Reilly Media, Inc., 1005 Gravenstein Highway North, Sebastopol, CA 95472. 

O’Reilly books may be purchased for educational, business, or sales promotional use. Online editions are also available for most titles (http://safaribooksonline.com). For more information, contact our corporate/ institutional sales department: 800-998-9938 or corporate@oreilly.com. 

Editors: Courtney Nash and Amy Jollymore Production Editor: Nicole Shelby Copyeditor: Jasmine Kwityn Proofreader: Kim Cofer 

Indexer: Ellen Troutman 

Interior Designer: David Futato 

Cover Designer: Ellie Volckhausen 

Illustrator: Rebecca Demarest 

June 2015: First Edition 

## Revision History for the First Edition

2015-06-30: First Release 

See http://oreilly.com/catalog/errata.csp?isbn=9781449367374 for release details. 

The O’Reilly logo is a registered trademark of O’Reilly Media, Inc. Bioinformatics Data Skills, the cover image, and related trade dress are trademarks of O’Reilly Media, Inc. 

While the publisher and the author have used good faith efforts to ensure that the information and instructions contained in this work are accurate, the publisher and the author disclaim all responsibility for errors or omissions, including without limitation responsibility for damages resulting from the use of or reliance on this work. Use of the information and instructions contained in this work is at your own risk. If any code samples or other technology this work contains or describes is subject to open source licenses or the intellectual property rights of others, it is your responsibility to ensure that your use thereof complies with such licenses and/or rights 

978-1-449-36737-4 

To my (rather large) family for their continued support: Mom, Dad, Anne, Lisa, Lauren, Violet, and Dalilah; the Bufalos, the Kihns, and the Lambs. 

And my earliest mentors for inspiring me to be who I am today: Randy Siverson and Duncan Temple Lang. 

## Table of Contents

Preface....xiii 

Part I. Ideology: Data Skills for Robust and Reproducible Bioinformatics
1. How to Learn Bioinformatics.... 1
Why Bioinformatics? Biology's Growing Data 1
Learning Data Skills to Learn Bioinformatics 4
New Challenges for Reproducible and Robust Research 5
Reproducible Research 6
Robust Research and the Golden Rule of Bioinformatics 8
Adopting Robust and Reproducible Practices Will Make Your Life Easier, Too 9
Recommendations for Robust Research 10
Pay Attention to Experimental Design 10
Write Code for Humans, Write Data for Computers 11
Let Your Computer Do the Work For You 12
Make Assertions and Be Loud, in Code and in Your Methods 12
Test Code, or Better Yet, Let Code Test Code 13
Use Existing Libraries Whenever Possible 14
Treat Data as Read-Only 14
Spend Time Developing Frequently Used Scripts into Tools 15
Let Data Prove That It's High Quality 15
Recommendations for Reproducible Research 16
Release Your Code and Data 16
Document Everything 16
Make Figures and Statistics the Results of Scripts 17
Use Code as Documentation 17
Continually Improving Your Bioinformatics Data Skills 17 

## Part II. Prerequisites: Essential Skills for Getting Started with a Bioinformatics Project

2. Setting Up and Managing a Bioinformatics Project.... 21
Project Directories and Directory Structures 21
Project Documentation 24
Use Directories to Divide Up Your Project into Subprojects 26
Organizing Data to Automate File Processing Tasks 26
Markdown for Project Notebooks 31
Markdown Formatting Basics 31
Using Pandoc to Render Markdown to HTML 35

3. Remedial Unix Shell.... 37
Why Do We Use Unix in Bioinformatics? Modularity and the Unix Philosophy 37
Working with Streams and Redirection 41
Redirecting Standard Out to a File 41
Redirecting Standard Error 43
Using Standard Input Redirection 45
The Almighty Unix Pipe: Speed and Beauty in One 45
Pipes in Action: Creating Simple Programs with Grep and Pipes 47
Combining Pipes and Redirection 48
Even More Redirection: A tee in Your Pipe 49
Managing and Interacting with Processes 50
Background Processes 50
Killing Processes 51
Exit Status: How to Programmatically Tell Whether Your Command Worked 52
Command Substitution 54

4. Working with Remote Machines.... 57
Connecting to Remote Machines with SSH 57
Quick Authentication with SSH Keys 59
Maintaining Long-Running Jobs with nohup and tmux nohup 61
Working with Remote Machines Through Tmux 61
Installing and Configuring Tmux 62
Creating, Detaching, and Attaching Tmux Sessions 62
Working with Tmux Windows 64 

5. Git for Scientists....67
Why Git Is Necessary in Bioinformatics Projects 68
Git Allows You to Keep Snapshots of Your Project 68
Git Helps You Keep Track of Important Changes to Code 69
Git Helps Keep Software Organized and Available After People Leave 69
Installing Git 70
Basic Git: Creating Repositories, Tracking Files, and Staging and Committing Changes 70
Git Setup: Telling Git Who You Are 70
git init and git clone: Creating Repositories 70
Tracking Files in Git: git add and git status Part I 72
Staging Files in Git: git add and git status Part II 73
git commit: Taking a Snapshot of Your Project 76
Seeing File Differences: git diff 77
Seeing Your Commit History: git log 79
Moving and Removing Files: git mv and git rm 80
Telling Git What to Ignore: .gitignore 81
Undoing a Stage: git reset 83
Collaborating with Git: Git Remotes, git push, and git pull 83
Creating a Shared Central Repository with GitHub 86
Authenticating with Git Remotes 87
Connecting with Git Remotes: git remote 87
Pushing Commits to a Remote Repository with git push 88
Pulling Commits from a Remote Repository with git pull 89
Working with Your Collaborators: Pushing and Pulling 90
Merge Conflicts 92
More GitHub Workflows: Forking and Pull Requests 97
Using Git to Make Life Easier: Working with Past Commits 97
Getting Files from the Past: git checkout 97
Stashing Your Changes: git stash 99
More git diff: Comparing Commits and Files 100
Undoing and Editing Commits: git commit --amend 102
Working with Branches 102
Creating and Working with Branches: git branch and git checkout 103
Merging Branches: git merge 105
Branches and Remotes 106
Continuing Your Git Education 108

6. Bioinformatics Data....109
Retrieving Bioinformatics Data 110
Downloading Data with wget and curl 110
Rsync and Secure Copy (scp) 113 

Data Integrity 114
SHA and MD5 Checksums 115
Looking at Differences Between Data 116
Compressing Data and Working with Compressed Data 118
gzip 119
Working with Gzipped Compressed Files 120
Case Study: Reproducibly Downloading Data 120

Part III. Practice: Bioinformatics Data Skills

7. Unix Data Tools.... 125
Unix Data Tools and the Unix One-Liner Approach: Lessons from Programming Pearls 125
When to Use the Unix Pipeline Approach and How to Use It Safely 127
Inspecting and Manipulating Text Data with Unix Tools 128
Inspecting Data with Head and Tail 129
less 131
Plain-Text Data Summary Information with wc, ls, and awk 134
Working with Column Data with cut and Columns 138
Formatting Tabular Data with column 139
The All-Powerful Grep 140
Decoding Plain-Text Data: hexdump 145
Sorting Plain-Text Data with Sort 147
Finding Unique Values in Uniq 152
Join 155
Text Processing with Awk 157
Bioawk: An Awk for Biological Formats 163
Stream Editing with Sed 165
Advanced Shell Tricks 169
Subshells 169
Named Pipes and Process Substitution 171
The Unix Philosophy Revisited 173

8. A Rapid Introduction to the R Language.... 175
Getting Started with R and RStudio 176
R Language Basics 178
Simple Calculations in R, Calling Functions, and Getting Help in R 178
Variables and Assignment 182
Vectors, Vectorization, and Indexing 183
Working with and Visualizing Data in R 193
Loading Data into R 194 

Exploring and Transforming Dataframes 199  
Exploring Data Through Slicing and Dicing: Subsetting Dataframes 203  
Exploring Data Visually with ggplot2 I: Scatterplots and Densities 207  
Exploring Data Visually with ggplot2 II: Smoothing 213  
Binning Data with cut() and Bar Plots with ggplot2 215  
Merging and Combining Data: Matching Vectors and Merging Dataframes 219  
Using ggplot2 Facets 224  
More R Data Structures: Lists 228  
Writing and Applying Functions to Lists with lapply() and sapply() 231  
Working with the Split-Apply-Combine Pattern 239  
Exploring Dataframes with dplyr 243  
Working with Strings 248  
Developing Workflows with R Scripts 253  
Control Flow: if, for, and while 253  
Working with R Scripts 254  
Workflows for Loading and Combining Multiple Files 257  
Exporting Data 260  
Further R Directions and Resources 261  

D. Working with Range Data.... 263  
A Crash Course in Genomic Ranges and Coordinate Systems 264  
An Interactive Introduction to Range Data with GenomicRanges 269  
Installing and Working with Bioconductor Packages 269  
Storing Generic Ranges with IRanges 270  
Basic Range Operations: Arithmetic, Transformations, and Set Operations 275  
Finding Overlapping Ranges 281  
Finding Nearest Ranges and Calculating Distance 290  
Run Length Encoding and Views 292  
Storing Genomic Ranges with GenomicRanges 299  
Grouping Data with GRangesList 303  
Working with Annotation Data: GenomicFeatures and rtracklayer 308  
Retrieving Promoter Regions: Flank and Promoters 314  
Retrieving Promoter Sequence: Connection GenomicRanges with Sequence Data 315  
Getting Intergenic and Intronic Regions: Gaps, Reduce, and Setdiffs in Practice 319  
Finding and Working with Overlapping Ranges 324  
Calculating Coverage of GRanges Objects 327  
Working with Ranges Data on the Command Line with BEDTools 329  
Computing Overlaps with BEDTools Intersect 330  
BEDTools Slop and Flank 333  
Coverage with BEDTools 335 

Other BEDTools Subcommands and pybedtools 336   
10. Working with Sequence Data. 339   
The FASTA Format 339   
The FASTQ Format 341   
Nucleotide Codes 343   
Base Qualities 344   
Example: Inspecting and Trimming Low-Quality Bases 346   
A FASTA/FASTQ Parsing Example: Counting Nucleotides 349   
Indexed FASTA Files 352   
11. Working with Alignment Data. 355   
Getting to Know Alignment Formats: SAM and BAM 356   
The SAM Header 356   
The SAM Alignment Section 359   
Bitwise Flags 360   
CIGAR Strings 363   
Mapping Qualities 365   
Command-Line Tools for Working with Alignments in the SAM Format 365   
Using samtools view to Convert between SAM and BAM 365   
Samtools Sort and Index 367   
Extracting and Filtering Alignments with samtools view 368   
Visualizing Alignments with samtools tview and the Integrated Genomics Viewer 372   
Pileups with samtools pileup, Variant Calling, and Base Alignment Quality 378   
Creating Your Own SAM/BAM Processing Tools with Pysam 384   
Opening BAM Files, Fetching Alignments from a Region, and Iterating Across Reads 384   
Extracting SAM/BAM Header Information from an AlignmentFile Object 387   
Working with AlignedSegment Objects 388   
Writing a Program to Record Alignment Statistics 391   
Additional Pysam Features and Other SAM/BAM APIs 394   
12. Bioinformatics Shell Scripting, Writing Pipelines, and Parallelizing Tasks. 395   
Basic Bash Scripting 396   
Writing and Running Robust Bash Scripts 396   
Variables and Command Arguments 398   
Conditionals in a Bash Script: if Statements 401   
Processing Files with Bash Using for Loops and Globbing 405   
Automating File-Processing with find and xargs 411   
Using find and xargs 411   
Finding Files with find 412 

find's Expressions 413
find's -exec: Running Commands on find's Results 415
xargs: A Unix Powertool 416
Using xargs with Replacement Strings to Apply Commands to Files 418
xargs and Parallelization 419
Make and Makefiles: Another Option for Pipelines 421

13. Out-of-Memory Approaches: Tabix and SQLite.... 425
Fast Access to Indexed Tab-Delimited Files with BGZF and Tabix 425
Compressing Files for Tabix with Bgzip 426
Indexing Files with Tabix 427
Using Tabix 427
Introducing Relational Databases Through SQLite 428
When to Use Relational Databases in Bioinformatics 429
Installing SQLite 431
Exploring SQLite Databases with the Command-Line Interface 431
Querying Out Data: The Almighty SELECT Command 434
SQLite Functions 441
SQLite Aggregate Functions 442
Subqueries 447
Organizing Relational Databases and Joins 448
Writing to Databases 455
Dropping Tables and Deleting Databases 458
Interacting with SQLite from Python 459
Dumping Databases 465

14. Conclusion.... 467
Where to Go From Here? 468

Glossary.... 471

Bibliography.... 479

Index.... 483 

## Preface

This book is the answer to a question I asked myself two years ago: “What book would I want to read frst when getting started in bioinformatics?” When I began working in this field, I had programming experience in Python and R but little else. I had hunted around for a terrific introductory text on bioinformatics, and while I found some good books, most were not targeted to the daily work I did as a bioinfor‐ matician. A few of the texts I found approached bioinformatics from a theoretical and algorithmic perspective, covering topics like Smith-Waterman alignment, phylogeny reconstruction, motif finding, and the like. Although they were fascinating to read (and I do recommend that you explore this material), I had no need to implement bioinformatics algorithms from scratch in my daily bioinformatics work—numerous terrific, highly optimized, well-tested implementations of these algorithms already existed. Other bioinformatics texts took a more practical approach, guiding readers unfamiliar with computing through each step of tasks like running an aligner or downloading sequences from a database. While these were more applicable to my work, much of those books’ material was outdated. 

As you might guess, I couldn’t find that best “first” bioinformatics book. Bioinformat‐ ics Data Skills is my version of the book I was seeking. This book is targeted toward readers who are unsure how to bridge the giant gap between knowing a scripting lan‐ guage and practicing bioinformatics to answer scientific questions in a robust and reproducible way. To bridge this gap, one must learn data skills—an approach that uses a core set of tools to manipulate and explore any data you’ll encounter during a bioinformatics project. 

Data skills are the best way to learn bioinformatics because these skills utilize timetested, open source tools that continue to be the best way to manipulate and explore changing data. This approach has stood the test of time: the advent of highthroughput sequencing rapidly changed the field of bioinformatics, yet skilled bioin formaticians adapted to this new data using these same tools and skills. Nextgeneration data was, after all, just data (different data, and more of it), and master bioinformaticians had the essential skills to solve problems by applying their tools to this new data. Bioinformatics Data Skills is written to provide you with training in these core tools and help you develop these same skills. 

## The Approach of This Book

Many biologists starting out in bioinformatics tend to equate “learning bioinformat‐ ics” with “learning how to run bioinformatics software.” This is an unfortunate and misinformed idea of what bioinformaticians actually do. This is analogous to think‐ ing “learning molecular biology” is just “learning pipetting.” Other than a few simple examples used to generate data in Chapter 11, this book doesn’t cover running bioin‐ formatics software like aligners, assemblers, or variant callers. Running bioinformat‐ ics software isn’t all that difficult, doesn’t take much skill, and it doesn’t embody any of the significant challenges of bioinformatics. I don’t teach how to run these types of bioinformatics applications in Bioinformatics Data Skills for the following reasons: 

• It’s easy enough to figure out on your own 

• The material would go rapidly out of date as new versions of software or entirely new programs are used in bioinformatics 

• The original manuals for this software will always be the best, most up-to-date resource on how to run a program 

Instead, the approach of this book is to focus on the skills bioinformaticians use to explore and extract meaning from complex, large bioinformatics datasets. Exploring and extracting information from these datasets is the fun part of bioinformatics research. The goal of Bioinformatics Data Skills is to teach you the computational tools and data skills you need to explore these large datasets as you please. These data skills give you freedom; you’ll be able to look at any bioinformatics data—in any for‐ mat, and files of any size—and begin exploring data to extract biological meaning. 

Throughout Bioinformatics Data Skills, I emphasize working in a robust and reprodu‐ cible manner. I believe these two qualities—reproducibility and robustness—are too often overlooked in modern computational work. By robust, I mean that your work is resilient against silent errors, confounders, software bugs, and messy or noisy data. In contrast, a fragile approach is one that does not decrease the odds of some type of error adversely affecting your results. By reproducible, I mean that your work can be repeated by other researchers and they can arrive at the same results. For this to be the case, your work must be well documented, and your methods, code, and data all need to be available so that other researchers have the materials to reproduce every‐ thing. Reproducibility also relies on your work being robust—if a workflow run on a different machine yields a different outcome, it is neither robust nor fully reproduci‐ ble. I introduce these concepts in more depth in Chapter 2, and these are themes that reappear throughout the book. 

## Why This Book Focuses on Sequencing Data

Bioinformatics is a broad discipline, and spans subfields like proteomics, metabolo‐ mics, structure bioinformatics, comparative genomics, machine learning, and image processing. Bioinformatics Data Skills focuses primarily on handling sequencing data for a few reasons. 

First, sequencing data is abundant. Currently, no other “omics” data is as abundant as high-throughput sequencing data. Sequencing data has broad applications across biology: variant detection and genotyping, transcriptome sequencing for gene expres‐ sion studies, protein-DNA interaction assays like ChIP-seq, and bisulfite sequencing for methylation studies just to name a few examples. The ways in which sequencing data can be used to answer biological questions will only continue to increase. 

Second, sequencing data is terrific for honing your data skills. Even if your goal is to analyze other types of data in the future, sequencing data serves as great example data to learn with. Developing the text-processing skills necessary to work with sequenc‐ ing data will be applicable to working with many other data types. 

Third, other subfields of bioinformatics are much more domain specific. The wide availability and declining costs of sequencing have allowed scientists from all disci‐ plines to use genomics data to answer questions in their systems. In contrast, bioin‐ formatics subdisciplines like proteomics or high-throughput image processing are much more specialized and less widespread. Still, if you’re interested in these fields, Bioinformatics Data Skills will teach you useful computational and data skills that will be helpful in your research. 

## Audience

In my experience teaching bioinformatics to friends, colleagues, and students of an intensive week-long course taught at UC Davis, most people wishing to learn bioin‐ formatics are either biologists, or computer scientists/programmers. Biologists wish to develop the computational skills necessary to analyze their own data, while the programmers and computer scientists wish to apply their computational skills to bio‐ logical problems. Although these two groups differ considerably in biological knowl‐ edge and computational experience, Bioinformatics Data Skills covers material that should be helpful to both. 

If you’re a biologist, Bioinformatics Data Skills will teach you the core data skills you need to work with bioinformatics data. It’s important to note that Bioinformatics Data Skills is not a how-to bioinformatics book; such a book on bioinformatics would quickly go out of date or be too narrow in focus to help the majority of biologists. You will need to supplement this book with knowledge of your specific research and sys‐ tem, as well as the modern statistical and bioinformatics methods that your subfield uses. For example, if your project involves aligning sequencing reads to a reference genome, this book won’t tell you the newest and best alignment software for your particular system. But regardless of which aligner you use, you will need to have a thorough understanding of alignment formats and how to manipulate alignment data —a topic covered in Chapter 11. Throughout this book, these general computational and data skills are meant to be a solid, widely applicable foundation on which the majority of biologists can build. 

If you’re a computer scientist or programmer, you are likely already familiar with some of the computational tools I teach in this book. While the material presented in Bioinformatics Data Skills may overlap knowledge you already have, you will still learn about the specific formats, tools, and approaches bioinformaticians use in their work. Also, working through the examples in this book will give you good practice in applying your computational skills to genomics data. 

## The Difculty Level of

Bioinformatics Data Skills is designed to be a thorough—and in parts, dense—book. When I started writing this book, I decided the greatest misdeed I could do would be to treat bioinformatics as a subject that’s easier than it truly is. Working as a professio‐ nal bioinformatician, I routinely saw how very subtle issues could crop up and adversely change the outcome of the analysis had they not been caught. I don’t want your bioinformatics work to be incorrect because I’ve made a topic artificially simple. The depth at which I cover topics in Bioinformatics Data Skills is meant to prepare you to catch similar issues in your own work so your results are robust. 

The result is that sections of this book are quite advanced and will be difficult for some readers. Don’t feel discouraged! Like most of science, this material is hard, and may take a few reads before it fully sinks in. Throughout the book, I try to indicate when certain sections are especially advanced so that you can skip over these and return to them later. 

Lastly, I often use technical jargon throughout the book. I don’t like using jargon, but it’s necessary to communicate technical concepts in computing. Primarily it will help you search for additional resources and help. It’s much easier to Google successfully for “left outer join” than “data merge where null records are included in one table.” 

## Assumptions This Book Makes

Bioinformatics Data Skills is meant to be an intermediate book on bioinformatics. To make sure everyone starts out on the same foot, the book begins with a few simple chapters. In Chapter 2, I cover the basics of setting up a bioinformatics project, and in Chapter 3 I teach some remedial Unix topics meant to ensure that you have a solid grasp of Unix (because Unix is a large component in later chapters). Still, as an inter‐ mediate book, I make a few assumptions about you: 

## You know a scripting language

This is the biggest assumption of the book. Except for a few Python programs and the R material (R is introduced in Chapter 8), this book doesn’t directly rely on using lots of scripting. However, in learning a scripting language, you’ve already encountered many important computing concepts such as working with a text editor, running and executing programs on the command line, and basic programming. If you do not know a scripting language, I would recommend learning Python while reading this book. Books like Bioinformatics Programming Using Python by Mitchell L. Model (O’Reilly, 2009), Learning Python, 5th Edition, by Mark Lutz (O’Reilly, 2013), and Python in a Nutshell, 2nd, by Alex Martelli (O’Reilly, 2006) are great to get started. If you know a scripting language other than Python (e.g., Perl or Ruby), you’ll be prepared to follow along with most examples (though you will need to translate some examples to your scripting lan‐ guage of choice). 

## You know how to use a text editor

It’s essential that you know your way around a text editor (e.g., Emacs, Vim, Text‐ Mate2, or Sublime Text). Using a word processor (e.g., Microsoft Word) will not work, and I would discourage using text editors such as Notepad or OS X’s Tex‐ tEdit, as they lack syntax highlighting support for common programming lan‐ guages. 

## You have basic Unix command-line skills

For example, I assume you know the difference between a terminal and a shell, understand how to enter commands, what command-line options/flags and arguments are, and how to use the up arrow to retrieve your last entered com‐ mand. You should also have a basic understanding of the Unix file hierarchy (including concepts like your home directory, relative versus absolute directories, and root directories). You should also be able to move about and manipulate the directories and files in Unix with commands like cd, ls, pwd, mv, rm, rmdir, and mkdir. Finally, you should have a basic grasp of Unix file ownership and permis‐ sions, and changing these with chown and chmod. If these concepts are unclear, I would recommend you play around in the Unix command line first (carefully!) and consult a good beginner-level book such as Practical Computing for Biologists by Steven Haddock and Casey Dunn (Sinauer, 2010) or UNIX and Perl to the Res‐ cue by Keith Bradnam and Ian Korf (Cambridge University Press, 2012). 

## You have a basic understanding of biology

Bioinformatics Data Skills is a BYOB book—bring your own biology. The examples don’t require a lot of background in biology beyond what DNA, RNA, proteins, and genes are, and the central dogma of molecular biology. You should also be familiar with some very basic genetics and genomic concepts (e.g., single nucleo‐ tide polymorphisms, genotypes, GC content, etc.). All biological examples in the book are designed to be quite simple; if you’re unfamiliar with any topic, you should be able to quickly skim a Wikipedia article and proceed with the example. 

## You have a basic understanding of regular expressions

Occasionally, I’ll make use of regular expressions in this book. In most cases, I try to quickly step through the basics of how a regular expression works so that you can get the general idea. If you’ve encountered regular expressions while learning a scripting language, you’re ready to go. If not, I recommend you learn the basics —not because regular expressions are used heavily throughout the book, but because mastering regular expressions is an important skill in bioinformatics. Introducing Regular Expressions by Michael Fitzgerald (O’Reilly) is a great intro‐ duction. Nowadays, writing, testing, and debugging regular expressions is easier than ever thanks to online tools like http://regex101.com and http://www.debug‐ gex.com. I recommend using these tools in your own work and when stepping through my regular expression examples. 

## You know how to get help and read documentation

Throughout this book, I try to minimize teaching information that can be found in manual pages, help documentation, or online. This is for three reasons: 

• I want to save space and focus on presenting material in a way you can’t find elsewhere 

• Manual pages and documentation will always be the best resource for this information 

• The ability to quickly find answers in documentation is one of the most important skills you can develop when learning computing 

This last point is especially important; you don’t need to remember all arguments of a command or R function—you just need to know where to find this information. Pro‐ grammers consult documentation constantly in their work, which is why documenta‐ tion tools like man (in Unix) and help() (in R) exist. 

## You can manage your computer system (or have a system administrator)

This book does not teach you system administration skills like setting up a bioin‐ formatics server or cluster, managing user accounts, network security, managing disks and disk space, RAID configurations, data backup, and high-performance computing concepts. There simply isn’t the space to adequately cover these important topics. However, these are all very, very important—if you don’t have a system administrator and need to fill that role for your lab or research group, it’s essential for you to master these skills, too. Frankly, system administration skills take years to master and good sysadmins have incredible patience and experience in handling issues that would make most scientists go insane. If you can employ a full-time system administrator shared across labs or groups or utilize a university cluster with a sysadmin, I would do this. Lastly, this shouldn’t need to be said, but just in case: constantly back up your data and work. It’s easy when learning Unix to execute a command that destroys files—your best protection from losing everything is continual backups. 

## Supplementary Material on GitHub

The supplementary material needed for this book’s examples is available in the Git‐ Hub repository. You can download material from this repository as you need it (the repository is organized by chapter), or you can download everything using the Download Zip link. Once you learn Git in Chapter 5, I would recommend cloning the repository so that you can restore any example files should you accidentally over‐ write them. 

Try navigating to this repository now and poking around so you’re familiar with the layout. Look in the Preface’s directory and you’ll find the README.md file, which includes additional information about many of the topics I’ve discussed. In addition to the supplementary files needed for all examples in the book, this repository con‐ tains: 

• Documentation on how all supplementary files were produced or how they were acquired. In some cases, I’ve used makefiles or scripts (both of these topics are covered in Chapter 12) to create example data, and all of these resources are available in each chapter’s GitHub directory. I’ve included these materials not only for reproducible purposes, but also to serve as additional learning material. 

• Additional information readers may find interesting for each chapter. This infor‐ mation is in each chapter’s README.md file. I’ve also included other resources like lists of recommended books for further learning. 

• Errata, and any necessary updates if material becomes outdated for some reason. 

I chose to host the supplementary files for Bioinformatics Data Skills on GitHub so that I could keep everything up to date and address any issues readers may have. Feel free to create a new issue on GitHub should you find any problem with the book or its supplementary material. 

## Computing Resources and Setup

I’ve written this entire book on my laptop, a 15-inch MacBook Pro with 16 GB of RAM. Although this is a powerful laptop, it is much smaller than the servers common in bioinformatics computing. All examples are designed and tested to run a machine this size. Nearly every example should run on a machine with 8 GB of memory. 

All examples in this book work on Mac OS X and Linux—other operating systems are not supported (mostly because modern bioinformatics relies on Unix-based operat‐ ing systems). All software required throughout the book is freely available and is easily installable; I provide some basic instructions in each section as software instal‐ lation is needed. In general, you should use your operating system’s package manage‐ ment system (e.g., apt-get on Ubuntu/Debian). If you’re using a Mac, I highly recommend Homebrew, a terrific package manager for OS X that allows you to easily install software from the command line. You can find detailed instructions on Home‐ brew’s website, Most important, Homebrew maintains a collection of scientific soft‐ ware packages called homebrew-science, including the bioinformatics software we use throughout this book. Follow the directions in homebrew-science’s README.md to learn how to install these scientific programs. 

## Organization of This Book

This book is composed of three parts: Part I, containing one chapter on ideology; Part II, which covers the basics of getting started with a bioinformatics project; and Part III, which covers bioinformatics data skills. Although chapters were written to be read sequentially, if you’re comfortable with Unix and R, you may find that you can skip around without problems. 

In Chapter 1, I introduce why learning bioinformatics by developing data skills is the best approach. I also introduce the ideology of this book, and describe reproducible and robust bioinformatics and some recommendations to apply in your own work. 

Part II of Bioinformatics Data Skills introduces the basic skills needed to start a bioin‐ formatics project. First, we’ll look at how to set up and manage a project directory in Chapter 2. This may seem like trivial topic, but complex bioinformatics projects demand we think about project management. In the frenzy of research, there will be files everywhere. Starting out with a carefully organized project can prevent a lot of hassle in the future. We’ll also learn about documentation with Markdown, a useful format for plain-text project documentation. 

In Chapter 3, we explore intermediate Unix in bioinformatics. This is to make sure that you have a solid grasp of essential concepts (e.g., pipes, redirection, standard input and output, etc.). Understanding these prerequisite topics will allow you to focus on analyzing data in later chapters, not struggling to understand Unix basics. 

Most bioinformatics tasks require more computing power than we have on our per‐ sonal workstations, meaning we have to work with remote servers and clusters. Chapter 4 covers some tips and tricks to increase your productivity when working with remote machines. 

In Chapter 5, we learn Git, which is a version control system that makes managing versions of projects easy. Bioinformatics projects are filled with lots of code and data that should be managed using the same modern tools as collaboratively developed software. Git is a large, powerful piece of software, so this is a long chapter. However, this chapter was written so that you could skip the section on branching and return to it later. 

Chapter 6 looks at data in bioinformatics projects: how to download large amounts of data, use data compression, validate data integrity, and reproducibly download data for a project. 

In Part III, our attention turns to developing the essential data skills all bioinformati‐ cians need to tackle problems in their daily work. Chapter 7 focuses on Unix data tools, which allow you to quickly write powerful stream-processing Unix pipelines to process bioinformatics data. This approach is a cornerstone of modern bioinformat‐ ics, and is an absolutely essential data skill to have. 

In Chapter 8, I introduce the R language through learning exploratory data analysis techniques. This chapter prepares you to use R to explore your own data using tech‐ niques like visualization and data summaries. 

Genomic range data is ubiquitous in bioinformatics, so we look at range data and range operations in Chapter 9. We’ll first step through the different ways to represent genomic ranges, and work through range operations using Bioconductor’s IRanges package to bolster our range-thinking intuition. Then, we’ll work with genomic data using GenomicRanges. Finally, we’ll look at the BEDTools Suite of tools for working with range data on the command line. 

In Chapter 10, we learn about sequence data, a mainstay of bioinformatics data. We’ll look at the FASTA and FASTQ formats (and their limitations) and work through trimming low-quality bases off of sequences and seeing how this affects the distribu‐ tion of quality scores. We’ll also look at FASTA and FASTQ parsing. 

Chapter 11 focuses on the alignment data formats SAM and BAM. Understanding and manipulating files in these formats is an integral bioinformatics skill in working with high-throughput sequencing data. We’ll see how to use Samtools to manipulate these files and visualize the data, and step through a detailed example that illustrates some of the intricacies of variant calling. Finally, we’ll learn how to use Pysam to parse SAM/BAM files so you can write your own scripts that work with these special‐ ized data formats. 

Most daily bioinformatics work involves writing data-processing scripts and pipe‐ lines. In Chapter 12, we look at how to write such data-processing pipelines in a robust and reproducible way. We’ll look specifically at Bash scripting, manipulating files using Unix powertools like find and xargs, and finally take a quick look at how you can write pipelines using Make and makefiles. 

In bioinformatics, our data is often too large to fit in our computer’s memory. In Chapter 7, we saw how streaming with Unix pipes can help to solve this problem, but Chapter 13 looks at a different method: out-of-memory approaches. First, we’ll look at Tabix, a fast way to access information in indexed tab-delimited files. Then, we’ll look at the basics of SQL through analyzing some GWAS data using SQLite. 

Finally, in Chapter 14, I discuss where you should head next to further develop your bioinformatics skills. 

## Code Conventions

Most bioinformatics data has one thing in common: it’s large. In code examples, I often need to truncate the output to have it fit into the width of a page. To indicate that output has been truncated, I will always use [...] in the output. Also, in code examples I often use variable names that are short to save space. I encourage you to use more descriptive names than those I’ve used throughout this book in your own personal work. 

## Conventions Used in This Book

The following typographical conventions are used in this book: 

## Italic

Indicates new terms, URLs, email addresses, filenames, and file extensions. 

## Constant width

Used for program listings, as well as within paragraphs to refer to program ele ments such as variable or function names, databases, data types, environment variables, statements, and keywords. 

## Constant width bold

Shows commands or other text that should be typed literally by the user. 

## Constant width italic

Shows text that should be replaced with user-supplied values or by values deter‐ mined by context. 

![image](images/fig_001.jpg)


This element signifies a tip or suggestion. 

![image](images/fig_002.jpg)



This element indicates a warning or caution.


## Using Code Examples

This book is here to help you get your job done. In general, if example code is offered with this book, you may use it in your programs and documentation. You do not need to contact us for permission unless you’re reproducing a significant portion of the code. For example, writing a program that uses several chunks of code from this book does not require permission. Selling or distributing a CD-ROM of examples from O’Reilly books does require permission. Answering a question by citing this book and quoting example code does not require permission. Incorporating a signifi‐ cant amount of example code from this book into your product’s documentation does require permission. 

We appreciate, but do not require, attribution. An attribution usually includes the title, author, publisher, and ISBN. For example: “Bioinformatics Data Skills by Vince Buffalo (O’Reilly). Copyright 2015 Vince Buffalo, 978-1-449-36737-4.” 

If you feel your use of code examples falls outside fair use or the permission given above, feel free to contact us at permissions@oreilly.com. 

## Safari® Books Online

![image](images/fig_003.jpg)


Safari Books Online is an on-demand digital library that deliv‐ ers expert content in both book and video form from the world’s leading authors in technology and business. 

Technology professionals, software developers, web designers, and business and crea‐ tive professionals use Safari Books Online as their primary resource for research, problem solving, learning, and certification training. 

Safari Books Online offers a range of plans and pricing for enterprise, government, education, and individuals. 

Members have access to thousands of books, training videos, and prepublication manuscripts in one fully searchable database from publishers like O’Reilly Media, Prentice Hall Professional, Addison-Wesley Professional, Microsoft Press, Sams, Que, Peachpit Press, Focal Press, Cisco Press, John Wiley & Sons, Syngress, Morgan Kauf‐ mann, IBM Redbooks, Packt, Adobe Press, FT Press, Apress, Manning, New Riders, McGraw-Hill, Jones & Bartlett, Course Technology, and hundreds more. For more information about Safari Books Online, please visit us online. 

## How to Contact Us

Please address comments and questions concerning this book to the publisher: 

O’Reilly Media, Inc. 

1005 Gravenstein Highway North 

Sebastopol, CA 95472 

800-998-9938 (in the United States or Canada) 

707-829-0515 (international or local) 

707-829-0104 (fax) 

We have a web page for this book, where we list errata, examples, and any additional information. You can access this page at http://bit.ly/Bio-DS. 

To comment or ask technical questions about this book, send email to bookques‐ tions@oreilly.com. 

For more information about our books, courses, conferences, and news, see our web‐ site at http://www.oreilly.com. 

Find us on Facebook: http://facebook.com/oreilly 

Follow us on Twitter: http://twitter.com/oreillymedia 

Watch us on YouTube: http://www.youtube.com/oreillymedia 

## Acknowledgments

Writing a book is a monumental effort—for two years, I’ve worked on Bioinformatics Data Skills during nights and weekends. This is in addition to a demanding career as a professional bioinformatician (and for the last five months of writing, as a PhD stu‐ dent). Balancing work and life is already difficult enough for most scientists; I now know that balancing work, life, and writing a book is nearly impossible. I wouldn’t have survived this process without the support of my partner, Helene Hopfer. 

I thank Ciera Martinez for continually providing useful feedback and helping me cali‐ brate the tone and target audience of this book. Cody Markelz tirelessly provided feedback and was never afraid to say when I’d missed the mark on a chapter—for this, all readers should be thankful. My friend Al Marks deserves special recognition not only for proving valuable feedback on many chapters, but also for introducing me to computing and programming back in high school. I also thank Jeff Ross-Ibarra for inspiring my passion for population genetics and presenting me with challenging and interesting projects in his lab. I owe a debt of gratitude to the entire UC Davis Bioin‐ formatics Core for the fantastic time I spent working there; thanks especially to Dawei Lin, Joe Fass, Nikhil Joshi, and Monica Britton for sharing their knowledge and granting me freedom to explore bioinformatics. Mike Lewis also deserves a spe‐ cial thanks for teaching me about computing and being a terrific person to nerd out on techie details with. Peter Morrell, his lab, and the “Does[0]compute?” reading group provided lots of useful feedback that I’m quite grateful for. I thank Jorge Dub‐ covsky—witnessing his tireless pursuit of science has motivated me to do the same. Lastly, I’m indebted to my wonderful advisor, Graham Coop, for his patience in allowing me to finish this book—with this book out of the way, I’m eager to pursue my future directions under his mentorship. 

This book was significantly improved by the valuable input of many reviewers, collea‐ gues, and friends. Thank you Peter Cock, Titus Brown, Keith Bradnam, Mike Coving‐ ton, Richard Smith-Unna, Stephen Turner, Karthik Ram, Gabe Becker, Noam Ross, Chris Hamm, Stephen Pearce, Anke Schennink, Patrik D’haeseleer, Bill Broadley, Kate Crosby, Arun Durvasula, Aaron Quinlan, and David Ruddock. Shaun Jackman deserves recognition for his tireless effort in making bioinformatics software easy to install through the Homebrew and apt-get projects—my readers will greatly benefit from this. I also am grateful for the comments and positive feedback I received from many of the early release readers of this book; the positive reception provided a great motivating push to finish everything. However, as author, I do take full credit for any errors or omissions that have slipped by these devoted reviewers. 

Most authors are lucky if they work with one great editor—I got to work with two. Thank you, Courtney Nash and Amy Jollymore, for your continued effort and encouragement throughout this process. Simply put, I wouldn’t have been able to do this without you both. I’d also like to thank my production editor Nicole Shelby, copyeditor Jasmine Kwityn, and the rest of the O’Reilly production team for their extremely hard work in editing Bioinformatics Data Skills. Finally, thank you, Mike Loukides, for your feedback and for taking an interest in my book when it was just a collection of early, rough ideas—you saw more. 

Ideology: Data Skills for Robust and Reproducible Bioinformatics