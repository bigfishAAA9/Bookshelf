# Bioinformatics with Python Cookbook

Learn how to use modern Python bioinformatics libraries and applications to do cutting-edge research in computational biology 

Tiago Antao 

![image](images/fig_001.jpg)


BIRMINGHAM - MUMBAI 

www.ebook777.comwww.it-ebooks.info 

# Bioinformatics with Python Cookbook

Copyright © 2015 Packt Publishing 

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews. 

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, nor Packt Publishing, and its dealers and distributors will be held liable for any damages caused or alleged to be caused directly or indirectly by this book. 

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information. 

First published: June 2015 

Production reference: 1230615 

Published by Packt Publishing Ltd. 

Livery Place 

35 Livery Street 

Birmingham B3 2PB, UK. 

ISBN 978-1-78217-511-7 

www.packtpub.com 

## Credits

Author Tiago Antao 

Project Coordinator Harshal Ved 

Reviewers Cho-Yi Chen Giovanni M. Dall'Olio 

Proofreader Safis Editing 

Commissioning Editor Nadeem N. Bagban 

Indexer Monica Ajmera Mehta 

Acquisition Editor Kevin Colaco 

Production Coordinator Arvindkumar Gupta 

Content Development Editor Gaurav Sharma 

Cover Work Arvindkumar Gupta 

Technical Editor Shashank Desai 

Copy Editor Relin Hedly 

## About the Author

Tiago Antao is a bioinformatician. He is currently studying the genomics of the mosquito Anopheles gambiae, the main vector of malaria. Tiago was originally a computer scientist who crossed over to computational biology with an MSc in bioinformatics from the Faculty of Sciences of the University of Porto, Portugal. He holds a PhD in the spread of drug resistant malaria from the Liverpool School of Tropical Medicine, UK. Tiago is one of the coauthors of Biopython—a major bioinformatics package—written on Python. He has also developed Lositan, a Jython-based selection detection workbench. 

In his postdoctoral career, he has worked with human datasets at the University of Cambridge, UK, and with the mosquito whole genome sequence data at the University of Oxford, UK. He is currently working as a Sir Henry Wellcome fellow at the Liverpool School of Tropical Medicine. 

I would like to take this opportunity to acknowledge everyone at Packt Publishing, especially Gaurav Sharma, my very patient development editor. The quality of this book owes much to the excellent work of the reviewers who provided outstanding comments. Finally, I would like to thank Ana for all that she endured during the writing of this book. 

# About the Reviewers

Cho-Yi Chen is an Olympic swimmer, a bioinformatician, and a computational biologist. He majored in computer science and later devoted himself to biomedical research. Cho-Yi Chen received his MS and PhD degrees in bioinformatics, genomics, and systems biology from National Taiwan University. He was a founding member of the Taiwan Society of Evolution and Computational Biology and is now a postdoctoral research fellow at the Department of Biostatistics and Computational Biology at the Dana-Farber Cancer Institute, Harvard University. As an active scientist and a software developer, Cho-Yi Chen strives to advance our understanding of cancer and other human diseases. 

Giovanni M. Dall'Olio is a bioinformatician with a background in human population genetics and cancer. He maintains a personal blog on bioinformatics tips and best practices at http://bioinfoblog.it. Giovanni was one of the early moderators of Biostar, a Q&A on bioinformatics (http://biostars.org/). He is also a Python enthusiast and was a co-organizer of the Barcelona Python Meetup community for many years. 

After earning a PhD in human population genetics at the Pompeu Fabra University of Barcelona, he moved to King's College London, where he applies his knowledge and programming skills to the study of cancer genetics. He is also responsible for the maintenance of the Network of Cancer Genes (http://ncg.kcl.ac.uk/), a database of system-level properties of genes involved in cancer. 

# www.PacktPub.com

## Support files, eBooks, discount offers, and more

For support files and downloads related to your book, please visit www.PacktPub.com. 

Did you know that Packt offers eBook versions of every book published, with PDF and ePub files available? You can upgrade to the eBook version at www.PacktPub.com and as a print book customer, you are entitled to a discount on the eBook copy. Get in touch with us at service@packtpub.com for more details. 

At www.PacktPub.com, you can also read a collection of free technical articles, sign up for a range of free newsletters and receive exclusive discounts and offers on Packt books and eBooks. 

## TM

https://www2.packtpub.com/books/subscription/packtlib 

Do you need instant solutions to your IT questions? PacktLib is Packt's online digital book library. Here, you can search, access, and read Packt's entire library of books. 

## Why Subscribe?

f Fully searchable across every book published by Pack 

f Copy and paste, print, and bookmark content 

On demand and accessible via a web browser 

## Free Access for Packt account holders

If you have an account with Packt at www.PacktPub.com, you can use this to access PacktLib today and view 9 entirely free books. Simply use your login credentials for immediate access. 

## Table of Contents

Preface
Chapter 1: Python and the Surrounding Software Ecology
Introduction
Installing the required software with Anaconda
Installing the required software with Docker
Interfacing with R via rpy2
Performing R magic with IPython
Chapter 2: Next-generation Sequencing
Introduction
Accessing GenBank and moving around NCBI databases
Performing basic sequence analysis
Working with modern sequence formats
Working with alignment data
Analyzing data in the variant call format
Studying genome accessibility and filtering SNP data
Chapter 3: Working with Genomes
Introduction
Working with high-quality reference genomes
Dealing with low-quality genome references
Traversing genome annotations
Extracting genes from a reference using annotations
Finding orthologues with the Ensembl REST API
Retrieving gene ontology information from Ensembl
Chapter 4: Population Genetics
Introduction
Managing datasets with PLINK
Introducing the Genepop format 

## www.ebook777.comwww.it-ebooks.info

Exploring a dataset with Bio.PopGen 101
Computing F-statistics 107
Performing Principal Components Analysis 113
Investigating population structure with Admixture 118
Chapter 5: Population Genetics Simulation 125
Introduction 125
Introducing forward-time simulations 126
Simulating selection 132
Simulating population structure using island and stepping-stone models 138
Modeling complex demographic scenarios 143
Simulating the coalescent with Biopython and fastsimcoal 149
Chapter 6: Phylogenetics 155
Introduction 155
Preparing the Ebola dataset 156
Aligning genetic and genomic data 162
Comparing sequences 164
Reconstructing phylogenetic trees 170
Playing recursively with trees 174
Visualizing phylogenetic data 179
Chapter 7: Using the Protein Data Bank 187
Introduction 187
Finding a protein in multiple databases 188
Introducing Bio.PDB 192
Extracting more information from a PDB file 197
Computing molecular distances on a PDB file 201
Performing geometric operations 205
Implementing a basic PDB parser 208
Animating with PyMol 212
Parsing mmCIF files using Biopython 220
Chapter 8: Other Topics in Bioinformatics 223
Introduction 223
Accessing the Global Biodiversity Information Facility 224
Geo-referencing GBIF datasets 230
Accessing molecular-interaction databases with PSIQUIC 236
Plotting protein interactions with Cytoscape the hard way 239
Chapter 9: Python for Big Genomics Datasets 247
Introduction 247
Setting the stage for high-performance computing 248
Designing a poor human concurrent executor 254 

Performing parallel computing with IPython 260
Computing the median in a large dataset 266
Optimizing code with Cython and Numba 271
Programming with laziness 275
Thinking with generators 278
Index 281 

Table of Contents 

Free ebooks ==> www.ebook777.com 

## Preface

Whether you are reading this book as a computational biologist or a Python programmer, you will probably relate to the "explosive growth, exciting times" expression. The recent growth of Python is strongly connected with its status as the main programming language for big data. On the other hand, the deluge of data in biology, mostly from genomics and proteomics makes bioinformatics one of the forefront applications of data science. There is a massive need for bioinformaticians to analyze all this data; of course, one of the main tools is Python. We will not only talk about the programming language, but also the whole community and software ecology behind it. When you choose Python to analyze your data, you will also get an extensive set of libraries, ranging from statistical analysis to plotting, parallel programming, machine learning, and bioinformatics. However, when you choose Python, you expect more than this; the community has a tradition of providing good documentation, reliable libraries, and frameworks. It is also friendly and supportive of all its participants. 

In this book, we will present practical solutions to modern bioinformatics problems using Python. Our approach will be hands-on, where we will address important topics, such as next-generation sequencing, genomics, population genetics, phylogenetics, and proteomics among others. At this stage, you probably know the language reasonably well and are aware of the basic analysis methods in your field of research. You will dive directly into relevant complex computational biology problems and learn how to tackle them with Python. This is not your first Python book or your first biology lesson; this is where you will find reliable and pragmatic solutions to realistic and complex problems. 

## What this book covers

Chapter 1, Python and the Surrounding Software Ecology, tells you how to set up a modern bioinformatics environment with Python. This chapter discusses how to deploy software using Docker, interface with R, and interact with the IPython Notebook. 

Chapter 2, Next-generation Sequencing, provides concrete solutions to deal with next-generation sequencing data. This chapter teaches you how to deal with large FASTQ, BAM, and VCF files. It also discusses data filtering. 

## Preface

Chapter 3, Working with Genomes, not only deals with high-quality references—such as the human genome—but also discusses how to analyze other low-quality references typical in non-model species. It introduces GFF processing, teaches you how to analyze genomic feature information, and discusses how to use gene ontologies. 

Chapter 4, Population Genetics, describes how to perform population genetics analysis of empirical datasets. For example, on Python, we will perform Principal Components Analysis, compute $\mathsf { F } _ { \mathsf { s } \mathsf { T } ^ { \prime } }$ or Structure/Admixture plots. 

Chapter 5, Population Genetics Simulation, covers simuPOP, an extremely powerful Python-based forward-time population genetics simulator. This chapter shows you how to simulate different selection and demographic regimes. It also briefly discusses the coalescent simulation. 

Chapter 6, Phylogenetics, uses complete sequences of recently sequenced Ebola viruses to perform real phylogenetic analysis, which includes tree reconstruction and sequence comparisons. This chapter discusses recursive algorithms to process tree-like structures. 

Chapter 7, Using the Protein Data Bank, focuses on processing PDB files, for example, performing the geometric analysis of proteins. This chapter takes a look at protein visualization. 

Chapter 8, Other Topics in Bioinformatics, talks about how to analyze data made available by the Global Biodiversity Information Facility (GBIF) and how to use Cytoscape, a powerful platform to visualize complex networks. This chapter also looks at how to work with geo-referenced data and map-based services. 

Chapter 9, Python for Big Genomics Datasets, discusses high-performance programming techniques necessary to handle big datasets. It briefly discusses cluster usage and code optimization platforms (such as Numba or Cython). 

## What you need for this book

Modern bioinformatics analysis is normally performed on a Linux server. Most of our recipes will also work on Mac OS X. It will also work on Windows in theory, but this is not recommended. If you do not have a Linux server, you can use a free virtual machine emulator such as VirtualBox to run it on a Windows/Mac computer. An alternative that we explore in the book is to use Docker as a container, which can be used on Windows and Mac via boot2docker. 

As modern bioinformatics is a big data discipline, you will need a reasonable amount of memory; at least 4 GB on a native Linux machine, probably 8 GB on a Mac/Windows system, but more would be better. A broadband Internet connection will also be necessary to download the real and hands-on datasets used in the book. 

## Preface

Python is a requirement. All the code will work with version 2, although you are highly encouraged to use version 3 whenever possible. Many free Python libraries will also be required and these will be presented in the book. Biopython, NumPy, SciPy, and Matplotlib are used in almost all chapters. Although the IPython Notebook is not strictly required, it's highly encouraged. Different chapters will also require various bioinformatics tools. All the tools used in the book are freely available and thorough instructions are provided in the relevant chapters of this book. 

## Who this book is for

If you have intermediate-level knowledge of Python and are well aware of the main research and vocabulary in your bioinformatics topic of interest, this book will help you develop your knowledge further. 

## Sections

In this book, you will find several headings that appear frequently. To give clear instructions on how to complete a recipe, we use these sections as follows: 

## Getting ready

This section tells you what to expect in the recipe, and describes how to set up any software or any preliminary settings required for the recipe. 

## How to do it…

This section contains the steps required to follow the recipe. 

## There's more…

This section consists of additional information about the recipe in order to make the reader more knowledgeable about the recipe. 

## See also

This section provides helpful links to other useful information for the recipe. 

![image](images/fig_002.jpg)


Preface 

## Conventions

In this book, you will find a number of text styles that distinguish between different kinds of information. Here are some examples of these styles and an explanation of their meaning. 

Code words in text, filenames, file extensions, URLs, user input, are shown as follows: "If you are using notebooks, open the 00_Intro/Interfacing_R notebook.ipynb and just execute the wget command on top." 

A block of code is set as follows: 

```python
from collections import OrderedDict
import simuPOP as sp
pop_size = 100
pops = sp.Population(pop_size, loci=[1] * num_loci)
num_loci = 10
init_ops['Freq'] = sp.InitGenotype(freq=[0.5, 0.5]) 
```

Any command-line input or output is written as follows: 

conda create -n bioinformatics biopython=1.65 python=3.4 

New terms and important words are shown in bold. Words that you see on the screen, for example, in menus or dialog boxes, appear in the text like this: "The top line is without migration, the middle line with 0.005 migration and the bottom line with 0.1." 

![image](images/fig_003.jpg)


Warnings or important notes appear in a box like this. 

![image](images/fig_004.jpg)


Tips and tricks appear like this. 

## Reader feedback

Feedback from our readers is always welcome. Let us know what you think about this book— what you liked or disliked. Reader feedback is important for us as it helps us develop titles that you will really get the most out of. 

To send us general feedback, simply e-mail feedback@packtpub.com, and mention the book's title in the subject of your message. 

If there is a topic that you have expertise in and you are interested in either writing or contributing to a book, see our author guide at www.packtpub.com/authors. 

![image](images/fig_005.jpg)


Preface 

## Customer support

Now that you are the proud owner of a Packt book, we have a number of things to help you to get the most from your purchase. 

## Downloading the example code

You can download the example code files from your account at http://www.packtpub.com for all the Packt Publishing books you have purchased. If you purchased this book elsewhere, you can visit http://www.packtpub.com/support and register to have the files e-mailed directly to you. 

## Downloading the color images of this book

We also provide you with a PDF file that has color images of the screenshots/diagrams used in this book. The color images will help you better understand the changes in the output. You can download this file from: http://www.packtpub.com/sites/default/files/ downloads/5117OS_ColoredImages.pdf 

## Errata

Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you find a mistake in one of our books—maybe a mistake in the text or the code—we would be grateful if you could report this to us. By doing so, you can save other readers from frustration and help us improve subsequent versions of this book. If you find any errata, please report them by visiting http://www.packtpub.com/submit-errata, selecting your book, clicking on the Errata Submission Form link, and entering the details of your errata. Once your errata are verified, your submission will be accepted and the errata will be uploaded to our website or added to any list of existing errata under the Errata section of that title. 

To view the previously submitted errata, go to https://www.packtpub.com/books/ content/support and enter the name of the book in the search field. The required information will appear under the Errata section. 

## Piracy

Piracy of copyrighted material on the Internet is an ongoing problem across all media. At Packt, we take the protection of our copyright and licenses very seriously. If you come across any illegal copies of our works in any form on the Internet, please provide us with the location address or website name immediately so that we can pursue a remedy. 

## Preface

Please contact us at copyright@packtpub.com with a link to the suspected pirated material. 

We appreciate your help in protecting our authors and our ability to bring you valuable content. 

## Questions

If you have a problem with any aspect of this book, you can contact us at questions@ packtpub.com, and we will do our best to address the problem. 

![image](images/fig_006.jpg)