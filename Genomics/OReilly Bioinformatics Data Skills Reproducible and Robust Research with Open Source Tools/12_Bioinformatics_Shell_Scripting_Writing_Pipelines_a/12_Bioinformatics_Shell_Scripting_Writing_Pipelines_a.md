# Bioinformatics Shell Scripting, Writing Pipelines, and Parallelizing Tasks

I’ve waited until the penultimate chapter this book to share a regrettable fact: every‐ day bioinformatics work often involves a great deal of tedious data processing. Bioin‐ formaticians regularly need to run a sequence of commands on not just one file, but dozens (sometimes even hundreds) of files. Consequently, a large part of bioinfor‐ matics is patching together various processing steps into a pipeline, and then repeat‐ edly applying this pipeline to many files. This isn’t exciting scientific work, but it’s a necessary hurdle before tackling more exciting analyses. 

While writing pipelines is a daily burden of bioinformaticians, it’s essential that pipe‐ lines are written to be robust and reproducible. Pipelines must be robust to problems that might occur during data processing. When we execute a series of commands on data directly into the shell, we usually clearly see if something goes awry—output files are empty when they should contain data or programs exit with an error. However, when we run data through a processing pipeline, we sacrifice the careful attention we paid to each step’s output to gain the ability to automate processing of numerous files. The catch is that not only are errors likely to still occur, they’re more likely to occur because we’re automating processing over more data files and using more steps. For these reasons, it’s critical to construct robust pipelines. 

Similarly, pipelines also play an important role in reproducibility. A well-crafted pipe‐ line can be a perfect record of exactly how data was processed. In the best cases, an individual could download your processing scripts and data, and easily replicate your exact steps. However, it’s unfortunately quite easy to write obfuscated or sloppy pipe‐ lines that hinder reproducibility. We’ll see some principles that can help you avoid these mistakes, leading to more reproducible projects. 

In this chapter, we’ll learn the essential tools and skills to construct robust and repro‐ ducible pipelines. We’ll see how to write rerunnable Bash shell scripts, automate fileprocessing tasks with find and xargs, run pipelines in parallel, and see a simple makefile. Note that the subset of techniques covered in this chapter do not target a specific cluster or high-performance computing (HPC) architecture—these are gen‐ eral Unix solutions that work well on any machine. For parallelization techniques specific to your HPC system, you will need consult its documentation. 

## Basic Bash Scripting

We’ve found that duct tape is not a perfect solution for anything. But with a little ingenuity, in a pinch, it’s an adequate solution for just about everything. 

— Mythbusters’ Jamie Hyneman 

Bash, the shell we’ve used interactively throughout the book, is also a full-fledged scripting language. Like many other tools presented in this book, the trick to using Bash scripts effectively in bioinformatics is knowing when to use them and when not to. Unlike Python, Bash is not a general-purpose language. Bash is explicitly designed to make running and interfacing command-line programs as simple as possible (a good characteristic of a shell!). For these reasons, Bash often takes the role as the duct tape language of bioinformatics (also referred to as a glue language), as it’s used to tape many commands together into a cohesive workflow. 

Before digging into how to create pipelines in Bash, it’s important to note that Python may be a more suitable language for commonly reused or advanced pipelines. Python is a more modern, fully featured scripting language than Bash. Compared to Python, Bash lacks several nice features useful for data-processing scripts: better numeric type support, useful data structures, better string processing, refined option parsing, avail‐ ability of a large number of libraries, and powerful functions that help with structur‐ ing your programs. However, there’s more overhead when calling command-line programs from a Python script (known as calling out or shelling out) compared to Bash. Although Bash lacks some of Python’s features, Bash is often the best and quickest “duct tape” solution (which we often need in bioinformatics). 

## Writing and Running Robust Bash Scripts

Most Bash scripts in bioinformatics are simply commands organized into a rerunnable script with some added bells and whistles to check that files exist and ensur‐ ing any error causes the script to abort. These types of Bash scripts are quite simple to write: you’ve already learned important shell features like pipes, redirects, and back‐ ground processes that play an important role in Bash scripts. In this section, we’ll cover the basics of writing and executing Bash scripts, paying particular attention to how create robust Bash scripts. 

## A robust Bash header

By convention, Bash scripts have the extension .sh. You can create them in your favorite text editor (and decent text editors will have support for Bash script syntax highlighting). Anytime you write a Bash script, you should use the following Bash script header, which sets some Bash options that lead to more robust scripts (there’s also a copy of this header in the template.sh file in this chapter’s directory on GitHub): 

```shell
#!/bin/bash ①
set -e ②
set -u ③
set -o pipefail ④ 
```

1 This is called the shebang, and it indicates the path to the interpreter used to exe‐ cute this script. This is only necessary when running the script as a program (more on this in a bit). Regardless of how you plan to run your Bash script, it’s best to include a shebang line. 

② By default, a shell script containing a command that fails (exits with a nonzero exit status) will not cause the entire shell script to exit—the shell script will just continue on to the next line. This is not a desirable behavior; we always want errors to be loud and noticeable. set -e prevents this, by terminating the script if any command exited with a nonzero exit status. Note, however, that set -e has complex rules to accommodate cases when a nonzero exit status indicates “false” rather than failure. For example, test -d file.txt will return a nonzero exit status if its argument is not a directory, but in this context this isn’t meant to rep‐ resent an error. set -e ignores nonzero statuses in if conditionals for this rea‐ son (we’ll discuss this later). Also, set -e ignores all exit statuses in Unix pipes except the last one—this relates to set -o pipefail, which we discuss later. 

③ set -u fixes another unfortunate default behavior of Bash scripts: any command containing a reference to an unset variable name will still run. As a horrifying example of what this can lead to, consider: rm -rf $TEMP_DIR/*. If the shell vari‐ able $TEMP_DIR isn’t set, Bash will still substitute its value (which is nothing) in place of it. The end result is rm -rf /*! You can see this for yourself: 

```shell
$ echo "rm $NOTSET/blah"
rm /blah 
```

set -u prevents this type of error by aborting the script if a variable’s value is unset. 

④ As just discussed, set -e will cause a script to abort if a nonzero exit status is encountered, with some exceptions. One such exception is if a program run in a Unix pipe exited unsuccessfully; unless this program was the last program in the pipe, this would not cause the script to abort even with set -e. Including set -o pipefail will prevent this undesirable behavior—any program that returns a nonzero exit status in the pipe will cause the entire pipe to return a nonzero sta‐ tus. With set -e enabled, too, this will lead the script to abort. 

![image](images/fig_001.jpg)


## The Robust Bash Header in Bash Script Examples in this Chapter

I will omit this header in Bash scripts throughout this chapter for clarity and to save space, but you should always use it in your own work. 

These three options are the first layer of protection against Bash scripts with silent errors and unsafe behavior. Unfortunately, Bash is a fragile language, and we need to mind a few other oddities to use it safely in bioinformatics. We’ll see these as we learn more about the language. 

## Running Bash scripts

Running Bash scripts can be done one of two ways: with the bash program directly (e.g., bash script.sh), or calling your script as a program (./script.sh). For our purposes, there’s no technical reason to prefer one approach over the other. Practi‐ cally, it’s wise to get in the habit of running scripts you receive with ./script.sh, as they might use interpreters other than /bin/bash (e.g., zsh, csh, etc.). But while we can run any script (as long as it has read permissions) with bash script.sh, calling the script as an executable requires that it have executable permissions. We can set these using: 

## $ chmod u+x script.sh

This adds executable permissions (+x) for the user who owns the file (u). Then, the script can be run with ./script.sh. 

## Variables and Command Arguments

Bash variables play an extremely important role in robust, reproducible Bash scripts. Processing pipelines having numerous settings that should be stored in variables (e.g., which directories to store results in, parameter values for commands, input files, etc.). Storing these settings in a variable defined at the top of the file makes adjusting set‐ tings and rerunning your pipelines much easier. Rather than having to change numerous hardcoded values in your scripts, using variables to store settings means you only have to change one value—the value you’ve assigned to the variable. Bash also reads command-line arguments into variables, so you’ll need to be familiar with accessing variables’ values to work with command-line arguments. 

Unlike other programming languages, Bash’s variables don’t have data types. It’s help‐ ful to think of Bash’s variables as strings (but that may behave differently depending on context). We can create a variable and assign it a value with (note that spaces mat‐ ter when setting Bash variables—do not use spaces around the equals sign!): 

```txt
results_dir="results/" 
```

To access a variable’s value, we use a dollar sign in front of the variable’s name (e.g., $results_dir). You can experiment with this in a Bash script, or directly on the com‐ mand line: 

```shell
results_dir="results/"
echo $results_dir
results/ 
```

As mentioned in the previous section, you should always set set -u to force a Bash script to exit if a variable is not set. 

Even though accessing a variable’s value using the dollar sign syntax works, it has one disadvantage: in some cases it’s not clear where a variable name ends and where an adjacent string begins. For example, suppose a section of your Bash script created a directory for a sample’s alignment data, called <sample>_aln/, where <sample> is replaced by the sample’s name. This would look like: 

```txt
sample="CNTRL01A"
mkdir $sample_aln/ 
```

Although the intention of this code block was to create a directory called CNTRL01A_aln/, this would actually fail, because Bash will try to retrieve the value of a variable named $sample_aln. To prevent this, wrap the variable name in braces: 

```txt
sample="CNTRL01A"
mkdir ${sample}_aln/ 
```

Now, a directory named CNTRL01A_aln/ will be created. While this solves our immediate problem of Bash interpreting sample_aln as the variable name, there’s one more step we should take to make this more robust: quoting variables. This prevents commands from interpreting any spaces or other special characters that the variable may contain. Our final command would look as follows: 

```hcl
sample="CNTRL01A"
mkdir "${sample}_aln/" 
```

## Command-line arguments

Let’s now look at how Bash handles command-line arguments (which are assigned to the value $1, $2, $3, etc.). The variable $0 stores the name of the script. We can see this ourselves with a simple example script: 

#!/bin/bash 

```shell
echo "script name: $0"
echo "first arg: $1"
echo "second arg: $2"
echo "third arg: $3" 
```

Running this file prints arguments assigned to $0, $1, $2, and $3: 

```yaml
$ bash args.sh arg1 arg2 arg3
script name: args.sh
first arg: arg1
second arg: arg2
third arg: arg3 
```

Bash assigns the number of command-line arguments to the variable $# (this does not count the script name, $0, as an argument). This is useful for user-friendly messages (this uses a Bash if conditional, which we’ll cover in more depth in the next section): 

```shell
#!/bin/bash
if [ "$#" -lt 3 ] # are there less than 3 arguments?
then
    echo "error: too few arguments, you provided $#, 3 required"
    echo "usage: script.sh arg1 arg2 arg3"
    exit 1
fi
echo "script name: $0"
echo "first arg: $1"
echo "second arg: $2"
echo "third arg: $3" 
```

Running this with too few arguments gives an error (and leads the process to exit with a nonzero exit status—see “Exit Status: How to Programmatically Tell Whether Your Command Worked” on page 52 if you’re rusty on what exit statuses mean): 

```asm
$ ./script.sh some_arg
error: too few arguments, you provided 1, 3 required
usage: script.sh arg1 arg2 arg3 
```

It’s possible to have more complex options and argument parsing with the Unix tool getopt. This is out of the scope of this book, but the manual entry for getopt is quite thorough. However, if you find your script requires numerous or complicated options, it might be easier to use Python instead of Bash. Python’s argparse module is much easier to use than getopt. 

![image](images/fig_002.jpg)


## Reproducibility and Environmental Variables

Some bioinformaticians make use of environmental variables to store settings using the command export, but in general this makes scripts less portable and reproducible. Instead, all important settings should be stored inside the script as variables, rather than as external environmental variables. This way, the script is selfcontained and reproducible. 

Variables created in your Bash script will only be available for the duration of the Bash process running that script. For example, running a script that creates a variable with some_var=3 will not create some_var in your current shell, as the script runs in an entirely separate shell process. 

## Conditionals in a Bash Script: if Statements

Like other scripting languages, Bash supports the standard if conditional statement. What makes Bash a bit unique is that a command’s exit status provides the true and false (remember: contrary to other languages, 0 represents true/success and anything else is false/failure). The basic syntax is: 

```shell
if [commands] 1
then
    [if-statements] 2
else
    [else-statements] 3
fi 
```

0 [commands] is a placeholder for any command, set of commands, pipeline, or test condition (which we’ll see later). If the exit status of these commands is 0, execu‐ tion continues to the block after then; otherwise execution continues to the block after else. The then keyword can be placed on the same line as if, but then a semicolon is required: if [commands]; then. 

[if-statements] is a placeholder for all statements executed if [commands] eval‐ uates to true (0). 

[else-statements] is a placeholder for all statements executed if [commands] evaluates to false (1). The else block is optional. 

Bash’s if condition statements may seem a bit odd compared to languages like Python, but remember: Bash is primarily designed to stitch together other com‐ mands. This is an advantage Bash has over Python when writing pipelines: Bash allows your scripts to directly work with command-line programs without requiring any overhead to call programs. Although it can be unpleasant to write complicated programs in Bash, writing simple programs is exceedingly easy because Unix tools and Bash harmonize well. For example, suppose we wanted to run a set of commands only if a file contains a certain string. Because grep returns 0 only if it matches a pat‐ tern in a file and 1 otherwise, we could use: 

```shell
#!/bin/bash
if grep "pattern" some_file.txt > /dev/null then
    # commands to run if "pattern" is found
    echo "found 'pattern' in 'some_file.txt"
fi 
```

This grep command is our condition statement. The redirection is to tidy the output of this script such that grep’s output is redirected to /dev/null and not to the script’s standard out. 

The set of commands in an if condition can use all features of Unix we’ve mastered so far. For example, chaining commands with logical operators like && (logical AND) and || (logical OR): 

```shell
#!/bin/bash
if grep "pattern" file_1.txt > /dev/null &&
    grep "pattern" file_2.txt > /dev/null
then
    echo "found 'pattern' in 'file_1.txt' and in 'file_2.txt'"
fi 
```

We can also negate our program’s exit status with !: 

```shell
#!/bin/bash
if ! grep "pattern" some_file.txt > /dev/null
then
    echo "did not find 'pattern' in 'some_file.txt'
fi 
```

Finally, it’s possible to use pipelines in if condition statements. Note, however, that the behavior depends on set -o pipefail. If pipefail is set, any nonzero exit status in a pipe in your condition statement will cause execution to continue on, skipping the if-statements section (and going on to the else block if it exists). However, if pipefail is not set, only the exit status of the last command is considered. Rather than trying to remember all of these rules, just use the robust header provided earlier —pipefail is a more sensible default. 

The final component necessary to understand Bash’s if statements is the test com‐ mand. Like other programs, test exits with either 0 or 1. However test’s exit status indicates the return value of the test specified through its arguments, rather than exit success or error. test supports numerous standard comparison operators (whether two strings are equal, whether two integers are equal, whether one integer is greater than or equal to another, etc.). Bash can’t rely on familiar syntax such as > for “greater than,” as this is used for redirection: instead, test has its own syntax (see Table 12-1 for a full list). You can get a sense of how test works by playing with it directly on the command line (using ; echo "$?" to print the exit status): 

$test "ATG" = "ATG" ; echo "$?"$ $test "ATG" = "atg" ; echo "$?"$ $test 3 -lt 1 ; echo "$?"$ $test 3 -le 3 ; echo "$?"$ 0 


Table 12-1. String and integer comparison operators


<table><tr><td>String/integer</td><td>Description</td></tr><tr><td>-z str</td><td>String str is null (empty)</td></tr><tr><td>str1 = str2</td><td>Strings str1 and str2 are identical</td></tr><tr><td>str1 != str2</td><td>Strings str1 and str2 are different</td></tr><tr><td>int1 -eq int2</td><td>Integers int1 and int2 are equal</td></tr><tr><td>int1 -ne int2</td><td>Integers int1 and int2 are not equal</td></tr><tr><td>int1 -lt int2</td><td>Integer int1 is less than int2</td></tr><tr><td>int1 -gt int2</td><td>Integer int1 is greater than int2</td></tr><tr><td>int1 -le int2</td><td>Integer int1 is less than or equal to int2</td></tr><tr><td>int1 -ge int2</td><td>Integer int1 is greater than or equal to int2</td></tr></table>

In practice, the most common conditions you’ll be checking aren’t whether some integer is less than another, but rather checking to see if files or directories exist and whether you can write to them. test supports numerous file- and directory-related test operations (the few that are most useful in bioinformatics are in Table 12-2). Let’s look at a few basic command-line examples: 

$test -d some_directory ; echo $? # is this a directory?$ $test -f some_file.txt ; echo $? # is this a file?$ $test -r some_file.txt ; echo $? $ is this file readable?$ 

```shell
0
$ test -w some_file.txt ; echo $? $ is this file writable?
1 
```


Table 12-2. File and directory test expressions


```txt
File/directory expression Description
-d dir dir is a directory
-f file file is a file
-e file file exists
-h link link is a link
-r file file is readable
-w file file is writable
-x file file is executable (or accessible if argument is expression) 
```

Combining test with if statements is simple; test is a command, so we could use: 

```shell
if test -f some_file.txt
then
    [...]
fi 
```

However, Bash provides a simpler syntactic alternative to test statements: [ -f some_file.txt ] . Note the spaces around and within the brackets—these are required. This makes for much simpler if statements involving comparisons: 

```shell
if [-f some_file.txt]
then
    [...]
fi 
```

When using this syntax, we can chain test expressions with -a as logical AND, -o as logical OR, ! as negation, and parentheses to group statements. Our familiar && and || operators won’t work in test, because these are shell operators. As an example, suppose we want to ensure our script has enough arguments and that the input file is readable: 

```shell
#!/bin/bash
set -e
set -u
set -o pipefail
if [ "$#" -ne 1 -o ! -r "$1" ] 
```

```shell
then
echo "usage: script.sh file_in.txt"
exit 1
fi 
```

As discussed earlier, we quote variables (especially those from human input); this is a good practice and prevents issues with special characters. 

When chained together with -a or -e, test’s syntax uses short-circuit evaluation. This means that test will only evaluate as many expressions as needed to determine whether the entire statement is true or false. In this example, test won’t check if the file argument $1 is readable if there’s not exactly one argument provided (the first condition is true). These two expressions are combined with a logical OR, which only requires one expression to be true for the entire condition to be true. 

## Processing Files with Bash Using for Loops and Globbing

In bioinformatics, most of our data is split across multiple files (e.g., different treat‐ ments, replicates, genotypes, species, etc.). At the heart of any processing pipeline is some way to apply the same workflow to each of these files, taking care to keep track of sample names. Looping over files with Bash’s for loop is the simplest way to accomplish this. This is such an important part of bioinformatics processing pipelines that we’ll cover additional useful tools and methods in the next section of this chap‐ ter. 

There are three essential parts to creating a pipeline to process a set of files: 

• Selecting which files to apply the commands to 

• Looping over the data and applying the commands 

• Keeping track of the names of any output files created 

There are different computational tricks to achieve each of these tasks. Let’s first look at the simple ways to select which files to apply commands to. 

There are two common ways to select which files to apply a bioinformatics workflow to: approaches that start with a text file containing information about samples (their sample names, file path, etc.), and approaches that select files in directories using some criteria. Either approach is fine—it mostly comes down to what’s most efficient for a given task. We’ll first look at an approach that starts with sample names and return to how to look for files later. Suppose you have a file called samples.txt that tells you basic information about your raw data: sample name, read pair, and where the file is. Here’s an example (which is also in this chapter’s directory on GitHub): 

```txt
$ cat samples.txt
zmaysA R1 seq/zmaysA_R1.fastq
zmaysA R2 seq/zmaysA_R2.fastq 
```

```txt
zmaysB R1 seq/zmaysB_R1.fastq
zmaysB R2 seq/zmaysB_R2.fastq
zmaysC R1 seq/zmaysC_R1.fastq
zmaysC R2 seq/zmaysC_R2.fastq 
```

The first column gives sample names, the second column contains read pairs, and the last column contains the path to the FASTQ file for this sample/read pair combina‐ tion. The first two columns are called metadata (data about data), which is vital to relating sample information to their physical files. Note that the metadata is also in the filename itself, which is useful because it allows us to extract it from the filename if we need to. 

With this samples.txt file, the first step of creating the pipeline is complete: all infor‐ mation about our files to be processed, including their path, is available. The second and third steps are to loop over this data, and do so in a way that keeps the samples straight. How we accomplish this depends on the specific task. If your command takes a single file and returns a single file, the solution is trivial: files are the unit we are processing. We simply loop over each file and use a modified version of that file’s name for the output. 

Let’s look at an example: suppose that we want to loop over every file, gather quality statistics on each and every file (using the imaginary program fastq_stat), and save this information to an output file. Each output file should have a name based on the input file, so if a summary file indicates something is wrong we know which file was affected. There’s a lot of little parts to this, so we’re going to step through how to do this a piece at a time learning about Bash arrays, basename, and a few other shell tricks along the way. 

First, we load our filenames into a Bash array, which we can then loop over. Bash arrays can be created manually using: 

$ sample_names=(zmaysA zmaysB zmaysC) 

And specific elements can be extracted with (note Bash arrays are 0-indexed): 

```shell
echo ${sample_names[0]}
zmaysA
echo ${sample_names[2]}
zmaysC 
```

All elements are extracted with the cryptic-looking ${sample_files[@]}: 

```txt
$ echo ${sample_names[@]}
zmaysA zmaysB zmaysC 
```

You can also access how many elements are in the array (and each element’s index) with the following: 

```shell
echo ${#sample_names[@]}
3 
```

```txt
echo {!sample_names[@]}
0 1 2 
```

But creating Bash arrays by hand is tedious and error prone, especially because we already have our filenames in our sample.txt file. The beauty of Bash is that we can use a command substitution (discussed in “Command Substitution” on page 54) to construct Bash arrays (though this can be dangerous; see the following warning). Because we want to loop over each file, we need to extract the third column using cut -f 3 from samples.txt. Demonstrating this in the shell: 

```shell
$ sample_files=$(cut -f 3 samples.txt))
$ echo ${sample_files[@]}
seq/zmaysA_R1.fastq seq/zmaysA_R2.fastq seq/zmaysB_R1.fastq seq/zmaysB_R2.fastq seq/zmaysC_R1.fastq seq/zmaysC_R2.fastq 
```

Again, this only works if you can make strong assumptions about your filenames— namely that they only contain alphanumeric characters, (_), and (-)! If spaces, tabs, newlines, or special characters like * end up in filenames, it will break this approach. 

![image](images/fig_003.jpg)


## The Internal Field Separator, Word Splitting, and Filenames

When creating a Bash array through command substitution with sample_files=($(cut -f 3 samples.txt)), Bash uses word split‐ ting to split fields into array elements by splitting on the characters in the Internal Field Separator (IFS). The Internal Field Separator is stored in the Bash variable IFS, and by default includes spaces, tabs, and newlines. You can inspect the value of IFS with: 

```txt
$ printf %q "$IFS"
'$ \t\n' 
```

Note that space is included in IFS (the first character). This can introduce problems when filenames contain spaces, as Bash will split on space characters breaking the filename into parts. Again, the best way to avoid issues is to not use spaces, tabs, newlines, or special characters (e.g., *) in filenames—only use alphanumeric characters, (-), and (_). The techniques taught in this section assume files are properly named and are not robust against improperly named files. If spaces are present in filenames, you can set the value of IFS to just tabs and newlines; see this chapter’s README file on GitHub for additional details on this topic. 

With our filenames in a Bash array, we’re almost ready to loop over them. The last component is to strip the path and extension from each filename, leaving us with the most basic filename we can use to create an output filename. The Unix program base name strips paths from filenames: 

```txt
$ basename seqs/zmaysA_R1.fastq
zmaysA_R1.fastq 
```

basename can also strip a suffix (e.g., extension) provided as the second argument from a filename (or alternatively using the argument -s): 

```shell
$ basename seqs/zmaysA_R1.fastq .fastq
zmaysA_R1
$ basename -s .fastq seqs/zmaysA_R1.fastq
zmaysA_R1 
```

We use basename to return the essential part of each filename, which is then used to create output filenames for fastq_stat’s results. 

Now, all the pieces are ready to construct our processing script: 

```shell
#!/bin/bash

set -e
set -u
set -o pipefail

# specify the input samples file, where the third
# column is the path to each sample FASTQ file
sample_info=samples.txt

# create a Bash array from the third column of $sample_info
sample_files=$(cut -f 3 "$sample_info")) ①

for fastq_file in ${sample_files[@]} ②
do
    # strip .fastq from each FASTQ file, and add suffix
    # "-stats.txt" to create an output filename for each FASTQ file
    results_file="${(basename $fastq_file .fastq)-stats.txt" ③}
    # run fastq_stat on a file, writing results to the filename we've
    # above
    fastq_stat $fastq_file > stats/$results_file ④
done 
```

1 This line uses command substitution to create a Bash array containing all FASTQ files. Note that this uses the filename contained in the variable $sample_info, which can later be easily be changed if the pipeline is to be run on a different set of samples. 

Next, we loop through each sample filename using a Bash for loop. The expres‐ sion ${sample_files[@]} returns all items in the Bash array. 

③ This important line creates the output filename, using the input file’s filename. The command substitution $(basename $fastq_file .fastq) takes the current filename of the iteration, stored in $fastq_file, and strips the .fastq extension off. What’s left is the portion of the sample name that identifies the original file, to which the suffix -stats.txt is added. 

Finally, the command fastq_stat is run, using the filename of the current itera‐ tion as input, and writing results to stats/$results_file. 

That’s all there is to it. A more refined script might add a few extra features, such as using an if statement to provide a friendly error if a FASTQ file does not exist or a call to echo to report which sample is currently being processed. 

This script was easy to write because our processing steps took a single file as input, and created a single file as output. In this case, simply adding a suffix to each filename was enough to keep our samples straight. However, many bioinformatics pipelines combine two or more input files into a single output file. Aligning paired-end reads is a prime example: most aligners take two input FASTQ files and return one output alignment file. When writing scripts to align paired-end reads, we can’t loop over each file like we did earlier. Instead, each sample, rather than each file, is the process‐ ing unit. Our alignment step takes both FASTQ files for each sample, and turns this into a single alignment file for this sample. Consequently, our loop must iterate over unique sample names, and we use these sample names to re-create the input FASTQ files used in alignment. This will be clearer in an example; suppose that we use the aligner BWA and our genome reference is named zmays_AGPv3.20.fa: 

```shell
#!/bin/bash
set -e
set -u
set -o pipefail

# specify the input samples file, where the third
# column is the path to each sample FASTQ file
sample_info=samples.txt

# our reference
reference=zmays_AGPv3.20.fa

# create a Bash array from the first column, which are
# sample names. Because there are duplicate sample names
# (one for each read pair), we call uniq
sample_names=$(cut -f 1 "$sample_info" | uniq)) ①

for sample in ${sample_names[@]}
do
    # create an output file from the sample name
    results_file="${sample}.sam" ②
    bwa mem $reference ${sample}_R1.fastq ${sample}_R2.fastq \ ③
    > $results_file
done 
```

0 This is much like the previous example, except now we use cut to grab the first column (corresponding to sample names), and (most importantly) pipe these sample names to uniq so duplicates of the same sample name are removed. This is necessary because our first column repeats each sample name twice, once for each paired-end file. 

As before, we create an output filename for the current sample being iterated over. In this case, all that’s needed is the sample name stored in $sample. 

Our call to bwa provides the reference, and the two paired-end FASTQ files for this sample as input. Note how we can re-create the two input FASTQ files for a given sample name, as the naming of these files across samples is consistent. In practice, this is possible for a large amount of bioinformatics data, which often comes from machines that name files consistently. Finally, the output of bwa is redirected to $results_file. For clarity, I’ve omitted quoting variables in this command, but you may wish to add this. 

Finally, in some cases it might be easier to directly loop over files, rather than work‐ ing a file containing sample information like samples.txt. The easiest (and safest) way to do this is to use Bash’s wildcards to glob files to loop over (recall we covered glob‐ bing in “Organizing Data to Automate File Processing Tasks” on page 26). The syntax of this is quite easy: 

```shell
#!/bin/bash
set -e
set -u
set -o pipefail

for file in *.fastq
do
    echo "$file: " $(bioawk -c fastx 'END {print NR}' $file)
done 
```

This simple script uses bioawk to count how many FASTQ records are in a file, for each file in the current directory ending in .fastq. 

Bash’s loops are a handy way of applying commands to numerous files, but have a few downsides. First, compared to the Unix tool find (which we see in the next section), globbing is not a very powerful way to select certain files. Second, Bash’s loop syntax is lengthy for simple operations, and a bit archaic. Finally, there’s no easy way to par‐ allelize Bash loops in a way that constrains the number of subprocesses used. We’ll see a powerful file-processing Unix idiom in the next section that works better for some tasks where Bash scripts may not be optimal. 

## Automating File-Processing with fnd and xargs

In this section, we’ll learn about a more powerful way to specify files matching some criteria using Unix find. We’ll also see how files printed by find can be passed to another tool called xargs to create powerful Unix-based processing workflows. 

## Using fnd and xargs

First, let’s look at some common shell problems that find and xargs solve. Suppose you have a program named process_fq that takes multiple filenames through stan‐ dard in to process. If you wanted to run this program on all files with the suffix .fq, you might run: 

$ ls *.fq 

treatment-01.fq treatment 02.fq treatment-03.fq 

$ ls *.fq | process_fq 

Your shell expands this wildcard to all matching files in the current directory, and ls prints these filenames. Unfortunately, this leads to a common complication that makes ls and wildcards a fragile solution. Suppose your directory contains a file‐ name called treatment 02.fq. In this case, ls returns treatment 02.fq along with other files. However, because files are separated by spaces, and this file contains a space, process_fq will interpret treatment 02.fq as two separate files, named treatment and 02.fq. This problem crops up periodically in different ways, and it’s necessary to be aware of when writing file-processing pipelines. Note that this does not occur with file globbing in arguments—if process_fq takes multiple files as arguments, your shell handles this properly: 

$ process_fq *.fq 

Here, your shell automatically escapes the space in the filename treatment 02.fq, so process_fq will correctly receive the arguments treatment-01.fq, treatment 02.fq, treatment-03.fq. So why not use this approach? Alas, there’s a limit to the number of files that can be specified as arguments. It’s not unlikely to run into this limit when processing numerous files. As an example, suppose that you have a tmp/ directory with thousands and thousands of temporary files you want to remove before rerunning a script. You might try rm tmp/*, but you’ll run into a problem: 

$ rm tmp/* 

/bin/rm: cannot execute [Argument list too long] 

New bioinformaticians regularly encounter these two problems (personally, I am asked how to resolve these issues at least once every other month by various collea‐ gues). The solution to both of these problems is through find and xargs, as we will see in the following sections. 

## Finding Files with fnd

Unlike ls, find is recursive (it will search for matching files in subdirectories, and subdirectories of subdirectories, etc.). This makes find useful if your project direc‐ tory is deeply nested and you wish to search the entire project for a file. In fact, run‐ ning find on a directory (without other arguments) can be a quick way to see a project directory’s structure. Again, using the zmays-snps/ toy directory we created in “Organizing Data to Automate File Processing Tasks” on page 26: 

```txt
$ find zmays-snps
zmays-snps
zmays-snps/analysis
zmays-snps/data
zmays-snps/data/seqs
zmays-snps/data/seqs/zmaysA_R1.fastq
zmays-snps/data/seqs/zmaysA_R2.fastq
zmays-snps/data/seqs/zmaysB_R1.fastq
zmays-snps/data/seqs/zmaysB_R2.fastq
zmays-snps/data/seqs/zmaysC_R1.fastq
zmays-snps/data/seqs/zmaysC_R2.fastq
zmays-snps/scripts 
```

find’s recursive searching can be limited to search only a few directories deep with the argument -maxdepth. For example, to search only within the current directory, use -maxdepth 1; to search within the current directory and its subdirectories (but not within those subdirectories), use -maxdepth 2. 

The basic syntax for find is find path expression. Here, path specifies which directory find is to search for files in (if you’re currently in this directory, it’s simply find .). The expression part of find’s syntax is where find’s real power comes in. Expressions are how we describe which files we want to find return. Expressions are built from predicates, which are chained together by logical AND and OR operators. find only returns files when the expression evaluates to true. Through expressions, find can match files based on conditions such as creation time or the permissions of the file, as well as advanced combinations of these conditions, such as “find all files created after last week that have read-only permissions.” 

To get a taste of how simple predicates work, let’s see how to use find to match files by filename using the -name predicate. Earlier we used unquoted wildcards with ls, which are expanded by the shell to all matching filenames. With find, we quote pat‐ terns (much like we did with grep) to avoid our shells from interpreting characters like *. For example, suppose we want to find all files matching the pattern "zmaysB*fastq" (e.g., FASTQ files from sample “B”, both read pairs) to pass to a pipeline. We would use the command shown in Example 12-1: 

## Example 12-1. Find through flename matching

```txt
$ find data/seqs -name "zmaysB*fastq"
data/seqs/zmaysB_R1.fastq
data/seqs/zmaysB_R2.fastq 
```

This gives similar results to ls zmaysB*fastq, as we’d expect. The primary difference is that find reports results separated by newlines and, by default, find is recursive. 

## fnd’s Expressions

find’s expressions allow you to narrow down on specific files using a simple syntax. In the previous example, the find command (Example 12-1) would return directories also matching the pattern "zmaysB*fastq". Because we only want to return FASTQ files (and not directories with that matching name), we might want to limit our results using the -type option: 

```txt
$ find data/seqs -name "zmaysB*fastq" -type f
data/seqs/zmaysB_R1.fastq
data/seqs/zmaysB_R2.fastq 
```

There are numerous different types you can search for (e.g., files, directories, named pipes, links, etc.), but the most commonly used are f for files, d for directories, and l for links. 

By default, find connects different parts of an expression with logical AND. The find command in this case returns results where the name matches "zmaysB*fastq" and is a file (type “f ”). find also allows explicitly connecting different parts of an expres‐ sion with different operators. The preceding command is equivalent to: 

```shell
$ find data/seqs -name "zmaysB*fastq" -and -type f
data/seqs/zmaysB_R1.fastq
data/seqs/zmaysB_R2.fastq 
```

We might also want all FASTQ files from samples A or C. In this case, we’d want to chain expressions with another operator, -or (see Table 12-3 for a full list): 

```shell
$ find data/seqs -name "zmaysA*fastq" -or -name "zmaysC*fastq" -type f
data/seqs/zmaysA_R1.fastq
data/seqs/zmaysA_R2.fastq
data/seqs/zmaysC_R1.fastq
data/seqs/zmaysC_R2.fastq 
```

Table 12-3. Common fnd expressions and operators 

```txt
Operator/expression Description
-name <pattern> Match a filename to <pattern>, using the same special characters (*, ?, and [...] as Bash) 
```

```txt
Operator/expression Description
- iname <patern> Identical to -name, but is case insensitive
-empty Matches empty files or directories
-type <x> Matches types x (f for files, d for directories, l for links)
-size <size> Matches files where the <size> is the file size (shortcuts for kilobytes (k), megabytes (M), gigabytes (G), and terabytes (T) can be used); sizes preceded with + (e.g., +50M) match files at least this size; sizes preceded with - (e.g., -50M) match files at most this size
- regex Match by regular expression (extended regular expressions can be enabled with -E)
- iregex Identical to -regex, but is case insensitive
-print0 Separate results with null byte, not newline
expr -and expr Logical "and"
expr -or expr Logical "or"
-not/ "!" expr Negation
(expr) Group a set of expressions 
```

An identical way to select these files is with negation: 

```txt
$ find seqs -type f "!" -name "zmaysC*fastq"
seqs/zmaysA_R1.fastq
seqs/zmaysA_R2.fastq
seqs/zmaysB_R1.fastq
seqs/zmaysB_R2.fastq 
```

Let’s see one more advanced example. Suppose that a messy collaborator decided to create a file named zmaysB_R1-temp.fastq in seqs/. You notice this file because now your find command is matching it (we are still in the zmays/data directory): 

```txt
touch seqs/zmaysB_R1-temp.fastq
find seqs -type f "!" -name "zmaysC*fastq"
seqs/zmaysB_R1-temp.fastq
seqs/zmaysA_R1.fastq
seqs/zmaysA_R2.fastq
seqs/zmaysB_R1.fastq
seqs/zmaysB_R2.fastq 
```

You don’t want to delete his file or rename it, because your collaborator may need that file and/or rely on it having that specific name. So, the best way to deal with it seems to be to change your find command and talk to your collaborator about this mystery file later. Luckily, find allows this sort of advanced file querying: 

```txt
$ find seqs -type f "!" -name "zmaysC*fastq" -and "!" -name "*-temp*" seqs/zmaysA_R1.fastq
seqs/zmaysA_R2.fastq
seqs/zmaysB_R1.fastq
seqs/zmaysB_R2.fastq 
```

Note that find’s operators like !, (, and ) should be quoted so as to avoid your shell from interpreting these. 

## fnd’s -exec: Running Commands on fnd’s Results

While find is useful for locating a file, its real strength in bioinformatics is as a tool to programmatically access certain files you want to run a command on. In the previous section, we saw how find’s expressions allow you to select distinct files that match certain conditions. In this section, we’ll see how find allows you to run commands on each of the files find returns, using find’s -exec option. 

Let’s look at a simple example to understand how -exec works. Continuing from our last example, suppose that a messy collaborator created numerous temporary files. Let’s emulate this (in the zmays-snps/data/seqs directory): 

```txt
$ touch zmays{A,C}_R{1,2}-temp.fastq
$ ls
zmaysA_R1-temp.fastq zmaysB_R1-temp.fastq zmaysC_R1.fastq
zmaysA_R1.fastq zmaysB_R1.fastq zmaysC_R2-temp.fastq
zmaysA_R2-temp.fastq zmaysB_R2.fastq zmaysC_R2.fastq
zmaysA_R2.fastq zmaysC_R1-temp.fastq 
```

Suppose your collaborator allows you to delete all of these temporary files. One way to delete these files is with rm *-temp.fastq. However, rm with a wildcard in a direc‐ tory filled with important data files is too risky. If you’ve accidentally left a space between * and -temp.fastq, the wildcard * would match all files in the current direc‐ tory and pass them to rm, leading to everything in the directory being accidentally deleted. Using find’s -exec is a much safer way to delete these files. 

find’s -exec works by passing each file that matches find’s expressions to the com‐ mand specified by -exec. With -exec, it’s necessary to use a semicolon at the end of the command to indicate the end of your command. For example, let’s use find - exec and rm -i to delete these temporary files. rm’s -i forces rm to be interactive, prompting you to confirm that you want to delete a file. Our find and remove com‐ mand is: 

```shell
$ find . -name "*-temp.fastq" -exec rm -i {} \;
remove ./zmaysA_R1-temp.fastq? y
remove ./zmaysA_R2-temp.fastq? y
remove ./zmaysB_R1-temp.fastq? y
remove ./zmaysC_R1-temp.fastq? y
remove ./zmaysC_R2-temp.fastq? y 
```

In one line, we’re able to pragmatically identify and execute a command on files that match a certain pattern. Our command was rm but can just as easily be a bioinformat‐ ics program. Using this approach allows you to call a program on any number of files in a directory. With find and -exec, a daunting task like processing a directory of 100,000 text files with a program is simple. 

![image](images/fig_004.jpg)


## Deleting Files with fnd -exec

Deleting files with find -exec is a such a common operation that find also has a -delete option you can use in place of -exec -rm {} (but it will not be interactive, unlike rm with -i). 

When using -exec, always write your expression first and check that the files returned are those you want to apply the command to. Then, once your find com‐ mand is returning the proper subset of files, add in your -exec statement. find - exec is most appropriate for quick, simple tasks (like deleting files, changing permissions, etc.). For larger tasks, xargs (which we see in the next section) is a better choice. 

## xargs: A Unix Powertool

If there were one Unix tool that introduced me to the incredible raw power of Unix, it is xargs. xargs allows us to take input passed to it from standard in, and use this input as arguments to another program, which allows us to build commands pro‐ grammatically from values received through standard in (in this way, it’s somewhat similar to R’s do.call()). Using find with xargs is much like find with -exec, but with some added advantages that make xargs a better choice for larger bioinformat‐ ics file-processing tasks. 

Let’s re-create our messy temporary file directory example again (from the zmayssnps/data/seqs directory): 

```txt
$ touch zmays{A,C}_R{1,2}-temp.fastq # create the test files 
```

xargs works by taking input from standard in and splitting it by spaces, tabs, and newlines into arguments. Then, these arguments are passed to the command sup‐ plied. For example, to emulate the behavior of find -exec with rm, we use xargs with rm: 

```shell
find . -name "*-temp.fastq"
./zmaysA_R1-temp.fastq
./zmaysA_R2-temp.fastq
./zmaysC_R1-temp.fastq
./zmaysC_R2-temp.fastq
find . -name "*-temp.fastq" | xargs rm 
```

![image](images/fig_005.jpg)


## Playing It Safe with fnd and xargs

There’s one important gotcha with find and xargs: spaces in file‐ names can break things, because spaces are considered argument separators by xargs. This would lead to a filename like treatment 02.fq being interpreted as two separate arguments, treatment and 02.fq. The find and xargs developers created a clever solution: both allow for the option to use the null byte as a separator. Here is an example of how to run find and xargs using the null byte delimiter: 

```powershell
$ find . -name "samples [AB].txt" -print0 | xargs -0 rm 
```

In addition to this precaution, it’s also wise to simply not use file‐ names that contain spaces or other strange characters. Simple alphanumeric names with either dashes or underscores are best. To simplify examples, I will omit -print0 and -0, but these should always be used in practice. 

Essentially, xargs is splitting the output from find into arguments, and running: 

```shell
$ rm ./zmaysA_R1-temp.fastq ./zmaysA_R2-temp.fastq \
./zmaysC_R1-temp.fastq ./zmaysC_R2-temp.fastq 
```

xargs passes all arguments received through standard in to the supplied program (rm in this example). This works well for programs like rm, touch, mkdir, and others that take multiple arguments. However, other programs only take a single argument at a time. We can set how many arguments are passed to each command call with xargs’s -n argument. For example, we could call rm four separate times (each on one file) with: 

```powershell
$ find . -name "*-temp.fastq" | xargs -n 1 rm 
```

One big benefit of xargs is that it separates the process that specifies the files to oper‐ ate on (find) from applying a command to these files (through xargs). If we wanted to inspect a long list of files find returns before running rm on all files in this list, we could use: 

```shell
find . -name "*-temp.fastq" > files-to-delete.txt
cat files-to-delete.txt
./zmaysA_R1-temp.fastq
./zmaysA_R2-temp.fastq
./zmaysC_R1-temp.fastq 
```

```txt
./zmaysC_R2-temp.fastq
$ cat files-to-delete.txt | xargs rm 
```

Another common trick is to use xargs to build commands that are written to a sim‐ ple Bash script. For example, rather than running rm directly, we could call echo on rm, and then allow xargs to place arguments after this command (remember, xargs’s behavior is very simple: it just places arguments after the command you provide). For example: 

```shell
$ find . -name "*-temp.fastq" | xargs -n 1 echo "rm -i" > delete-temp.sh
$ cat delete-temp.sh
rm -i ./zmaysA_R1-temp.fastq
rm -i ./zmaysA_R2-temp.fastq
rm -i ./zmaysC_R1-temp.fastq
rm -i ./zmaysC_R2-temp.fastq 
```

Breaking up the task in this way allows us to inspect the commands we’ve built using xargs (because the command xargs runs is echo, which just prints everything). Then, we could run this simple script with: 

```shell
$ bash delete-temp.sh
remove ./zmaysA_R1-temp.fastq? y
remove ./zmaysA_R2-temp.fastq? y
remove ./zmaysC_R1-temp.fastq? y
remove ./zmaysC_R2-temp.fastq? y 
```

## Using xargs with Replacement Strings to Apply Commands to Files

So far, xargs builds commands purely by adding arguments to the end of the com‐ mand you’ve supplied. However, some programs take arguments through options, like program --in file.txt --out-file out.txt; others have many positional arguments like program arg1 arg2. xargs’s -I option allows more fine-grained placement of arguments into a command by replacing all instances of a placeholder string with a single argument. By convention, the placeholder string we use with -I is {}. 

Let’s look at an example. Suppose the imaginary program fastq_stat takes an input file through the option --in, gathers FASTQ statistics information, and then writes a summary to the file specified by the --out option. As in our Bash loop example (“Processing Files with Bash Using for Loops and Globbing” on page 405), we want our output filenames to be paired with our input filenames and have corresponding names. We can tackle this with find, xargs, and basename. The first step is to use find to grab the files you want to process, and then use xargs and basename to extract the sample name. basename allows us to remove suffixes through the argu‐ ment -s: 

```txt
$ find . -name "*.fastq" | xargs basename -s ".fastq" zmaysA_R1 
```

zmaysA_R2 zmaysB_R1 zmaysB_R2 zmaysC_R1 zmaysC_R2 

Then, we want to run the command fastq_stat --in file.fastq --out ../ summaries/file.txt, but with file replaced with the file’s base name. We do this by piping the sample names we’ve created earlier to another xargs command that runs fastq_stat: 

```shell
$ find . -name "*.fastq" | xargs basename -s ".fastq" | \
xargs -I{} fastq_stat --in {}.fastq --out ../summaries/{}.txt 
```

![image](images/fig_006.jpg)


## BSD and GNU xargs

Unfortunately, the behavior of -I differs across BSD xargs (which is what OS X uses) and GNU xargs. BSD xargs will only replace up to five instances of the string specified by -I by default, unless more are set with -R. In general, it’s better to work with GNU xargs. If you’re on a Mac, you can install GNU Coreutils with Homebrew. To prevent a clash with your system’s xargs (the BSD version), Homebrew prefixes its version with g so the GNU version of xargs would be gxargs. 

Combining xargs with basename is a powerful idiom used to apply commands to many files in a way that keeps track of which output file was created by a particular input file. While we could accomplish this other ways (e.g., through Bash for loops or custom Python scripts), xargs allows for very quick and incremental command building. However, as we’ll see in the next section, xargs has another very large advantage over for loops: it allows parallelization over a prespecified number of pro‐ cesses. Overall, it may take some practice to get these xargs tricks under your fingers, but they will serve you well for decades of bioinformatics work. 

## xargs and Parallelization

An incredibly powerful feature of xargs is that it can launch a limited number of pro‐ cesses in parallel. I emphasize limited number, because this is one of xargs’s strengths over Bash’s for loops. We could launch numerous multiple background processes with Bash for loops, which on a system with multiple processors would run in paral‐ lel (depending on other running tasks): 

```shell
for filename in *.fastq; do program "$filename" & done 
```

But this launches however many background processes there are files in *.fastq! This is certainly not good computing etiquette on a shared server, and even if you were the only user of this server, this might lead to bottlenecks as all processes start reading from and writing to the disk. Consequently, when running multiple process in parallel, we want to explicitly limit how many processes run simultaneously. xargs allows us to do this with the option -P <num> where <num> is the number of processes to run simultaneously. 

Let’s look at a simple example—running our imaginary program fastq_stat in par‐ allel, using at most six processes. We accomplish this by adding -P 6 to our second xargs call (there’s no point in parallelizing the basename command, as this will be very fast): 

```shell
$ find . -name "*.fastq" | xargs basename -s ".fastq" | \
xargs -P 6 -I{} fastq_stat --in {}.fastq --out ../summaries/{}.txt 
```

Generally, fastq_stat could be any program or even a shell script that performs many tasks per sample. The key point is that we provide all information the program or script needs to run through the sample name, which is what replaces the string {}. 

![image](images/fig_007.jpg)


## xargs, Pipes, and Redirects

One stumbling block beginners frequently encounter is trying to use pipes and redirects with xargs. This won’t work, as the shell process that reads your xargs command will interpret pipes and redirects as what to do with xarg’s output, not as part of the com‐ mand run by xargs. The simplest and cleanest trick to get around this limitation is to create a small Bash script containing the com‐ mands to process a single sample, and have xargs run this script in many parallel Bash processes. For example: 

```shell
#!/bin/bash
set -e
set -u
set -o pipefail 
```

```txt
sample_name=$(basename -s ".fastq" "$1") 
```

```txt
some_program ${sample_name}.fastq | another_program > ${sample_name}-results.txt 
```

Then, run this with: 

```shell
$ find . -name "*.fastq" | xargs -n 1 -P 4 bash script.sh 
```

Where -n 1 forces xargs to process one input argument at a time. This could be easily parallelized by specifying how many processes to run with -P. 

Admittedly, the price of some powerful xargs workflows is complexity. If you find yourself using xargs mostly to parallelize tasks or you’re writing complicated xargs commands that use basename, it may be worthwhile to learn GNU Parallel. GNU Par‐ allel extends and enhances xargs’s functionality, and fixes several limitations of xargs. For example, GNU parallel can handle redirects in commands, and has a shortcut ({/.}) to extract the base filename without basename. This allows for very short, powerful commands: 

```txt
$ find . -name "*.fastq" | parallel --max-procs=6 'program {/.} > {/.}-out.txt' 
```

GNU Parallel has numerous other options and features. If you find yourself using xargs frequently for complicated workflows, I’d recommend learning more about GNU Parallel. The GNU Parallel website has numerous examples and a detailed tuto‐ rial. 

## Make and Makefles: Another Option for Pipelines

Although this chapter has predominantly focused on building pipelines from Bash, and using find and xargs to apply commands to certain files, I can’t neglect to quickly introduce another very powerful tool used to create bioinformatics pipelines. This tool is Make, which interprets makefles (written in their own makefile lan‐ guage). Make was intended to compile software, which is a complex process as each step that compiles a file needs to ensure every dependency is already compiled or available. Like SQL (which we cover in Chapter 13), the makefile language is declara‐ tive—unlike Bash scripts, makefiles don’t run from top to bottom. Rather, makefiles are constructed as a set of rules, where each rule has three parts: the target, the prereq‐ uisites, and the recipe. Each recipe is a set of commands used to build a target, which is a file. The prerequisites specify which files the recipe needs to build the target file (the dependencies). The amazing ingenuity of Make is that the program figures out how to use all rules to build files for you from the prerequisites and targets. Let’s look at a simple example—we want to write a simple pipeline that downloads a file from the Internet and creates a summary of it: 

```makefile
FASTA_FILE_LINK=http://bit.ly/egfr_flank ①
.PHONY: all clean ②
all: egfr_comp.txt ③
egfr_flank.fa: ④
curl -L $(FASTA_FILE_LINK) > egfr_flank.fa
egfr_comp.txt: egfr_flank.fa ⑤
seqtk comp egfr_flank.fa > egfr_comp.txt 
```

```batch
clean: 6
rm -f egfr_comp.txt egfr_flank.fa 
```

We define a variable in a makefile much like we do in a Bash script. We keep this link to this FASTA file at the top so it is noticeable and can be adjusted easily. 

The targets all and clean in this makefile aren’t the names of files, but rather are just names of targets we can refer to. We indicate these targets aren’t files by spec‐ ifying them as prerequisites in the special target .PHONY. 

③ all is the conventional name of the target used to build all files this makefile is meant to build. Here, the end goal of this simple example makefile is to down‐ load a FASTA file from the Internet and run seqtk comp on it, returning the sequence composition of this FASTA file. The final file we’re writing sequence composition to is egfr_comp.txt, so this is the prerequisite for the all target. 

④ This rule creates the file egfr_fank.fa. There are no prerequisites in this rule because there are no local files needed for the creation of egfr_fank.fa (as we’re downloading this file). Our recipe uses curl to download the link stored in the variable FASTA_FILE_LINK. Since this is a shortened link, we use curl’s -L option to follow redirects. Finally, note that to reference a variable’s value in a makefile, we use the syntax $(VARIABLE). 

This rule creates the file containing the sequencing composition data, egfr_comp.txt. Because we need the FASTA file egfr_fank.fa to create this file, we specify egfr_fank.fa as a prerequisite. The recipe runs seqtk comp on the prereq‐ uisite, and redirects the output to the target file, egfr_comp.txt. 

6 Finally, it’s common to have a target called clean, which contains a recipe for cleaning up all files this makefile produces. This allows us to run make clean and return the directory to the state before the makefile was run. 

We run makefiles using the command make. For the preceding makefile, we’d run it using make all, where the all argument specifies that make should start with the all target. Then, the program make will first search for a file named Makefle in the cur‐ rent directory, load it, and start at the target all. This would look like the following: 

```txt
$ make all
curl -L http://bit.ly/egfr_flank > egfr_flank.fa
% Total % Received % Xferd Average Speed Time Time Time Current
Dload Upload Total Spent Left Speed
100 190 100 190 0 0 566 0 --:--:-- --:--:-- --:--:-- 567
100 1215 100 1215 0 0 529 0 0:00:02 0:00:02 --:--:-- 713
seqtk comp egfr_flank.fa > egfr_comp.txt 
```

An especially powerful feature of make is that it only generates targets when they don’t exist or when their prerequisites have been modified. This is very powerful: if you have a long and complex makefile, and you modified one file, make will only rerun the recipes for the targets that depend on this modifed fle (assuming you fully specified dependencies). Note what happens if we run make all again: 

$ make all 

make: Nothing to be done for `all'. 

Because all targets have been created and no input files have been modified, there’s nothing to do. Now, look what happens if we use touch to change the modification time of the egfr_fank.fa file: 

$ touch egfr_flank.fa 

$ make all 

seqtk comp egfr_flank.fa > egfr_comp.txt 

Because egfr_fank.fa is a prerequisite to create the egfr_comp.txt file, make reruns this rule to update egfr_comp.txt using the newest version of egfr_fank.txt. 

Finally, we can remove all files created with our clean target: 

$ make clean 

rm -f egfr_comp.txt egfr_flank.fa 

We’re just scratching the surface of Make’s capabilities in this example; a full tutorial of this language is outside the scope of this book. Unfortunately, like Bash, Make’s syntax can be exceedingly cryptic and complicated for some more advanced tasks. Additionally, because makefiles are written in a declarative way (and executed in a nonlinear fashion), debugging makefiles can be exceptionally tricky. Still, Make is a useful tool that you should be aware of in case you need an option for simple tasks and workflows. For more information, see the GNU Make documentation.