## Index

Symbols (quotation marks, double), enclosing grep patterns, 141 # (hash mark), denoting comments, 135 in datasets, 195 matching using grep, 137 $ (dollar sign) in Bash variable names, 399 operator in R, 200 & (ampersand) for background processes, 50 & (logical and) operator in R, 202 && (and) operator, 52 ' ' (quotation marks, single), enclosing grep pat‐ terns, 144 - (dash) argument, 45 . (dot) commands in SQLite, 432 .indices, 458 0-based coordinate systems, 266 1-based coordinate systems, 266 IRanges and GenomicRanges packages, 271 : (colon) operator, negative indexes and, 186 ; (semicolon), sequential command execution, 53 < (redirection) operator, 45 <- (assignment) operator in R, 182, 230 = (assignment) operator in R, 182 > (redirection) operator, 42 2> operator, 44 >> (redirection) operator, 42, 44 ? (question mark) ?? function in R, 181 help function in R, 181 shell variable, 52 @ (at sign) in FASTQ format, 342 

[] (bracket) operator in R, 201 accessing list elements, 229, 230 extracting all columns by omitting column argument, 205 setting drop argument to FALSE, 202 [[]] (double bracket), accessing an element within a list, 229, 230 \\ (double backslash) in R regular expressions, 249, 257 || (or) operator, 52 ~ (tilde), specifying model formula in R, 227 Ô (caret) in regular expressions, 47 representing parent commit, 101 

## A

absolute paths, 23, 255 actions (awk), 158 ad hoc bioinformatics formats, pitfalls with, 339 aes() function including in call to ggplot(), 209 setting alpha outside of, 211 aesthetic mappings (in ggplot2), 208 in call to ggplot(), 209 mapping columns to more than one aes‐ thetic, 213 aesthetics (in ggplot2), 209 aggregate functions (SQLite), 442 common aggregate functions, 443 grouping rows with GROUP BY, 444 aggregate() function (in R), 242 algorithms and data structures, 468 alias program, 55 aligned sequencing reads 

strand and, 268 AlignedSegment object, 385 working with, 388 alignment, 471 alignment data, 355-394 command-line tools for working with file in SAM format, 365-372 creating SAM/BAM processing tools with Pysam, 384-394 pileups, variant calling, and Base Alignment Quality, 378-384 SAM and BAM formats, 355 getting to know, 356-365 visualizing alignments with samtools tview and Integrated Genomics Viewer, 372-377 alignment entries in SAM files, 359 AlignmentFile object, 385, 386 extracting SAM/BAM header information from, 387 alleles, 471 alpha (transparency level), 211 ALTER TABLE statement, 458 ambiguous nucleotide codes, 343 ambiguous strand *, 319 analysis of variance (ANOVA), 175 analysis/ directory, 22 and operator (&&), 52 annotate (bedtools), 336 annotation data working with, using GenomicFeatures and rtracklayer, 308-314 creating TranscriptDb objects, 312 installing transcript annotation package for mouse, Mus musculus, 309 rtracklayer package, 313-314 anonymous functions, 234 APIs (application programming interfaces), 384 other SAM/BAM APIs, 394 provided by bioinformatics databases, 429 apropos() function (R), 181 arguments, 179, 181 for functions in R, 234 arithmetic operators in awk, 159 using on IRanges objects, 275 vectorization in R, 183 arrange() function (dplyr), 245 arrays 

Bash, 406 in R, 238 as() function, 320 ASCII, 145, 471 base qualities, 344 assert functions, 12 assertions, explicitly stating and testing assumptions about data, 12 assignment in R, 182 vector names in R, 192 associative arrays, 161 attributes, 249 authentication HTTP or FTP, with wget, 110 with Git remotes, 87 with SSH keys, 59 automating tasks, 12 awk, 136, 157-163, 163 (see also bioawk program) associative arrays, 161 cleaning up gene names matched by grep, 145 converting between BED and GTF files, 161 functions, built-in, 162 implementations, 157 pattern-action pairs, 158 performance, grep versus, 140 processing of records, 157 setting field, output field, and record sepa‐ rators, 161 tabular plain-text data processing with, 136 axis labels (ggplot2), 209 

## B

background processes, 50 bacterial artificial chromosomes (BACs), 471 Baggerly, Keith, 7 BAM alignment format, 353, 355 (see also SAM/BAM files) converting between SAM and BAM with samtools view, 365 BAM files BEDTools support for, 331 indexing, 368 using samtools view, 358 bar plots, 216-218 Base Alignment Quality algorithm (samtools), 378, 383 

disabling, 378 base calling, 133, 365, 471 base graphics (R), 207 basename program, 407 using with xargs to apply commands to files, 419 basename() function (R), 259 bases base qualities, 344-346 different quality schemes, 345 inspecting and trimming low-quality bases (example), 346-349 Bash shell, 40, 396-410 automating file processing with find and xargs, 411-421 find's exec, 415 find's expressions, 413 finding files with find, 412 using find and xargs, 411 using xargs to apply commands to files, 418 xargs and parallelization, 419 xargs, Unix powertool, 416 conditionals in a script, if statements, 401-405 processing files using for loops and glob‐ bing, 405-410 Python versus, 396 variables and command arguments, 398-401 command-line arguments, 399 writing and running robust scripts, 396 robust Bash header, 397 running Bash scripts, 398 bc (bench calculator), 372 BCF (Binary Call Format), 380, 472 (see also Variant Call Format) bcftools, 380 bcftools call, 381-383 BED files, 129 converting between BED and GTF files with awk, 161 downloading for use with BEDTools, 329 extracting regions with samtools view, 368 inspecting beginning and end, 130 inspecting with wc, 134 of dbSNP (build 137) variants, 325 random pseudogenes written to, exporting with rtracklayer, 314 working with, using bioawk, 163 

BEDTools, 329-337, 472 computing coverage with genomecov, 335 computing overlaps with intersect, 330-333 getfasta subcommand, 334 other subcommands and pybedtools, 336 slop and flank, 333 subcommands, 329 BEGIN and END patterns (awk), 160 bench calculator (bc), 372 bg (background) command, 51 BGZF compression, 425 BGZF-compressed files, 353 bgzip, compressing files for Tabix, 426 bias-variance trade-off, 218 Binary Call Format (BCF), 380, 472 (see also Variant Call Format) binary numbers, 361 binning data, 215-218 in WHERE clauses, 440 bioawk program, 146, 163-165 counting FASTA/FASTQ entries, 343 creating test data, 333 options for working with tab-delimited files, 164 processing FASTA/FASTQ files, 164 working with a BED file, 163 BiocInstaller package, 270 biocLite() function, 270 Bioconductor packages for data, 309 for working with promoter sequences, extracting motifs, and creating sequence logo plots, 319 for working with SAM/BAM files and align ments, 394 installing and working with, 269 IRanges and GenomicRanges, 269 qrqc, 346 Bioconductor project, 178 biocUpdatePackages() function, 270 biocValid() function, 270 bioinformatics challenges for reproducible and robust research, 5 growth of biological data, 1 learning data skills for, 4 rapid develoment of tools, 3 reasons for using Unix as computing envi ronment, 37 

bioinformatics data (see data) bioinformatics projects, 21-35 Markdown for project notebooks, 31 formatting basics, 31-35 rendering Markdown to HTML with Pandoc, 35 necessity of using Git, 68-69 organizing data to automate file processing, 26 project directories and directory structures, 21 project documentation, 24 using directories to divide a project into subprojects, 26 BioMart, 145, 472 creating TranscriptDb object for annotation data, 312 BioPerl library, 350 Biopython library, 350 Biostrings (BSgenome) packages, 316 BioStrings package, 269 Bitbucket, 86 bitwise flags (SAM/BAM), 359, 360 SAM bitwise flags and SAM fields, 371 using samtools view to filter on, 369 Blocked GNU Zip Format (see BGZF compres‐ sion) Boolean algebra, 188 Bourne-again shell (see Bash shell) brace expansion, 27, 472 versus wildcard matching, 29 branches (Git), 72, 472 working with, 102-108 branches and remotes, 106 creating branches with git branch, 103 how branches help in bioinformatics work, 102 merging branches with git merge, 105 using git checkout, 103 using git log, 105 break statement (R), 253 breakpoints, 236, 472 browser() function (R), 236 BSD utils, 142 Awk, 157 grep, 147 sed, 165 sort, 150 xargs, 419 

BSgenome packages, 269, 316 BSgenome.Mmusculus.UCSC.mm10 package, 316 chromosome names, 317 

## C C

C API (SAMtools), 394 c() function (R), 183, 185 C. elegans reference genome, simulated read of, 356 call stack, 236, 473 capturing groups (in regular expressions), 250, 473 capturing in regular expressions, 166 carriage return and linefeed (\r\n), 129 cat command, 42, 172 zcat (or gzcat) for compressed files, 120 cbind() function (R), 240 CDS, 308 (see also coding sequences) cdsByOverlaps() function, 312 character classes (in reguar expressions), 168, 473 character encodings, 145 ASCII, 344 character ranges, 29 character vectors (R), 189, 191 coercion into factors. 200 creating a factor from, 191 strings as, 248 checksums, 114, 473 comparing for downloaded file, 121 SHA and MD5, 115 chr() function (Python), 344 chromosome names, 264 in Mus_musculus.GRCm38.75_chr1.gtf.gz, 317 chsh command, 40 CIGAR format, 473 CIGAR strings, 360, 363 class() function (R), 219 classes for storing sequence data, Biostrings pack‐ ages, 316 in R, 192, 272 setting colClasses, 195 Cleveland, William S., 215 cloning repositories, 71 closed intervals, 267 

closest (bedtools), 337 code releasing for reproducibility, 16 testing, 13 using as documentation, 17 using existing libraries when possible, 14 writing for humans, 11 coding sequences (CDS), 308 as argument in GenomicFeatures functions, 311 retrieving with GenomicFeatures, 311 coercion, 473 type coercion in R, 190 col.names() function (R), 199 colClasses (R), 195 collaborators, adding to GitHub repository, 87 columns dataframes in R, 199 selecting with dplyr select(), 244 subsetting, 205 finding number in a file, using awk, 136 formatting tabular data with column com mand, 139 selecting in SQLite with SELECT, 435 sorting with sort, 148 working with column data using cut, 138 combining data (split-apply-combine pattern), 240 comma-separated vaules files (see CSV files) command substitution, 54 using to construct Bash arrays, 407 command-line arguments (Bash), 399 commandArgs() function (R), 256 commands limits on number of arguments, 28 spaces separating arguments in Unix, 23 comments commenting code, 11 header block in GTF file, 135 in FASTA files, 341 in FASTQ files, 342 removing a comment header block, 136 commits, 70 comparing with git diff, 100 pulling from a remote repository with git pull, 89 pushing and pulling, 90 pushing to a remote repository with gi push, 88 

seeing commit history with git log, 79 taking a snapshot of your project with git commit, 76 verifying that two repositories have same commits using git log, 89 comparison operators in awk, 159 in R, 188 chaining in dataframe queries, 204 using to build logical vectors, 187 string and integer comparison operators in Bash, 402 complex vectors (R), 190 Comprehensive R Archive Network (CRAN), 177 Bioconductor package system versus, 269 compression BGZF, 425 compressing and working with, 118 compressing and working with compressed data working with gzipped files, 120 CRAM format, 366 working with gzipped compressed files, 196 writing dataframe to gzipped file, 260 concatenation cat command, 42 compressed files, 120 string concatenation in awk, 158 conditioning, 215 conservation tracks, 292 constraints, 456 contigs, 48, 121, 263, 473 control flow statements in R, 253 Control-C, killing a process, 51 Control-z, suspending a process, 51 Coombes, Kevin, 7 coordinate systems, 263, 473 0-based and 1-based. 266 1-based, IRanges and GenomicRanges packages, 271 in common bioinformatics formats, 267 remapping for changes in genome versions, 266 coplots, 215 count() function (SQLite), 442 countOverlaps() function, 289, 327 countQueryHits() function, 288 countSubjectHits() function, 288 

coverage, 292, 474 calculating for GRanges objects, 327 computing with bedtools genomecov, 335 defining ranges corresponding to highcoverage peaks, 296 coverage() function, 294, 307 cp (copy) command, 114 CRAM files, 366, 474 CRAN (Comprehensive R Archive Network), 177 Bioconductor package system versus, 269 CREATE INDEX statement, 458 CREATE statement, 432 CREATE TABLE statement, 455 CrossMap tool, 266 csh (C shell), 40 CSV (comma-separated values) files, 129 Dataset_S1.txt, 195 reading with read.csv() function in R, 196 curl program, 112 cut program cleaning up gene names matched by grep, 145 working with column data, 138 cut() function (R), 215-218 

## D

data, 109-122 challenges in genomics datasets, 109 compressing and working with gzip and bzip2, 119 gzipped files, 120 compressing and working with compressed data, 118 differences between data, examining, 116 downloading reproducibly, case study, 120-122 formatting for computer readability, 11 in project directory, documenting origin of, 24 integrity of, 114 checking with SHA and MD5 check‐ sums, 115 letting data prove it's high quality, 15 releasing for reproducibility, 16 retrieving bioinformatics data, 110 downloading data with curl, 112 downloading data with wget, 110 using rsync and secure copy (SCP), 113 

treating as read-only, 14 data skills learning to learn bioinformatics, 4 recommendations for further learning, 468 data types Bash variables and, 399 columns of heterogeneous types of vectors in dataframes, 199 difference between object type and class in R, 192 getting type of any object in R, 191 homogenous data type and type coercion in R vectors, 190 in SQLite, 433, 456 lists in R, 228 vectors in R, 228 data.frame() function (R), 200 database connections, using dplyr functions with, 247 database dumps, 465 databases deleting in SQLite, 458 version information, documenting, 25 dataframes (R) DataFrame object, 303 exploring and transforming, 199-203 exporting to plain-text files, 260 subsetting, 203-207 working with, ggplot2, 208 writing to a gzipped file, 260 Dataset_S1.txt, 194 date command, 54 dates, SQLite versus other relational databases, 443 dbsnp137 object, 325 debugging R code, 236-237 decimal numbers, 362 declarative languages, 430, 474 degenerate (or ambiguous) nucleotide codes, 343 densities, plotting, 212, 213 dependencies (in programs), 474 dependencies (project), storing in Git reposi‐ tory, 83 depth of coverage (see coverage) /dev/null, 44 difference (set operation), 280 difference between ranges representing a chromosome, 321 

getting between transcripts range and exons ranges, 322 differences in a file viewing with git diff, 77 viewing with Unix diff, 116 zdiff for compressed files, 120 dim() function (R), 199 directories bioinformatic project, 21 using to divide a project into subprojets, 26 discretization (see binning data) distance() function, 291 distanceToNearest() function, 291 DISTINCT keyword, 443 distributed version control system (Git), 84 divergence, 194 do.call() function (R), 241, 258 document converter (Pandoc), 35 documentation BEDTools, 332 Bioconductor packages, 270 bioinformatics project, 24 documenting everything for reproducibility, 16 downloaded data from Ensembl, 122 Git as essential part of, 69 in R, 180 SAM format specification, 356 using code as, 17 DOS-style line separator (\r\n), 129 dot commands in SQLite, 432 double vectors, 189 downstream flanking (ranges), 277 dplyr package, 243-248 adding new columns to dataframe with mutate() function, 245 basic functions for manipulating data‐ frames, 243 chaining operations, 246 filter() function, 244 grouping and summarizing data, 246 additional operations chained to, 24 covenience functions for summaries, 24 select() function, 244 sorting columns using arrange() function, 245 tbl_df class wrapping dataframes, 243 using pipes, 246 DROP INDEX statement, 458 

DROP TABLE statement, 458 Duke Saga, 7 

## E

echo command, 171 creating test data for bedtools slop, 333 using in command substitution, 54 using to check ? shell variable, 52 EDA (see exploratory data analysis) elementLengths() function, 307 encodings, file, 145 END pattern (awk), 160 end position, 264 (see also start and end position) end position (ranges) extending, 273 Ensembl, 121, 309 no inconsistent naming issues with, 251 Ensembl/NCBI chromosome name style, 318 environment variables, 401, 474 environments (R), 182 errors bug-prone nature of scientific coding, 13 exit status and, 52 in bioinformatics research findings, 6 silent errors in scientific computing, 8 example() function (R), 181 exit status. 52, 474 checking for rsync, 114 commands in Bash, 401 negating for Bash scripts, 402 test command in Bash, 402 exons retrieving with GenomicFeatures, 311 set difference between transcripts range and exons' ranges, 322 exonsBy() function, 322 exonsByOverlaps() function, 312 experimental design, 10 exploratory data analysis (EDA), 175, 474 exporting data from R, 260 expressions, evaluation in R, 178 

## F

facets (ggplot2), 224 facet_grid() function, 226 facet_wrap() function, 225 factor() function (R), 191 factors (R), 191 

and loading data into R, 197 as integer vectors, 192 coercion of column of strings into factor, 200 columns in a dataset, 219 levels, 192 FASTA/FASTO files FASTA format, 339 downside of, 340 FASTQ format, 341 displaying and inspecting a file with less, 132 inspecting and trimming low-quality bases, 347 pitfalls of, 342 quality schemes, 345 indexed FASTA files, 352 ins and outs of counting entries, 342 nucleotide (and protein) sequences in, 339 parsing example, counting nucleotides, 349 processing with bioawk, 164 using bedtools getfasta with FASTA file, 334 FastQC program, 346 fg (foreground) command, 50 fields, 128 counting in awk, 136 in awk, 157 setting variables for fields in bioawk, 163 file descriptors, 44 file formats BEDTools for, 329 bioinformatics, using bioawk with, 163 CSV (comma-separated values), 129 importing and exporting range data with rtracklayer, 313 SAM and BAM for alignment data, 355 space-delimited, 129 tabular plain-text data, 128 file program, 146 files and directories differences in files, viewing with git diff, 77 finding file size with ls -l command, 135 finding files with find, 412 ignoring files in Git, 81 naming, 23 naming scheme, consistent, aiding file pro‐ cessing, 28 organizing data to automate processing tasks, 26 

paths, 23 processing files in Bash using for loops and globbing, 405-410 redirecting standard out to file, 41 test expressions in Bash, 403 filter() function (dplyr), 244 find program exec option, 415 expressions, 413 finding files with, 412 playing safe with, 417 using, 411 using with xargs, 416 findOverlaps() function, 282, 307 select parameter, 285 type argument, 284 using with GRanges objects, 325 using with IntervalTree objects, 287 FLAG (bitwise flag), 359, 360 flank (bedtools), 333 flank() function, 277, 307, 315 flanking ranges, 277, 315 extracting with bedtools flank, 333 ranges flanked by 7 elements, upstream and downstream, 277 flat file formats, 428 follow() function, 290 for loops (Bash), 408 xargs versus, 419 for loops (R), 232, 253 foreign key, 450, 474 fork, or forking, 474 forking repositories, 97 pull requests, 97 fread() function (R), 196 FTP, 475 downloading files via, with curl, 112 downloading files via, with wget, 110 full outer joins, 454 function calls (R), 242 functions (R), 179 applying to data, 231 list apply functions, sapply() and map‐ ply(), 237 other apply functions for other data structures, 238 using lapply(), 231-234, 239 documentation, 180 mathematical. 179 

object class and, 192 polymorphism, 193 writing, 234-236 defining functions, 234 scoping, 235 functions (SQLite), 441 aggregate functions, 442 

## G

GAM (generalized additive models), 214 gaps between ranges, 279 gaps() function, 279, 319 garbage in, garbage out, 9 Gawk (GNU awk), 157 GC content, 194, 263, 475 GENCODE website. 315 Gene Feature Format (GFF) files, 313 (see also GTF files) Gene Transfer Format files (see GTF files) generalized attitive models (GAM), 214 generator functions, 350 generic functions (R), 193 generic ranges basic range operations on, 275 storing with IRanges, 270 genes() function, 310 genome browsers rtracklayer interfacing with, 314 UCSC Genome Browser, 110, 251 Genome Reference Consortium (GRC), 121 genome version, ranges linked to, 265 Genome-Wide Association Studies (GWAS), 431 genomecov (bedtools), 335 genomes BSgenome packages, 316 reference genome versions, 265 genomic intervals (see genomic ranges; range data) genomic ranges, 265-269, 475 coordinate systems, 266 gaps, 319 range types of common bioinformatics formats, 267 ranges on an imaginary stretch of chromo‐ some (example), 265 reference genome versions, 265 storing with GenomicRanges, 299 strands and, 268 

GenomicAlignments package, 325 GenomicFeatures package, 269, 308-313 creating TranscriptDb object for annotation data, 312 finding overlaps, methods for, 311 TranscriptDb object, creating and workin with, 308 TranscrriptDb object, creating and working with, 311 GenomicRanges package, 221, 264, 269 calculating coverage for GRanges objects, 327 finding and working with overlapping ranges, 324-327 getting intergenic and intronic regions, 319 reference manual and vignettes, 270 retrieving promoter regions with flank() and promoters(), 314 retrieving promoter sequence, 315 storing genomic ranges, 299 geometric objects (geoms) in ggplot2, 209 geom_density() function, 212 geom_point() function, 208 geom_smooth() function, 225 specifying smoothing method, 214 getOption() function (R), 180 getwd() function (R), 194 GFF files, 313 (see also GTF files) ggplot() function, 208 ggplot2, 207-215 axis labels, plot titles, and scales, 209 bar plots, 216-218 default behaviors, 211 documentation and additional resources, 207 downloading and installing, 177 facets, using, 224-228 grammar of graphics, 208 smoothing, 213-215 themes, 210 Git, 67-108 BedTools, comparing to, 329 collaborating with, using remotes, git push and git pull, 83 authenticating with Git remotes, 87 connecting with git remote, 87 creating a shared central repository, 86 merge conflicts, 92-97 

more GitHub workflows, forking and pull requests, 97 

practicing pushing and pulling, 90 

pulling commits from remote repository with git pull, 89 

pushing commits to remote repository with git push, 88 

creating repositories with git init and git clone, 70 

further education in, 108 

installing, 70 

moving and removing files with git mv and git rm, 80 

necessity of, in bioinformatics projects, 68 keeping snapshots of the project, 68 keepng software available and organized, 69 tracking important changes to code, 69 

seeing commit history with git log, 79 

staging files using git add and git status, 73 

taking a snapshot of your project with git commit, 76 

telling Git what to ignore with .gitignore file, 81 

telling Git who you are, 70 

tracking files using git status and git add, 72 

undoing a stage using git reset, 83 

viewing file differences with git diff, 77 

working with branches, 102-108 branches and remotes, 106 creating branches with git branch, 103 merging branches with git merge, 105 using git checkout, 103 

working with past commits, 97 getting files with git checkout, 97 more git diff, comparing commits and files, 100 storing changes with git stash, 99 undoing and editing commits with git commit --amend, 102 

git fetch command, 107 

GitHub creating a shared central repository with, 86 Dataset_S1.txt, 194 

global environment (R), 182 

global option (R), 180 

globbing, 28, 410, 475 

glue language, Bash as, 396 

GNU Awk (Gawk), 157 

GNU coreutils, 142 sed, 165 sort, 150 xargs, 419 

GNU Parallel, 421 

Golden Rule of Bioinformatics, 9 stopping execution or issuing warnings when running code, 252 

GRanges object, 299 accessing all IRanges with ranges(), 301 accessors for start and end positions and width, 301 calculating coverage for, 327 containing data returned by GenomicFea tures, 311 findOverlaps() method, 325 gaps() function, 319 representing introns of transcripts, 321 seqnames() and strand() methods, 301 using set operations on, 320 using split() to create GRangesList, 306 

GRangesList object, 303 

accessing elements with [] and [[]] opera‐ tors, 304 

applying split-apply-combine pattern to, 307 

created by split(), 322 

creating by using split() on GRanges, 306 

grouping exons and traanscripts by tran‐ script, 323 

methods applied to, working at groupeddata level, 308 

graphics systems in R, 207 

GRC (Genome Reference Consortium), 121 

greedy matching (in regular expressions), 168 grep, 140-145 

--color=auto option, 141 

-o option, extracting only matching part of a line, 144 

basic usage, pattern and file to search for it, 141 

context for matches, 143 

counting FASTA/FASTQ entries, 342 

counting matching lines for a pattern, 144 

finding non-ASCII characters in a file, 147 

gene names matched, cleaning up, 145 

in Bash if statement, 402 

matching lines in a comment header pat‐ tern, 137 

output of, inserting into another command, 54 piping standard out to head, 131 regular expressions support POSIX BREs, 143 POSIX EREs, 143 searching for whitespace in a file, 136 speed of, versus sed, awk, and custom Python script, 140 unintentional matching caused by partial matching (-v option), 142 using with pipes, 47 using with sort | uniq, 153 zgrep for compressed files, 120 grep() function (R), 248 GROUP BY statement, 444 grouping in regular expressions, 166 GTF files, 129, 138 converting between BED and GTF files with awk, 161 creating TranscriptDb object for annotation data, 313 importing and exporting range data with rtracklayer, 313 inspecting using grep output pipelined to head, 131 inspecting with head, 135 inspecting with wc, 135 random pseudogenes written to, exporting from rtracklayer, 313 guanine-cytosine content (see GC content) gunzip, 119 gzfile() function (R), 260 gzip, 118 working with gzipped compressed files, 120 

## H

half-closed, half-open intervals, 266 hangup signals, 51, 61 hard clipping, 363 hard masked, 343 HEAD (in Git), 83 head program, 129 finding number of columns in a file, 136 inspecting a GTF file, 135 looking at data from a Unix pipeline, 131 head() function (R), 199, 221 help() function (R), 180 help.search() function (R), 181 

hexadecimal numbers, 362 hexdump program, 146 Hidden Markov Model, 383 Hits object, 284, 326 extracting information from overlapping ranges, 287 overlapping ranges created from, using ranges(), 288 returned by distanceToNearest(), 292 Homebrew, using to install Git, 70 hostname command, 58 HTML, 31, 255 conversion to PDF, 35 rendering Markdown to, using Pandoc, 35 HTTP, 475 downloading files via, with curl, 112 downloading files via, with wget, 110 

## I

identifiers in FASTA descriptions, 340 if statements (Bash), 401-405 if statements (R), 253 ifelse() function (R), 254 ignoring files (.gitignore), 81 IGV (see Integrated Genomics Viewer) Illumina GGC sequences generating errors, 377 quality scheme (base qualities), 345 TruSeq kit, 133 implicit assumptions, 12 import() function (rtracklayer), 313 in operator (R), 219 inconsistent naming in bioinformatics, 251 index (Git), 83 indexed (or indexing), 475 indexing 0-based in Python, 266 1-based in R, 267 BAM files, 368 databases, 457 dataframes in R, 201 FASTA files, 352 faster access to indexed files. 354 lists in R, 229 using Tabix, 427 vectors in R, 184 excluding elements from lists with nega tive indexes, 186 index rule, 187 

out-of-range indexing, 186 using indexes to reorder elements, 186 vectorized indexes, 185 with logical vectors, 187 .indices command (SQLite), 458 -Inf and +Inf values (R), 190 infinite values in R, 190 "Influence of Recombination on Human Genetic Diversity, The", 193 inner joins, 223, 451, 475 INSERT statement, 457 inspecting and manipulating text data install.packages() function (R), 177, 207 integer vectors (R), 189 IntegerList object, 305 integers comparison operators in Bash, 402 creating integer sequences in R, 183 Integrated Genomics Viewer (IGV), 372, 373-377 intergenic regions, 319 Internal Field Separator (IFS), 407 International Union of Pure and Applied Chemistry (IUPAC), nucleotide codes, 343 intersect (bedtools), computing overlaps 330-333 intersection, 280 interval trees, 286, 475 IntervalTree object, 287 introns created manually and by function, compar ing, 324 creating Granges objects representing, 321 getting using range set operations, 322 intronsByTranscripts() function, 321 IP addresses, using with SSH, 58 IRanges object, 271 basic range operations on, 275 arithmetic operations, 275 flanking ranges, 277 gaps(), 279 reduce(), 278 restricting ranges within a bound, 276 set operations, 280 before and after extending end position, 274 gaps, 319 getting start, end, and width positions, 273 subsetting, 274 using Rle object with, 295 

IRanges package, 269 finding nearest ranges and calculating dis‐ tance, 290 finding overlapping ranges, 281 IntervalTree object, 287 using qry and sbj with findOverlaps(), 283 GenomicRanges and, 299 storing generic ranges with, 270 IRanges() constructor, 272 is.finite() and is.infinite() functions (R), 190 is.list() function (R), 230 ISO 8601 format, 443 IUPAC nucleotide codes, 343 

## J

jaccard (bedtools), 337 Java FastQC program, 346 Integrated Genome Viewer (IGV), 373 Picard API, 394 jobs program, 50 join predicate, 452 join program, 155 -a option, 156 example, joining data files by a common column, 155 joins, 448 combining datasets in R, 219 inner join, 223, 451 left outer join, 223, 453 performed in R with merge(), 224 

## K

Kdiff (merge tool), 97 kill command, 52 killing processes, 51 knitr package, 255 Knuth, Donald, 125 ksh (Korn shell), 40 

## L

Lab Information Management System (LIMS), downloading data from, 110 lapply() function (R), 231-234 applying per-file summary function to data, 259 parallelizing, 233 

using in split-apply-combine pattern, 239 using with GRangesList, 307 lattice package (R), 207 layers, adding to plot with ggplot2, 209 leading zeros, using in filenames, 30 left outer joins, 223, 453 length() function (R), 183 GRanges object, 302 less program, 79, 131 commonly used commands, 132 debugging pipelines, 133 piping zgrep output to, 170 searching text and highlighting matches, 133 zless for compressed files, 120 levels (factors in R), 192 levels() function (R), 192, 219 lexical scoping, 235 Li, Heng, 163, 350 library() function (R), 181, 207 LiftOver tool, 266 LIMIT clause, 435 LIMS (Lab Information Management System), downloading information from, 110 line separators, 129 list() function (R), 229 list.files() function (R), 257 lists GRangesList object, 303 in R. 228-234 accessing elements, 229 creating or changing elements, 230 examining with str() function, 229 list apply functions, sapply() and map ply(), 237 vectors versus, 228 writing and applying functions to lists with lapply(), 231-234 literate programming, 126 load() function (R), 260 loading and combining multiple files in R, workflows for, 257-260 Logic of Scientific Discovery, The, 6 logical operators chaining commands in Bash, 402 in awk, 159 in R, 188 logical vectors (R), 187, 189 using to subset dataframes, 204 

## M

long format (tabular data), 198 lossy compression, 366 low-complexity sequences, 377 ls -l command, 43, 135 ls() function (R), 182 

magrittr package, pipes, 246 make tool and makefiles, 421-423 Map() function (R), 259 mapped reads, 371 mapping qualities, 360, 365 mapply() function (R), 238, 259 MAPQ (mapping quality), 360 Markdown formtting basics, 31-35 rendering to HTML, using Pandoc, 35 Rmarkdown package, 255 using for project notebooks, 31 master branch (Git), 72, 88 match() function (R), 220 matching GRangesList objects, 322 matching vectors in R, 219 match() function, 223 mathematical functions (R), 179 vectorization, 184 matrices (R), 238 coercing Hits objects into, 288 McIlroy, Malcolm Douglas (Doug), 125 on Unix pipelines, 127 mclapply() function (R), 233 mcols() function, 303 MD tag (SAM/BAM), 364 MD5 checksums, 115 md5sum (or md5) program, 116 mean() function (R) applying using lapply(), 239 calling from lapply(), 233 writing version with na.rm=TRUE, 234 Meld (merge tool), 97 merge (bedtools), 337 merge conflicts (in Git), 85, 92-97 in branch merges, 106 merge sort, 151 merging data in R, 220 considering structure of datasets, 221 creating example datasets, 221 merge() function, 224 using paste() function, 222 

validating, 223 validating keys' overlap, 222 message() function (R), 252 messages (git commit), 76 metadata accessing for GRanges object, 303 attached to GRanges objects, 300 documenting for project data, 24 importance in reproducibility, 7 in SAM/BAM files, 358 modularity (Unix), 38 more program, 79, 132 mouse (see Mus musculus) moving files with git mv, 80 mpileup (samtools), 378 multicov (bedtools), 337 multiinter (bedtools), 337 Mus musculus (mouse) BSgenome.Mmusculus.UCSC.mm10 pack‐ age, 316 genome version mm10 files, use with BED‐ Tools, 329 Mus_musculus.GRCm38.75.dna.chromo‐ some.8.fa.gz file, 352 Mus_musculus.GRCm38.75.dna_rm.tople‐ vel_chr1.fa file, 334 Mus_musculus.GRCm38.75_chr1.gtf.gz file, 313, 333 TxDb.Mmusculus.UCSC.mm10.ensGene package, 309 mutate() function (dplyr), 245 

## N

\n (newline) character, 129 NA value (R), 190, 223 named pipes, 171 names() function (R), 185, 192 GRanges object, 302 using with lists, 231 naming scheme FASTA descriptions and identifiers, 341 FASTQ descriptions, 342 no chromosome naming scheme, 317 transcript annotation packages, 309 NaN (not a number) values in R, 190 National Human Genome Research Institute, 431 NCBI Genome Remapping Service, 266 nchar() function (R), 248 

ncol() function (R), 199 nearest ranges, family of functions to find, 291 nearest() function, 290 negative and positive infinite values in R, 190 negative indexes, 186 next statement (R), 253 NM and MD tags (SAM/BAM), 364 nohup command, 61 non-greedy matching (in regular expressions), 168 NR variable (awk), 160 nrow() function (R), 199 nucleotide codes, 343 nucleotide diversity, 194, 203 nucleotide sequences, 263 nucleotides, counting, FASTA/FASTQ parsing example, 349-352 NULL value in R, 190 assigning to list elements, 230 numeric ranges, 29 numeric vectors (R), 189, 191 

## O

object-oriented systems (R), 193 objects dcoumentation for R objects, 181 discerning difference between object type and class in R, 192 list elements in R, 228 saving and loading in R, 260 Open Bioinformatics Foundation (OBF), FASTQ quality schemes, 345 operators common find expressions and operators, 413 commonly used in WHERE statements, 438 options() function (R), 180 options(error=NULL) setting (R), 237 options(error=recover) setting (R), 237 or operator (||), 52 ord() function (Python), 344 ORDER BY clause, 436, 444 order() function (R), 186 organism() function, 316 origin (Git remotes), 88 out-of-memory approaches, 425-465 relational databases and SQLite, 428-465 dropping tables and deleting databases, 458 

dumping databases, 465 exploring SQLite databases with the CLI, 431-434 installing SQLite, 431 interacting with SQLite from Python, 459 organizing relational databases and joins, 448-455 querying out data with SELECT, 434-440 SQLite aggregate functions, 442 SQLite functions, 441 subqueries, 447 when to use relational databases in bio‐ informatics, 429 writing to databases, 455-458 Tabix and BGZF, fast access to indexed tabdelimited files, 425-428 compressing files for Tabix with bgzip, 426 indexing files with Tabix, 427 using Tabix, 427 outer joins, 453 overlaps computing with bedtools intersect, 330-333 finding and working with overlapping ranges, 324-327 finding overlapping ranges, 281 overlaps used to quantify something, 286 GenomicFeatures methods for finding, 311 operation functions, GRangesList and, 307 overplotting alleviating with smoothing, 213 alleviating with transparency, 211 oversmoothing, 218 

## P

pagers, 79, 131 pairwise processing, transcript and its exons, 323 pairwise set operations on ranges, 281 Pandoc, 35 panes (Tmux windows), 64 paradox of scientific coding, 13 parallelization BEDTools operations, 330 parallelizing lapply() function in R, 233 xargs and, 419 parent directories, 23 

parsers FASTA/FASTO, 349 writing, 252 pasillaBamSubset package, 347 passwords, authentication without, using SSH keys, 60 paste() function (R), 222, 252 patch files, 118 patch program, 118 paths, stripping from filenames with basename, 407 pattern-action pairs (awk), 158 patterns finding and replacing in sed, 166 in awk, 158 searching for, in R character vectors, 248 PDF files, 35, 255 Perl Compatible Regular Expressions (PCRE), 248 PHRED quality scores, 345 PID (process ID), 50 pileup format, 378-379 Pysam pileups, 394 piped versus sequential commands, 169 pipeline approach (Unix) debugging pipelines with less, 133 historical example of use, 125 when and how to use, 127 pipelines, 395 using in Bash if condition statements, 402 using make and makefiles, 421-423 pipes, 38, 45-50 combining with redirection, 48 using tee program, 49 creating a simple program, using grep and pipes, 47 importance to bioinformatics, 173 magrittr package, use by dplyr, 246 named, 171 piping results of samtools commands to other programs, 359 xargs and, 420 plain-text files decoding, 145 soring plain-text data with sort, 147 Platypus, creating TranscriptDb object for, 313 plot titles (ggplot2), 209 PNEXT, 360 polymorphic functions, 193 

Popper, Karl, 6 population genetics statistics, 194 POS (position), 360 POSIX Basic Regular Expressions (BREs) support by grep, 143 support by sed, 166 POSIX Extended Regular Expressions (EREs) support by grep, 143 support by sed, 166 use by grep() function in R, 248 precede() function, 290 primary keys, 450, 456, 457 print() function (R), 179 private key (SSH), 60 process ID (PID), 50 process substitution, 172 processes, 50-53 managing from Unix shell background processes, 50 exit status, 52 killing processes, 51 project directory, 22 promoters retrieving promoter regions with Genomi cRanges, 314 retrieving promoter sequence with Genomi‐ cRanges, 315 retrieving with GenomicFeatures, 311 using bedtools flank to extract promoter regions for genes, 333 promoters() function, 315 provider() package, 316 providerVersion() function, 316 psetdiff() function, 323 pseudodevice, redirecting unwanted output to, 44 public key (SSH), 59 pull requests (GitHub), 97 Pysam API, creating SAM/BAM processing tools, 384-394 additional Pysam features, 394 extracting SAM/BAM header information from AlignmentFile, 387 opening BAM files, fetching alignments from a region, and iterating across reads, 384 working with AlignedSegment objects, 388 writing a program to record alignment sta tistics, 391-394 

Python, 127, 467 0-based indexing of strings and lists, 266 awk versus, 162 Bash versus, for writing pipelines, 396 converting between character and ASCII, 344 gzip module, 118 interacting with SQLite from, 459 connecting to SQLite and creating tables, 459 loading data into a table, 461 performance of a custom script versus grep, 140 pybedtools, 337 readfq implementation, 350 string processing functions, R language ver‐ sus, 248 tuple unpacking, 351 unit testing code, 13 

## Q

QNAME (query name), 359 qrqc package, 346 exploring base qualities before and after trimming, 347 QTL (qualitative trait loci) studies, 312 QUAL (base quality), 360 query ranges (qry), 283 and select parameter of findOverlaps(), 285 mapping between sbj ranges and, represent‐ ing overlap, 283 using countOverlaps() and subsetByOver‐ laps() with, 289 working with many, and counting overlaps, 286 queryHits() function, 284 quotation marks in R, 181 

## R

R language, 127, 175-261, 467 1-based indexing for vectors and strings, 267 basics, 178 calculations, 178 calling functions, 179 getting help, 180 variables and assignment, 182 vectors, vectorization, and indexing, 183-193 

binning data with cut() and bar plots with ggplot2, 215-218 

bioinformatics packages from Bioconduc tor, 269 

Comprehensive R Archive Network (CRAN), 177 

debugging code, 236-237 

developing workflows with R scripts, 253-261 control flow, if, for, and while, 253 exporting data, 260 loading and combining multiple files, 257 working with R scripts, 254 

exploring and transforming dataframes, 199-203 

exploring data by subsetting dataframes, 203-207 

exploring data visually with ggplot2 scatterplots and densities, 207-213 smoothing, 213-215 

exploring dataframes with dplyr, 243-248 

futher directions and resources, 261 

getting started with R and RStudio, 176 

installing R, 178 

lists list apply functions, sapply() and map‐ ply(), 237 

loading data into R, 194-198 getting data into shape, 198 inspecting file at command line before loading, 195 large genomics data, 195 working directory, 194 

merging and combining data, 219-224 

more data structures, lists, 228-234 

packages for data, 309 

RSQLite package, 464 

split-apply-combine pattern in data analysis, 239-243 

stopifnot function, 12 

visualizing grouped data with ggplot2 facets, 224-228 

working with data, 193 

working with ranges, 289 

working with strings, 248-253 

writing functions, 234-236 

range data, 263-337 

basic range operations on IRanges object, 275 

calculating coverage for GRanges objects, 327 

finding and working with overlapping ranges, 324-327 

finding nearest ranges and calculating dis‐ tance, 290 

finding overlapping ranges, 281 intricacies of overlap operations, 286 

genomic ranges and coordinate systems, 264-269 

getting intergenic and intronic regions, 319 

installing and working with Bioconductor packages, 269 

ranges, 263, 264 

run-length encoding and views, 292-299 creating ranges from run-length enco‐ ded sequences, 296 ranges and their coverage, 294 views, 297 

storing generic ranges with IRanges, 270 

storing genomic ranges with Genomi‐ cRanges, 299 grouping data with GRangesList, 303 

working with annotation data, using GenomicFeatures and rtracklayer, 308 

working with on command line, using BED‐ Tools, 329-337 computing overlaps with intersect, 330-333 genomecov, 335 other subcommands and pybedtools, 336 slop and flank, 333 

range width, 267 accessing for GRanges object, 301 getting for IRanges object, 273 

ranges() function using with GRanges, 301 using with Hits object, 288 

ranges, handling in Unix shell, 29 

raw vectors (R), 190 

rbind() function (R), 240, 258 

read groups (SAM/BAM files), 358 

read.csv() function (R), 200 coercion of column of strings into factor, 197 commonly used arguments, 196 

loading large data files more quickly, 195 returning results as data.frame, 200 read.delim() function (R), 260 coercion of column of strings into factor, 197 commonly used arguments, 196 loading large data files more quickly, 195 returning results as data.frame, 200 read.table() function (R), 196 readfq parser, 350 README files, 16 project documentation in, 25 recombination, 194 records, 128 NR variable in awk, 160 processing with awk, 157 recursive downloading (wget), 111 redirection, 38 combining with pipes, 48 using tee program, 49 redirecting standard error, 43 redirecting standard out to file, 41 using standard input redirection, 45 xargs, pipes, and redirects, 420 reduce() function, 278, 321 calling on GRangesList, 307 extracting and collapsing overlapping exon with, 325 ranges collapsed into non-overlapping ranges with, 278 reference genomes, 265 reference-based compression scheme, 366 regexpr() function (R), 248, 249 regular expressions capturing groups, 250 capturing text between delimiters in sed, 168 in R, 248 help on, 249 list.files() function, 257 use by sub() function, 251 POSIX Basic Regular Expressions (BREs), support by grep, 143 POSIX Extended Regular Expressions (ERE), support by grep, 143 support in sed, 166 with grep, 47 relational database management system (RDBMS), 428, 477 

relational databases, 428, 477 organizing, 448 when to use in bioinformatics, 429 relative paths, 23, 255 remote branches (Git), 106 remote machines, working with, 57-65 connecting via SSH, 57 maintaining long-running jobs with nohup and tmux, 61-65 quick authentication with SSH keys, 59 remote repositories (Git), 84 removing files with git rm, 80 RepeatMasker, 343 repositories (Git), 70, 82 creating with git clone, 71 creating with git init, 70 reproducible research adopting reproducible practices, 9 case study, Duke Saga, 7 definition in bioinformatics, 6 downloading files reproducibly, case study, 120-122 environment variables and, 401 new challenges for, 5 recommendations for, 16-17 documenting everything, 16 making figures and statistics result from scripts, 17 releasing your code and data, 16 using code as documentation, 17 Unix one-liners and pipelines, document ing, 128 using knitr and Rmarkdown packages, 254 versions of R and R packages, 256 reshape2 package (R), 198 restrict() function, 276 return value, 179, 180 for functions in R, 234 reverse complement, 268 right outer joins, 224, 454 emulating in SQLite, 454 Rle object, 293 subsetting, using IRanges object, 295 Rle() function, 293 RleList object, 305 Rmarkdown package, 255 RNA-seq quantification tools, 325 RNA-seq studies, 263, 282 overlaps and, 286, 324 

RNAME (reference name), 360 RNEXT and PNEXT, 360 rngs_cov object, 296 robust, 477 robust research, 8 adopting robust practices, 9 challenges for, 8 checking how programs change our data, 349 consistent file naming in, 30 Golden Rule of Bioinformatics, 9 recommendations for, 10-16 developing frequently used scripts into tools, 15 experimental design, 10 letting data prove it's high quality, 15 letting the computer do the work, 12 making assertions and being loud, 12 testing code, 13 treating data as read-only, 14 using existing libraries when possible, 14 writing code for humans, data for com‐ puters, 11 using Unix data tools, 137 round() function (R), 180 row.names() function (R), 199, 200 rows dataframes in R, 199 selecting specific rows with deply func tion filter(), 244 subsetting, 206 filtering in SQLite with SELECT WHERE, 437 grouping in SQLite with GROUP BY, 444 ordering in SQLite with ORDER BY, 436 RSQLite package, 196, 464 RStudio, 176 (see also R language) installing, 178 sending a whole function definition at once, 235 rsync program, 113 rtracklayer package, 269, 308, 313-314 export methods for range data saved to common formats, 313 import() function, 313 run-length encoding, 293 calculating coverage reads, 328 coverage() function and, 294 

## S

operations supported by Rle objects, 293 runLengths() function, 294 runs, 293 runValues() function, 294 \r\n (carriage return and linefeed), 129 

S3 object-oriented system (R), 193 SAM alignment format, 353, 355 specification, 356 SAM/BAM files, 355-365 bitwise flags, 360 CIGAR strings, 363 command-line tools for working with align‐ ments in SAM, 365 creating processing tools with Pysam, 384-394 mapping qualities, 365 SAM alignment section, 359 SAM header, 356 sequence matches and mismatches and NM and MD tags, 364 sample() function (R), 187, 328 SAMtools, 358, 365 extracting and filtering alignments with samtools view, 368-372 pileups with samtools pileup, variant calling, and Base Alignment Quality, 378-384 samtools calmd, 364 samtools faidx, 353 samtools flag, 361 samtools flagstat, 393 samtools mpileup, 368, 378 samtools tview, 372 samtools view, 358 sort and index, 367 subcommands implemented by Pysam, 394 support for CRAM file format, 366 using samtools view to convert between SAM and BAM, 365 Sanger quality scheme, 345 Sanger sequencing, 477 sapply() function (R), 237, 240 using with GRangesList, 307 save() function (R), 260 save.image() function (R), 261 savehistory command (R), 177 savehistory() function (R), 261 scale, x and y axes in ggplot2, 209 

facet_wrap() and facet_grid() functions, 227 scatterplot of nucleotide diversity, creating with ggplot2, 208-212 schema, 432, 477 scientific coding, paradox of, 13 scoping, 235 SCP (secure copy), 112 scp (secure copy) command, 114 scripts frequently used, developing into tools, 15 making figures and statistics the results of, 17 making Python scripts executable, 352 working with R scripts, 254 executing scripts from command line, 256 executing scripts with source(), 255 retrieving command-line argumens passed to scripts, 256 search() function (R), 182 secure shell (see SSH) sed, 165-169 cleaning up gene names matched by grep, 145 editing chroms.txt file (example), 165 GNU versus BSD sed, 165 performance, grep versus, 140 piping subshell command output to, 170 substitutions, using pattern matching and regular expressions, 166 using regular expressions to capture text between delimiters, 168 SELECT statement, 434 filtering rows with WHERE clause, 437 limiting results with LIMIT, 435 ordering rows with ORDER BY, 436 selecting columns, 435 select() function (dplyr), 244 separators Internal Field Separator (IFS), 407 setting in awk, 161 setting in bioawk, 164 seq command, 130 SEQ field, 360 seqencing depth, 194 seqinfo() function, 316 seqlevels() function, 318 seqlevelsStyle() function, 318 seqnames() function, 301 

Seqtk (SEQuence ToolKit), 71, 346 seqtk trimfq, inspecting and trimming low quality bases, 347 Sequence Alignment/Mapping (see SAM align ment format) sequence data, 339-354 base qualities, 344 counting FASTA/FASTQ entries, 342 counting nucleotides, FASTA/FASTQ pars ing example, 349 FASTA format, 339 FASTQ format, 341 indexed FASTA files, 352 inspecting and trimming low-quality base (example), 346 nucleotide codes, 343 sequence lengths, table of, creating using bio awk on a FASTA file, 164 sequence name, 264 sequencing depth relationship of GC content to, 215 relationship with SNPs, 214 sequential command execution with ;, 53 sequential versus piped commands, 169 seq_along() function (R), 254 serialization, 260, 477 sessionInfo() function (R), 256 sessions (Tmux), 62 set operations creating gaps using range operations, 320 on ranges, 280 pairwise set operations, 281 using range set operations to get intronic regions, 321 setdiff() function. 321 setwd() function (R), 194 scripts and, 255 SFTP (secure FTP), 112 SHA-1 checksums, 79 checking data integrity with shasum, 115 shared central repository, collaborating over, 84 shebang, 397, 477 shell expansion, tips on, 27 shell scripting, basic Bash scripting, 396-410 shell wildcards (see wildcards) short-circuit evaluation, 405 short-circuiting operators, 52 sickle program, 346 

inspecting and trimming low-quality bases (example), 347 SIGHUP signals, 51, 61 SIGINT signals, 131 significant digits (R), 179 SIGPIPE signals, 131 silent errors, 8 implicit assumptions leading to, 12 single nucleotide polymorphisms (SNPs), 203 relationship with depth of sequencing, 214 slice() function, 296 slop (bedtools), 333 smoothing (ggplot2), 213-215 bin widths and, 218 snapshots of your project, keeping with Git, 68 SNPs (see single nucleotide polymorphisms) soft and hard clipping, 363 soft masked, 343 software regression, 68 Solexa quality scheme, 345 sort (samtools), 367 sort program, 147 --parallel option, 152 -s option, sorting stability and, 149 -V option, 150 cleaning up gene names matched by grep, 145 grouping ranges for bedtools genomecov, 336 handling tabular data properly, 148 merge sort algorithm, 15 sort | uniq, 153 sorting BED files for bedtools intersect, 332 using different delimiters with sort, 148 validating sorting of a file, 150 sorting dataframe columns in R, using dplyr func‐ tion arrange(), 245 using tools other than Unix sort, 152 sorting stability, 149, 477 source() function (R), 255 space-delimited file formats, 129 split() function (R) GRangesList objects created by, 322 using on GRanges, 306 split-apply-combine pattern (R), 239-243 applying a function using lapply(), 239 combining data, 240 using do.call(), 241 

using summay() function, 240 convenience functions wrapping split(), lap‐ ply() and combine steps, 242 splitting data with split(), 239 using with GRangesList, 307 spreadsheet syndrome, 448, 478 SQL (Structured Query Language), 430 SQLite, 428, 431-465 aggregate functions, 442 grouping rows with GROUP BY, 444 database underlying TranscriptDb objects, 313 dropping tables and deleting databases, 458 dumping databases, 465 exploring databases with the command-line interface, 431 functions, 441 installing, 431 interacting with, from Python, 459 conecting to databases and creating tables, 459 loading data into a table, 461 organizing relational databases and joins, 448-455 inner joins, 451 left outer joins, 453 querying data with SELECT command, 434-440 filtering rows with WHERE, 437 limiting results with LIMIT, 435 ordering rows with ORDER BY, 436 selecting columns, 435 RSQLite package, 196 subqueries, 447 writing to databases, 455-458 creating tables, 455 indexing, 457 inserting records into tables, 457 sqlite3 (command-line tool), 431 working with, 434 sqrt() function (R), 179 SSH, 478 connecting to remote machines via, 57 quick authentication with SSH keys, 59 SSH config files, 58 using SSH keys for authentication in Git‐ Hub, 87 ssh command, 57 -add option, 60 

-agent option, 60 -copy-id options, 60 -keygen option, 59 -p option, 58 stable sorting, 149 staging files (Git), 73 git diff and, 78 tracked files’ changes, staging and commit‐ ting, 76 undoing with git reset, 83 standard error, 478 file descriptor, 44 redirected, monitoring with tail -f, 45 redirecting, 43, 49 standard input, 478 file descriptor, 44 redirection, using, 45 standard output, 478 curl command writing to, 112 file descriptor, 44 redirecting standard error to, 49 redirecting to a file, 41 start position and end position (ranges), 264 accessing for GRanges object, 301 getting for IRanges object, 273 statistical analysis of high-dimensional genom‐ ics data, 176 statistics and probability, 468 stop() function (R), 252 stopifnot() function (R), 252 str() function (R), 230 calling on IRanges object, 273 strand() function, 301 strands, 265 bedtools intersect and, 332 flank() handling of, 315 forward and reverse, genomic features on, 268 gaps between ranges and, 319 streams, 39, 41, 45 (see also standard error; standard input; standard output) editing with sed, 165-169 file descriptors on Unix, 44 strings character vectors in R, 189, 191 column of strings, coercion into factors, 197, 200 comparison operators in Bash, 402 

concatenating in awk, 158 string-matching functions to search BSge‐ nome objects, 317 working with in R, 248-253 combining sub() and strsplit(), 252 grep() function, 248 nchar() function, 248 paste() function, 252 regexpr() function, 249 sub() function, 250 substr() function, 250 strsplit() function (R), 252 sub() function (R), 250, 258 combining with strsplit(), 252 subcommands (Git), 70 subject ranges (sbj), 283 and select parameter of findOverlaps(), 285 building interval trees from, 286 mapping between qry ranges and, repre senting overlap, 283 using countOverlaps() and subsetByOver laps() with, 289 subjectHits() function, 284 subj_it object, 287 subprojects, creating using diretories, 26 subqueries, 447 subset() function (R), 206 subsetByOverlaps() function, 289, 327 subsetting, 185 dataframes in R, 203-207 using filter() function in dplyr, 244 GRanges object, 302 IRanges object, 274 lists in R, 229 run-length encoded sequences, 295 subshells, 169, 426 combining sequential commands' standard output into single stream, 170 substitute command (sed), 166 substr() function (R), 250 use of closed intervals, 267 sum program, 121 sum() function (R), 203 summarize() function (dplyr), 247 summary() function (R), 193, 203 using in split-apply-combine pattern, 240 suspending a process, 51 

## T

t-test, 175 tab-delimited files, 128 bioawk options for working with, 164 columns and, 139 indexing with Tabix, 427 reading with read.delim() function in R, 196 Tabix, 426 compressing files for, using bgzip, 426 indexing files with, 427 using to make fast queries, 427 table() function (R), 192, 202 counting items in a bin, 216 tables (database) creating, 455 dropping, 458 tabular data, wide and long formats, 198 tabular plain-text data formats, 128 handling tabular data with sort, 148 processing with awk, 136 tail -f, monitoring redirected standard error with, 45 tail program, 130 cutting off comments with tail -n, 137 weakness of using tail -n to remove com‐ ment header, 137 Tandem Repeats Finder, 343 tapply() function (in R), 242 tar program, 120 tbl_df class, 243 tcsh shell, 40 tee program, 49 terminal colors setting (Git), 70 terminal multiplexer (see Tmux) terminal pagers, 131 test command (Bash), 402 combining with if statements, 404 file and directory test expressions, 403 testing code, 13 importance of, strategy for determining, 13 text streams, 39 text-processing tasks, R language and, 248 titles, plot titles in ggplot2, 209 TLEN (template length), 360 Tmux (terminal multiplexer), 61-65 common key sequences, 64 common subcommands, 65 creating, detaching, and attaching sessions, 62 

installing and configuring, 62 working with Tmux windows, 64 tools developing frequently used scripts into, 15 for bioinformatics, 467 rapid development for bioinformatics, 3 TopHat and Cufflinks suite of tools, 325 Torvalds, Linus, 67 touch command, creating README files, 25 transcript annotation packages, 309 (see also annotation data) TranscriptDb object, 308, 310 coercing chromosome information into GRanges object, 320 creating, 312 extracting and collapsing overlapping exons with reduce(), 325 extracting exons from, 322 transcription termination site (TTS), 277 transcripts, 269, 322 (see also TranscriptDb object) retrieving with GenomicFeatures, 311 transcriptsByOverlaps() function, 312 transition start site (TSS), 277 transparency level (alpha), 211 trimmer program, 119 Tukey, J. W., 175 tuple unpacking, 351 tview (samtools), 372 TxDb.Mmusculus.UCSC.mm10.ensGene pack‐ age, 309 type affinities, 433 type coercion in R, 190 coercing TranscriptDb object's chromosome information into GRanges, 320 column of strings, coercion into factors, 197 sum() function, coercing logical values to integers, 203 typeof() function (R), 191 

## U

UCSC chromosome name style, 318 UCSC Ensembl track, 309 UCSC Genome Browser, 110, 440 creating TranscriptDb object for annotation data, 312 no inconsistent naming issues with, 251 rtracklayer interfacing with, 314 undersmoothing, 218 

unified diff format, 78, 117 union, 280 unionbedg (bedtools), 337 uniq program, 152 -d (duplicates) option, 154 -i option for case insensitivity, 153 cleaning up gene names matched by grep, 145 sort | uniq, 153 sort | uniq and sort | uniq -c, 153 unit testing, 13, 478 Unix, 467 common filename wildcards, 29 Unix data tools, 125-174, 169, 330 (see also BEDTools) (see also Unix shell) implementations, BSD utils and GNU coreutils, 142 inspecting and manipulating text data, 128 bioawk, 163-165 column data, using cut and column, 13 decoding plain-text data, 145 finding uniqe values with uniq, 152 grep, 140-145 information on plain-text files with wc, ls, and awk, 134 inspecting data with head and tail, 129 join, 155 sorting plain-text data with sort, 147 text processing with awk, 157-163 using less, 131 using one-liners and pipelines, historica example, 125 using pipeline approach, 127 Unix philosophy, 173 Unix shell, 37-56 advanced shell tricks, 169-173 named pipes and process substitution, 171 subshells, 169 command substitution, 54 managing and interacting with processes, 50-53 many versions, 40 modularity and the Unix philosophy, 37 pipes, 45-50 power of, 40 working with streams and redirection, 41-45 

unlist() function (R), 240 UNMAP flag, 369 unsplit() function (R), 241, 306 unstable sorting, 149 upstream flanking (ranges), 277 user accounts, getting with whoami command, 59 UTF-8 encoding, 146 

## V

values, special, in R, 190 variables Bash, 398 in awk, 157, 160 in R, 182 Variant Call Format (VCF), 380-383, 478 variant calling, 378, 379-383 vectorization (R), 183 vectorized logical operations, 188 vectors (R), 183 accessing elements by name, 185 creating a dataframe from, 200 dataframe column returned as a vector, 202 dataframe columns as vectors, 205 factors and classes, 191 functions returning index of first minimum or maximum element, 206 homogeneous data type and type coercion, 190 indexing of, 184 lists versus, 228 matching, 219 vector types, 189 version control systems (VCS), 67 (see also Git) versions documenting data version informtion, 25 documenting for software used, 25 views, 292, 296, 297 vignettes for Bioconductor packages, 270 visualization, 176 

## W

warning() function (R), 252 wc (word count) program, 134 assumption of well-formatted data, 136 wget program, 110 --user and --ask-password options, 110 recursive downloading, 111 

useful options, 111 wgsim read simulator, 356 WHERE clause, 437 aggregate functions with, 443 common operators used in, 438 which() function (R), 206 which.max() function (R), 206 which.min() function (R), 206 while loops (R), 253 whitespace, finding in a file with grep, 136 whoami command, 59 Wickham, Hadley, 207, 235, 243 wide format (tabular data), 198 wildcards expanding to match file names, 28 matching, brace expansion versus, 29 using in Bash for file globbing, 410 Wilk, M. B., 175 word count program (see wc program) word splitting in Bash, 407 workflows (GitHub) forking repositories, 97 shared central repository, 84 

working directory in R, 194, 255 write.table() function (R), 260 

## X

xargs program, 416 and parallelization, 419 BSD and GNU xargs, 419 playing safe with, 417 using, 411 using with replacement strings to apply commands to files, 418 xargs, pipes, and redirects, 420 

## Z

zdiff program, 120 zeros, leading, in filenames, 30 zgrep, 120, 170 extracting FASTA headers from gzipped file, 121 zless program, 120 zsh (Z shell), 40