# *Zea Mays* SNP Calling 
```

We sequenced three lines of *zea mays*, using paired-end sequencing. This sequencing was done by our sequencing core and we received the data on 2013-05-10. Each variety should have **two** sequences files, with suffixes `_R1.fastq` and `_R2.fastq`, indicating which member of the pair it is. 

## ## Sequencing Files

## All raw FASTQ sequences are in `data/seqs/`:

```txt
$ find data/seqs -name "*.fastq"
data/seqs/zmaysA_R1.fastq
data/seqs/zmaysA_R2.fastq
data/seqs/zmaysB_R1.fastq
data/seqs/zmaysB_R2.fastq
data/seqs/zmaysC_R1.fastq
data/seqs/zmaysC_R2.fastq 
```

```markdown
## Quality Control Steps 
```

After the sequencing data was received, our first stage of analysis was to ensure the sequences were high quality. We ran each of the three lines' two paired-end FASTQ files through a quality diagnostic and control pipeline. Our planned pipeline is: 

1. Create base quality diagnostic graphs. 

2. Check reads for adapter sequences. 

3. Trim adapter sequences. 

4. Trim poor quality bases. 

Recommended trimming programs: 

- Trimmomatic 

- Scythe 

Figure 2-1 shows this example Markdown notebook rendered in HTML5 by Pandoc. What makes Markdown a great format for lab notebooks is that it’s as easy to read in unrendered plain-text as it is in rendered HTML. Next, let’s take a look at the Mark‐ down syntax used in this example (see Table 2-2 for a reference). 

```txt
Zea Mays SNP Calling
We sequenced three lines of zea mays, using paired-end sequencing. This sequencing was done by our sequencing core and we received the data on 2013-05-10. Each variety should have two sequences files, with suffixes _R1.fastq and _R2.fastq, indicating which member of the pair it is.
Sequencing Files
All raw FASTQ sequences are in data/seqs/:
$ find data/seqs -name "*.fastq"
data/seqs/zmaysA_R1.fastq
data/seqs/zmaysA_R2.fastq
data/seqs/zmaysB_R1.fastq
data/seqs/zmaysB_R2.fastq
data/seqs/zmaysC_R1.fastq
data/seqs/zmaysC_R2.fastq
Quality Control Steps
After the sequencing data was received, our first stage of analysis was to ensure the sequences were high quality. We ran each of the three lines' two paired-end FASTQ files through a quality diagnostic and control pipeline. Our planned pipeline is:
1. Create base quality diagnostic graphs.
2. Check reads for adapter sequences.
3. Trim adapter sequences.
4. Trim poor quality bases.
Recommended trimming programs:
• Trimmomatic
• Scythe 
```


Figure 2-1. HTML Rendering of the Markdown notebook



Table 2-2. Minimal Markdown inline syntax


<table><tr><td>Markdown syntax</td><td>Result</td></tr><tr><td>*emphasis*</td><td>emphasis</td></tr><tr><td>**bold**</td><td>bold</td></tr><tr><td>`inline code`</td><td>inline code</td></tr><tr><td></td><td>Hyperlink to http://website.com/link</td></tr><tr><td>[link text](http://website.com/link)</td><td>Hyperlink to http://website.com/link, with text &quot;link text&quot;</td></tr></table>

```txt
Markdown syntax Result
![alt text](path/to/figure.png) Image, with alternative text "alt text" 
```

Block elements like headers, lists, and code blocks are simple. Headers of different levels can be specified with varying numbers of hashes (#). For example: 

```txt
# Header level 1
## Header level 2
### Header level 3 
```

Markdown supports headers up to six levels deep. It’s also possible to use an alterna‐ tive syntax for headers up to two levels: 

```txt
Header level 1
Header level 2 
```

Both ordered and unordered lists are easy too. For unordered lists, use dashes, aster‐ isks, or pluses as list element markers. For example, using dashes: 

```txt
- Francis Crick
- James D. Watson
- Rosalind Franklin 
```

To order your list, simply use numbers (i.e., 1., 2., 3., etc.). The ordering doesn’t matter, as HTML will increment these automatically for you. However, it’s still a good idea to clearly number your bullet points so the plain-text version is readable. 

Code blocks are simple too—simply add four spaces or one tab before each code line: 

I ran the following command: 

```txt
$ find seqs/ -name "*.fastq" 
```

If you’re placing a code block within a list item, make this eight spaces, or two tabs: 

1. I searched for all FASTQ files using: 

find seqs/ -name "*.fastq" 

2. And finally, VCF files with: 

```txt
find vcf/ -name "*.vcf" 
```

What we’ve covered here should be more than enough to get you started digitally documenting your bioinformatics work. Additionally, there are extensions to Mark‐ down, such as MultiMarkdown and GitHub Flavored Markdown. These variations add features (e.g., MultiMarkdown adds tables, footnotes, LaTeX math support, etc.) 

and change some of the default rendering options. There are also many specialized Markdown editing applications if you prefer a GUI-based editor (see this chapter’s README on GitHub for some suggestions). 

## Using Pandoc to Render Markdown to HTML

We’ll use Pandoc, a popular document converter, to render our Markdown docu‐ ments to valid HTML. These HTML files can then be shared with collaborators or hosted on a website. See the Pandoc installation page for instructions on how to install Pandoc on your system. 

Pandoc can convert between a variety of different markup and output formats. Using Pandoc is very simple—to convert from Markdown to HTML, use the --from mark down and --to html options and supply your input file as the last argument: 

$ pandoc --from markdown --to html notebook.md > output.html 

By default, Pandoc writes output to standard out, which we can redirect to a file (we’ll learn more about standard out and redirection in Chapter 3). We could also specify the output file using --output output.html. Finally, note that Pandoc can convert between many formats, not just Markdown to HTML. I’ve included some more examples of this in this chapter’s README on GitHub, including how to convert from HTML to PDF.