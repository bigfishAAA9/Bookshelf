# Appendix 1: Software Used in This Book

## A1.1 Introduction

We use a variety of different software packages in this book, which are all open source. Although there are a variety of programs to run different models, we have opted for ones such as R and PLINK that contain the most options. This appendix provides a short overview of the software used in this book that you will need to download in order to engage in the hands-on applications in the last two sections of the book. 

Although these programs are available in different operating systems (OSs), many advise to use GNU/Linux-based computer resources. For those wishing to make the jump, various excellent online tutorials exist, such as http://www.ee.surrey.ac.uk/Teaching/Unix/. When running the different commands, keep in mind that commands can differ by OS (see box 8.1). You can also refer to the online webpage to this book (http://www .intro-statistical-genetics.com) that will have hyperlinks (and if applicable updated links) and updated programs to those discussed here. 

## A1.2 RStudio and R

RStudio can be downloaded at https://www.rstudio.com/products/rstudio/download/. For this book we have used the freely available RStudio Desktop Open Source License. 

The version that is used in this book is RStudio Desktop 1.1.463. You will need to select the appropriate OS and follow the instructions on the website. This means downloading it and saving it in the appropriate directory. Note that RStudio also requires that you have the base program R, which you can download at http://cran.rstudio.com/. In this book we use the version R-3.5.2. Since there are many online tutorials and books on using RStudio and R, we do not replicate that material here. 

## A1.3 PLINK

Perhaps the most popular open-source free software program for QC (quality control) and GWAS analyses is PLINK $[1, 2]$ . It was developed by Shaun Purcell and colleagues and facilitates multiple types of data handling and usage. 

The home PLINK site is http://zzz.bwh.harvard.edu/plink/. 

PLINK can be downloaded from http://zzz.bwh.harvard.edu/plink/download.shtml. 

The PLINK 1.90 beta version can be downloaded from https://www.cog-genomics.org/plink2/. 

It is important that you download the version that matches your operating system (i.e., Linux (x86_64), Apple Mac). Versions are constantly being updated, so readers should check this site regularly. In this book we use PLINK 1.90 and 2.0. Specifically, we use PLINK v1.90b6.7 64-bit, which was updated on December 2, 2018. 

The basics of working with PLINK in the command line environment are described in detail in chapter 8 (section 8.2). Since there are many adequate resources, such as an excellent PLINK tutorial and active and helpful FAQs, mailing lists, and others, we do not repeat that information here. At the time of writing this book, PLINK 1.90 was in beta version and contains the same options. A clear advantage is that the new version is much faster. We recommend that neophytes follow the tutorial: http://zzz.bwh.harvard.edu/plink/tutorial.shtml. Once you have downloaded the zip archive with PLINK, no further installation is necessary. For usage, you need to type “plink” or “./plink” (depending on your OS) from the command line, followed by the options that you will run (see chapter 8). 

## A1.4 GCTA

In chapter 9 we use GCTA, which stands for Genome-wide Complex Trait Analysis and is available at https://cnsgenomics.com/software/gcta/#Overview [3]. It is developed by Jian Yang together with Peter Visscher, Mike Goddard, and Hong Lee. GCTA has multiple uses that can aid in the understanding of the genetic architecture of complex traits and supports many additional analyses not described in this introductory book. In this book we use the release v1.92.0beta1, released on February 1, 2019. To download GCTA, go to https://cnsgenomics.com/software/gcta/#Download. 

The executable files only support a 64-bit OS on the x86_64 CPU platform. Please note that the macOS and Windows versions include an additional file (libomp.dylib for macOS and libiomp5md.dll for Windows) in the /bin directory that needs to be located in the same folder as the executable file. 

## A1.5 PRSice

PRSice is polygenic score (PGS) software for calculating, applying, evaluating, and plotting the results of PGS, which we use in chapter 10 [4]. In this book, we used the version 2.1.4 of 

PRSice-2 [5]. PRSice can be downloaded from https://choishingwan.github.io/PRSice/. Note that depending on your OS, the downloaded files might have different extensions (i.e., PRSice _ linux, PRSice _ mac, PRSice _ win32.exe, or PRSice _ win64.exe). Please adapt the syntax accordingly or rename the respective file deleting the OS extension to execute the PRSice syntaxes in this book. See also box 8.1 for OS-specific commands. 

In order to plot graphs you will likely need an updated version of R, which is described on their website. To install all of the required R packages that you will need for PRSice, type the following commands: 

Rscript PRSice.R --dir 

## A1.6 Python

In chapter 10 we also use the statistical package LDpred, which is a software written in Python 3 [6]. To be able to run the examples you need to be sure that you have a Python version greater or equal to 3.5 installed in your computer. We recommend the installation of the Anaconda distribution of Python, because it contains the package management system pip that can be used to install and manage software packages written in Python. You can find Anaconda at https://www.anaconda.com/. 

There are two versions of Python Anaconda. One is based on Python 2 and the other one in Python 3. Some statistical packages presented in this book are based on Python 3 (LDpred), while others are currently written in Python 2 (LDSC and MTAG). However, you only need 1 version of Anaconda. Anaconda is also an environment manager and makes it very easy to go back and forth between Python 2 and 3 with a single installation of Anaconda. 

Once you have installed a version of Python in your system, you can verify which version is the default version in your system by following these commands: 

• Windows: Open the Anaconda Prompt 

• macOS: Open Launchpad, then open Terminal 

• Linux-CentOS: Open Applications - System Tools - Terminal. 

- Linux-Ubuntu: Open the Dash by clicking the upper left Ubuntu icon, then type "terminal." 

- Enter the command python. This command runs the Python shell. If Anaconda is installed and working, the version information it displays when it starts up will include "Anaconda." To exit the Python shell, enter the command quit(). 

## A1.6.1 How to switch from Python 3 to Python 2

You can navigate from different version of python using the Anaconda system. This is done by creating different Python environments. 

If you want to create a new environment with Python 2 installed, type: 

```txt
conda create -n python2 python=2.7 anaconda 
```

This will install the version of Python 2.7 and create two environments to work with. Beware that you need an Internet connection to run the previous command, and it will take a few minutes. 

Once you created the new environment, you can switch to Python 2 by doing the following: 

In Windows: 

```txt
activate python2 
```

In Linux, macOS: 

```txt
source activate python2 
```

To see the list of installed environments, you can type: 

```txt
conda env list 
```

This will show all the Python environments in your system. The * symbol shows which environment is active. 

```shell
# conda environments:
#
base /Users/nb17719/anaconda2
python2 /Users/nb17719/anaconda2/envs/python2
python3 * /Users/nb17719/anaconda2/envs/python3 
```

To deactivate a particular environment (and return to base), you can use the following command: 

In Windows: 

deactivate python2 

In Linux, macOS: 

```txt
source deactivate python2 
```

In case Python 2 is installed in your system, you can use the following command to create a new environment called python3: 

```txt
conda create -n python3 python=3.6 anaconda 
```

You then can use the previous commands to switch between different Python environments. 

## A1.6.2 Installing packages in Python

The Python software used in this book requires additional packages that can be installed in your environment. 

To run LDpred, for instance, you will need the following packages installed in your system: 

1. h5py http://www.h5py.org/ 

2. scipy http://www.scipy.org/ 

3. libplinkio https://github.com/mfranberg/libplinkio. 

The first two packages, h5py and scipy, are commonly used Python packages and are pre-installed on many computer systems. The last package, libplinkio, can be installed using pip (https://pip.pypa.io/en/latest/quickstart.html), which is also preinstalled in many systems. 

With pip, one can install libplinkio by typing the following command: 

pip install plinkio 

## A1.7 Git

Git is a the most used version control system—a category of software tools that help a software team manage changes to source code over time. We recommend the use of Git to download the latest version of LDpred, LDSC, and MTAG in your system. Git is available for Windows, Linux, and macOS from https://www.atlassian.com/git/tutorials/install-git. You will find a very complete guide on how to install Git in different systems. 

## A1.8 LDpred

Once you have all the Python and pip packages installed, you can install LDpred in your system. This can be done by downloading the source files from https://github.com/bvilhjal/ldpred/archive/master.zip or by typing the following if Git is installed in your system: 

git clone https://github.com/bvilhjal/ldpred.git 

To run the code presented in the last part of chapter 10, we assume that the LDpred software is installed in a folder called ldpred and that Python 3 is installed in your system and can be invoked by typing python3 in your terminal. 

To be certain that all the packages are installed, you can type: 

```batch
python3 ldpred/setup.py 
```

To get the best start possible, run the following command that runs a series of tests with LDpred: 

python3 ldpred/test.py 

test.py uses the test files contained in the /test_data directory downloaded with the software to calculate a series of PGS using LDscore. 

## A1.9 LDSC

LDSC is a software written in Python 2 that performs LD score regression and can be used to estimate heritability and genetic correlations of multiple traits. 

To install LDSC using Git, type: 

git clone https://github.com/bulik/ldsc.git 

To run LDSC, you need to switch to Python 2. You can activate a Python 2 environment following the instructions discussed previously in this appendix. 

To install all of the packages needed to run LDSC, it is possible to create a new Python environment using the following command: 

```batch
conda env create --file ldsc/environment.yml 
```

In Windows: 

activate ldsc 

In Linux, macOS: 

```txt
source activate ldsc 
```

To check that LDSC is correctly installed, type: 

```batch
python2 ldsc/ldsc.py -h 
```

## A1.10 MTAG

MTAG is a software written in Python 2. You can install the latest version of MTAG with Git using the following command: 

git clone https://github.com/omeed-maghzian/mtag.git 

To work properly, MTAG needs the following Python packages installed in the system: 

- numpy $(>= 1.13.1)$ 

- scipy 

• pandas (>=0.18.1) 

- argparse 

- bitarray (for ldsc) 

- joblib 

To install all of the packages needed to run MTAG, it is possible to create a new Python environment using the following command: 

conda env create --file mtag/environment.yml 

In Windows: 

activate mtag 

In Linux, macOS: 

```txt
source activate mtag 
```


Further documentation is available at https://github.com/omeed-maghzian/mtag/wiki.


## A1.11 Using Windows for this book

Windows users may experience some differences, depending on the operating system. It is possible to install Ubuntu on Windows 10, which will work in a virtually identical manner to Linux. For a tutorial on how to install Ubuntu, refer to https://tutorials.ubuntu.com/tutorial/tutorial-ubuntu-on-windows#0. Using the terminal on Ubuntu, it is possible to use most packages but also Bash, Z-shell, Korn, and other shell environments without virtual machines or dual-bootin. You will also be able to run tools such as SSH, git, apt, and dpkg directly from your Windows computer.