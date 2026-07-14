# Conclusion

When I set out to write Bioinformatics Data Skills, I initially struggled with how I could present intermediate-level bioinformatics in book format in a way that wouldn’t quickly become obsolete in the fast-moving field of bioinformatics. Even in the time it has taken to complete my book, new shiny algorithms, statistical methods, and bio‐ informatics software have been released and adopted by the bioinformatics commu‐ nity. It’s possible (perhaps even likely) that new sequencing technology will again revolutionize biology and bioinformaticians will need to adapt their approaches and tools. How can a print book be a valuable learning asset in this changing environ‐ ment? 

I found the solution to this problem by looking at the tools I use most in my everyday bioinformatics work: Unix, Python, and R. Unix dates back to the early 1970s, mak‐ ing it over 40 years old. The initial release of Python was in 1991 and R was born soon after in 1993, making both of these languages over 20 years old. These tools have all stood the test of time and are the foundation of modern data processing and statistical computing. Bioinformatics and Unix have a nearly inseparable history—the necessary first step of learning bioinformatics skills is to learn Unix. While genomics is rapidly evolving, bioinformaticians continue to reach for same standard tools to tackle new problems and analyze new datasets. Furthermore, Unix, Python, and R are all extensible tools. Nearly every new bioinformatics program is designed to be used on the Unix command line. Working with the newest bioinformatics and statistical methods often boils down to just downloading and installing to new Python and R packages. All other tools in this book—(from GenomicRanges, to sqlite3, to sam tools) are designed to work with our Unix, Python, and R data toolkits. Further‐ more, these tools work together, allowing us to mix and combine them to leverage each of their comparative advantages in our daily work—creating a foundational bio‐ informatics computing environment. While we can’t be certain what future sequenc‐ ing technology will allow us to do, we can be confident that Unix, Python, and R will continue to be the foundation of modern bioinformatics. 

However, powerful tools alone don’t create a proficient bioinformatician. Using pow‐ erful tools to adeptly solve real problems requires an advanced set of skills. With bio‐ informatics, this set of skills is only fully developed after years of working with real data. Bioinformatics Data Skills focuses on a robust and reproducible approach because this is the best context in which to develop your bioinformatics skills. Dis‐ trust of one’s tools and data and awareness of the numerous pitfalls that can occur during analysis is one of the most important skills to develop. However, you’ll only fully develop these skills when you’ve encountered and been surprised by serious issues in your own research. 

## Where to Go From Here?

Bioinformatics Data Skills was designed for readers familiar with scripting and a bit of Unix, but less so with how to apply these skills to everyday bioinformatics problems. Throughout the book, we’ve seen many other tools and learned important skills to solve nearly any problem in bioinformatics (alongside running tools like aligners, assemblers, etc.). Where do you go from here? 

First, I’d recommend you learn more statistics and probability. It’s impossible to emphasize this point too much. After learning the skills in this book and honing them on real-world data, the next step to becoming a masterful bioinformatician is learning statistics and probability. The practical skills are primarily computational; to turn a glut of genomics data into meaningful biological knowledge depends critically on your ability to use statistics. Similarly, understanding probability theory and hav‐ ing the skills to apply it to biological problems grants you with an entirely new way to approach problems. Even applying simple probabilistic approaches in your own work can free you from unpleasant heuristic methods and often work much, much better. Furthermore, many new innovative bioinformatics methods are built on probabilistic models—being familiar with the underlying probabilistic mechanics is crucial to understanding why these methods work and under which conditions they might not. 

Second, I would recommend learning some basic topics in computer science—espe‐ cially algorithms and data structures. We continuously need to process large amounts of data in bioinformatics, and it’s far too easy to take an approach that’s needlessly computationally inefficient. All too often researchers reach for more computational power to parallelize code that could easily run on their desktop machines if it were written more efficiently. Fortunately, it only takes some basic understanding of algo‐ rithms and data structures to design efficient software and scripts. I’d also recom‐ mend learning about specialized bioinformatics algorithms used in aligners and assemblers; having an in-depth knowledge of these algorithms can help you under‐ stand the limitation of bioinformatics software and choose the right tool (and develop your own if you like!). 

For more direction into these topics, see this chapter’s README file on GitHub. I’ve included my favorite books on these subjects there—and will continue to add others as I discover them. Finally, the last piece of advice I can give you in your path toward becoming a skilled bioinformatician is to use the source. In other words, read code, and read lots of code—(especially from programmers who are more skilled than you). Developing programming skills is 90% about experience—writing, debugging, and wrestling with code for years and years. But reading and learning from others’ code is like a secret shortcut in this process. While it can be daunting at first to stare at hundreds of thousands of lines of someone else’s complex code, you simultaneously strengthen your ability to quickly understand code while learning the tricks a pro‐ grammer more clever than you uses. Over time, the exercise of reading others’ code will reward you with better programming skills. 

## alignment

(1) The process of ordering a sequence such as DNA, protein, or RNA to another sequence that can be used to infer evolu‐ tionary relationships (e.g., homology) or sequence origin (e.g., a sequencing read aligned to a particular region of a chro‐ mosome). (2) A single aligned pair of sequences. 

## allele

An alternative form of a gene at a particu‐ lar locus. For example, SNP rs17822931 has two possible alleles (C and T) that determine earwax type. Individuals that have a C allele (e.g., their genotype is either CC or CT) have wet earwax, while individuals with two T alleles (e.g., their genotype is TT) have dry earwax. 

## AND

AND is a logical operator commonly used in programming languages. x AND y has the value true if and only if x and y are both true. In Python, the logical AND operator is and; in R, it is either && (for AND on an entire vector) or & (for element-wise AND). 

## anonymous function

A temporary function (used only once) without a name. Anonymous functions are commonly used in R sapply() or lapply() statements. Python also sup‐ ports anonymous functions through its lambda expressions (e.g., lambda x: 2*x). 

## application programming interface (API)

An API or application programming interface is a defined interface to some software component, such as a database or file format (e.g., SAM/BAM files). APIs are often modules or libraries that you can load in and utilize in your software projects, allowing you to use a preexisting set of routines that work with low-level details rather than writing your own. 

## ASCII (pronounced ask-ee)

A character encoding format that encodes for 128 different characters. The acronym stands for American Standard Code for Information Interchange. ASCII charac‐ ters take up 7 bits and are usually stored in a single byte in memory (one byte is 8 bits). In addition to the common letters and punctuation characters used in English, the ASCII scheme also supports 33 nonprinting characters. See man ascii for reference. 

## bacterial artifcial chromosomes (BACs)

A type of DNA construct used to clone DNA (up to 350,000 base pairs). 

## base calling

The process of inferring a nucleotide base from its raw signal (e.g., light intensity data) from a sequencer. 

## batch efects

## batch efects

Undesirable technical effects that can con‐ found a sequencing experiment. For example, if two different sequencing libra‐ ries are prepared using different reagents, observed expression differences between the two libraries could be due to batch effects (and is completely confounded by the reagent used). See Leek et al. (2010) for a good review on batch effects. 

## Binary Call Format (BCF)

The binary version of Variant Call Format (VCF); see Variant Call Format for more information. 

## BEDTools

A software suite of command-line tools for manipulating genomic range data in the BED, GFF, VCF, and BAM formats. 

## Blocked GNU Zip Format (BGZF)

A variant of GNU zip (or gzip) compres‐ sion that compresses a file in blocks rather than in its entirety. Block compression is often used with tools like Tabix that can seek to a specific block and uncompress it, rather than requiring the entire file be uncompressed. This allows for fast ran‐ dom access of large compressed files. 

## binary

(1) The base-2 numeric system that underlies computing, where values take only 0 (true) or 1 (false). (2) A file is said to be in binary if it’s not human-readable plain text. 

## BioMart

A software project that develops tools and databases to organize diverse types of bio‐ logical data, and simplifies querying infor‐ mation out of these databases. Large genomic databases like Ensembl use Bio‐ Mart tools for data querying and retrieval. 

## bit

Short for binary digit, a bit is the smallest value represented on computer hardware; a 0 or 1 value. 

## bit feld or bitwise fag

A technique used to store multiple true/ false values using bits. Bit fields or bitwise flags are used in SAM and BAM files to store information about alignments such as “is paired” or “is unmapped.” 

## binary large object (BLOB)

A data type used in database systems for storing binary data. 

## brace expansion

A type of shell expansion in Unix that expands out comma-separated values in braces to all combinations. For example, in the shell {dog,cat,rat}-food expands to dog-food cat-food rat-food. Unlike wildcard expansion, there’s no guarantee that expanded results have corresponding files. 

## branch

In Git, a branch is a path of development in a Git repository; alternative paths can be created by creating new branches. By default, Git commits are made to the mas‐ ter branch. Alternative branches are often used to separate new features or bug fixes from the main working version of your code on the master branch. 

## breakpoint

A point in code where execution is tem‐ porarily paused so the developer can debug code at that point. A breakpoint can be inserted in R code by calling the function browser() and in Python code using pdb.set_trace() (after importing the Python Debugger with import pdb). 

## byte

A common unit in computing equal to eight bits. 

## call

A function evaluated with supplied argu‐ ments. For example, in Python sum is a function we can call on a list like sum([1, 2, 3, 4]) to sum the list values. 

## call stack

A data structure used in programming languages to store data used in open func‐ tion calls. The call stack can be inspected when debugging code in Python using where in the Python debugger and in R using the traceback() function. 

## calling out or shelling out

Executing another program from the Unix shell from within a programming language. For example, in R: system (echo "this is a shellout"). Python’s subprocess module is used for calling processes from Python. 

## capturing groups

A grouped regular expression used to cap‐ ture matching text. For example, captur‐ ing groups could be used to capture the chromosome name in a string like chr3:831991,832018 with a regular expression like (.*):. For instance, with Python: re.match(r'(.*):', "chr3:831991,832018").groups() (after the re module has been imported with import re). 

## character class

A regular expression component that matches any single character specified between square brackets—for example, [ATCG] would match any single A, T, C, or G. 

## checksums

A special summary of digital data used to ensure data integrity and warns against data corruption. Checksums (such as SHA and MD5) are small summaries of data that change when the data changes even the slightest amount. 

## CIGAR

A format used to store alignment details in SAM/BAM and other bioinformatics formats. Short for the “Compact Idiosyn‐ cratic Gapped Alignment Report.” 

## coercion

In the context of programming languages, coercion is the process of changing one data type to another. For example, numeric data like “54.21” can be stored in a string, and needs to be coerced to a floating-point data type before being manipulated numerically. In R, this would be accomplished with as.numeric("54.21"), and in Python with float("54.21"). 

## command substitution

A technique used to create anonymous named pipes. 

## commit

In Git, a commit is a snapshot of your project’s current state. 

## constructor

A function used to create and initialize (also known as instantiate) a new object. 

## contig

Short for contiguous sequence of DNA; often used to describe an assembled DNA sequence (often which isn’t a full chromo‐ some). Consecutive contigs can be scafol‐ ded together into a longer sequence, which can contain gaps of unknown sequence and length. 

## coordinate system

How coordinates are represented in data. For example, some genomic range coordi‐ nate systems give the first base in a sequence the position 0 while others give it the position 1. Knowing which coordi‐ nate system is used is necessary to prevent errors; see Chapter 9 for details. 

## corner case

Roughly, an unusual case that occurs when data or parameters take extreme or unexpected values. Usually, a corner case refers to cases where a program may func‐ tion improperly, unexpectedly, or return incorrect results. Also sometimes referred to as an edge case (which is technically different, but very similar). 

## Coverage

Short for depth of coverage; in bioinfor‐ matics, this refers to the average depth of sequencing reads across a genome. For example, “10x coverage” means on aver‐ age, each base pair of a sequence is cov‐ ered by 10 reads. 

## CRAM

A compressed format for storing align‐ ments similar to BAM, which compresses data by only storing differences from the alignment reference (which must be stored too to uncompress files). 

## CRAN or Comprehensive R Archive Network

A collection of R packages for a variety of statistical and data analysis methods. Packages on CRAN can be installed with install.packages(). 

## data integrity

Data integrity is the state of data being free from corruption. 

## database dumps

An export of the table schemas and data in a database used to back up or duplicate a database and its tables. Database dumps contain the necessary SQL commands to entirely re-create the database and propa‐ gate it with its contents. 

## declarative language

A programming language paradigm where the user writes code that specifies the result user wants, but not how to per‐ form the computation to arrive at that result. For example, SQL is a declarative language because the relational database management system translates a query describing to the necessary computational steps to produce that result. 

## dependencies

Dependencies are required data, code, or other external information required by a program to function properly. For exam‐ ple, software may have other software programs it requires to run—these are called dependencies. 

## dif

(1) A Unix tool (diff) for calculating the difference between files. (2) A file pro‐ duced by diff or other programs (e.g., git diff) that represents the difference between two file versions. Diff files are also sometimes called patches, as they con‐ tain the necessary information to turn the original file into the modified file (in other words, to “patch” the other file). 

## Exploratory Data Analaysis or EDA

A statistical technique pioneered by John Tukey that relies on learning about data not through explicit statistical modeling, but rather exploring the data using sum‐ mary statistics and especially visualiza‐ tion. 

## environmental variables

In the Unix shell, environmental variables are global variables shared between all shells and applications. These can contain configurations like your terminal pager ($PAGER) or your terminal prompt ($PS1). Contrast to local variables, which are only available to the current shell. 

## exit status

An integer value returned by a Unix pro‐ gram to indicate whether the program completed successfully or with an error. Exit statuses with the value zero indicate success, while any other value indicates an error. 

## foreign key

A key used in one database table that uniquely refers to the entries of another table. 

## fork, forking

In GitHub lingo, forking refers to cloning a GitHub user’s repository to your own GitHub account. This allows you to develop your own version of the reposi‐ tory, and then submit pull requests to share your changes with the original project. 

## FTP

An acronym for File Transfer Protocol, a protocol used to transfer files over the Internet by connecting an FTP client to an FTP server. 

## GC content

The percentage of guanine and cytosine (nitrogenous) bases in a DNA sequence. 

## genomic range

A region on a genomic sequence, speci‐ fied by chromosome or sequence name, start position, end position, and strand. 

## Git

A distributed version control system. 

## globbing

A type of Unix shell expansion that expands a pattern containing the Unix wildcard character * to all existing and matching files. 

## greedy

In regular expressions, a greedy pattern is one that matches as many characters as possible. For example, the regular expres‐ sion (.*): applied to string “1:2:3” would not match just “1”, but “1:2” because it is greedy. 

## hangup

A Unix signal (SIGHUP) that indicates a terminal has been closed. Hangups cause most applications to exit, which can be a problem for long-running bioinformatics applications. A terminal multiplexer like tmux or the command-line tool nohup are used to prevent hangups from terminating programs (see Chapter 4). 

## hard masked

A hard masked sequence contains certain bases (e.g., low-complexity repeat sequen‐ ces) that are masked out using Ns. In con‐ trast, sof masked sequences are masked using lowercase bases (e.g., a, t, c, and g). 

## HEAD

In Git, HEAD is a pointer to the current branch’s latest commit. 

## HTTP

An acronym for Hypertext Transfer Proto‐ col; HTTP is the protocol used to transfer web content across the World Wide Web. 

## hunks

Sections of a dif file that represent dis‐ crete blocks of changes. 

## indexed, indexing

A computational technique used to speed up lookup operations, often used to decrease the time needed for access to various random (e.g., nonsequential) parts of a file. Indexing is used by many bioinformatics tools such as aligners, Samtools, and Tabix. 

## inner join

The most commonly used database join used to join two tables on one or more keys. The result of an inner join only con‐ tains rows with matching non-null keys in both tables; contrast with lef outer join and right outer join. 

## interval tree

A data structure used to store range data in a way that optimizes finding overlap‐ ping ranges. Interval trees are used in some genomic range libraries like Biocon‐ ductor’s GenomicRanges and the command-line BEDTools Suite. 

## IUPAC or International Union of Pure and Applied Chemistry

An organization known for standardizing symbols and names in chemistry; known in bioinformatics for the IUPAC nucleo‐ tide base codes. 

## key

In sorting, a sort key is the field or column used to sort data on. In databases, a key is a unique identifier for each entry in a table. 

## LaTeX

A document markup language and type‐ setting system often used for technical sci‐ entific and mathematical documents. 

## leading zeros

## leading zeros

A number with leading zeros is printed with the same number of digits—for example, 00002412 and 00000337. Encod‐ ing identifiers with leading zeros is a use‐ ful trick, as these identifiers automatically sort lexicographically. 

## left outer join

A type of database join in which all rows of the left table are included, but only matching rows of the right table are kept. Note that the left and right tables are the tables left and right of the SQL join state‐ ment, (e.g., with x LEFT OUTER JOIN y, x is the left table and y is the right table). A right outer join is the same as a left outer join with right and left tables switched. 

## literate programming

A style of programming pioneered by Donald Knuth that intermixes natural lan‐ guage with code. Code in the literate pro‐ gram can be separated out and executed. 

## local variables

See environmental variables. 

## locus, loci (plural)

The position of a gene. 

## lossless compression

A data compression scheme where no information is lost in the compression process. 

## lossy compression

A data compression scheme where data is intentionally lost in the compression scheme to save space. 

## master branch

See branch. 

## mapping quality

A measure of how likely a read is to actually originate from the position it maps to. 

## Markdown

A lightweight document markup language that extends plain-text formats. Mark‐ down documents can be rendered in HTML and many other languages. 

## metadata

Data or information about other data or a dataset. For example, a reference genome’s version, creation data, etc. are all metadata about the reference genome. 

## named pipes

A type of Unix pipe that’s represented as a special type of file on the filesystem. Data can be written to and read from a named pipe as if it were a regular file, but does not write to the disk, instead connecting the reading and writing processes with a stream. 

## NOT

A logical operator for negation; returns the logical opposite of the value it’s called on. For example, NOT true evaluates to false and NOT false evaluates to true. 

## ofset ofset

Both FASTQ and file offset (in context of seek). 

## OR

A logical operator where x OR y is true if either x or y are true, but false if neither x nor y are true. 

## overplotting

Overplotting occurs when a plot contains a large amount of close data points that crowd the visualization and make under‐ standing and extracting information from the graphic difficult. 

## pager, terminal pager

(1) A command-line tool used to view and scroll through a text file or text stream of data that’s too large to display in a single terminal screen (e.g., the Unix tools less and more). (2) The terminal pager config‐ ured with the environmental variable $PAGER. 

## patch fle

See dif. 

## plain text

A text format that does not contain special formatting or file encoding and is human readable. 

## POSIX Extended Regular Expressions or POSIX ERE

A variety of regular expression used by Unix programs that have support for additional features; contrast to POSIX Basic Regular Expressions. These extended feature regular expressions can often be enabled in Unix programs. 

## POSIX Basic Regular Expressions or POSIX BRE

See POSIX Extended Regular Expressions. 

## primary keys

A unique key used in a database table to identify a particular entry. 

## public key or private key

In SSH, an SSH public key is a key that’s shared across systems to grant login access to the user with the associated pri‐ vate key. Public keys are shareable, but private keys must be protected by the user (see Chapter 2). 

## relational databases

A type of database used to store relation‐ ships among data contained across differ‐ ent tables. 

## relational database management system

The software system that implements the tools to work with a relational database. 

## repository

In Git, a repository is a directory contain‐ ing code or other files that are being man‐ aged by Git. 

## right outer join

See lef outer join. 

## R markdown

A variant of the markdown format that interweaves R code with markdown text. The R packages rmarkdown and knitr can be used to render R markdown files. 

## robust

In software, robust means protected against common data problems and soft‐ ware bugs; the opposite of fragile soft‐ ware, which may break silently and unexpectedly when it encounters certain data or parameters. 

## S3 object orientation

R’s default object-orientation system; con‐ trast to S4 object orientation, which is used by Bioconductor and some other R pack‐ ages. 

## Sanger sequencing

A method of DNA sequencing commonly used before the advent high-throughput next-generation DNA sequencing meth‐ ods. Usually Sanger sequencing can ach‐ ieve read lengths of 700–1000bp. 

## schema

The organizational specification of a data‐ base, which describes how tables are structured, what columns they contain, and these columns’ data types, etc. 

## serialization

In programming, serialization is the pro‐ cess of storing an object on disk so that it can be loaded in later. 

## shebang

The character combination #! used to indicate which program the shell should run a script with. Shebangs should occur on the first line of a script. For example, #!/usr/bin/env python is a commonly used Python shebang. 

## soft masked

See hard masked. 

## sorting keys

See key. 

## sorting stability

A characteristic of some sorting algo‐ rithms where tied entries are sorted such that their original ordering is maintained. 

## spreadsheet syndrome

## spreadsheet syndrome

A habit of spreadsheets users to utilize as few tables as possible, which can lead to unnecessary data redundancy and ineffi‐ cient querying. In contrast, normalized databases split data into tables, which reduces redundancy. 

## SQL or Server Query Language

A declarative query language used to interact with a relational database man‐ agement system. 

## SSH or Secure Shell

A protocol used to securely connect to a machine over a network connection and initiate an encrypted shell session through which one can work with the remote machine. 

## standard error

In Unix, the standard error stream is used to relay error or other informational mes‐ sages to the user. 

## standard input

In Unix, the standard input (also known as standard in) stream is used to stream data into a program, often from a file or another program’s standard output (as with a Unix pipe). 

## standard output

In Unix, the standard output (also known as standard out) stream is a standard Unix stream used to output program results (and sometimes messages). Standard out‐ put can be redirected to a file through the Unix redirect operators > or >>. 

## unit testing

A method to test software where individ‐ ual functions, methods, or subroutines are tested as separate units. Unit testing helps protect against bugs and software regres‐ sion, which is when a newly introduced bug breaks previously functional code. 

## Variant Call Format or VCF

A plain-text tab-delimited format used to store variant positions and other variant information, and the genotypes of indi‐ viduals. 

## version control system or VCS

A system for recording changes to files (usually software code) during their development over time. For example, Git is a version control system.