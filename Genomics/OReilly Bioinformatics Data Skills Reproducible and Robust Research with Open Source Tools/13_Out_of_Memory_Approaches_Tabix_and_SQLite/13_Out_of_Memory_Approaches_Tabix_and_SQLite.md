# Out-of-Memory Approaches: Tabix and SQLite

In this chapter, we’ll look at out-of-memory approaches—computational strategies built around storing and working with data kept out of memory on the disk. Reading data from a disk is much, much slower than working with data in memory (see “The Almighty Unix Pipe: Speed and Beauty in One” on page 45), but in many cases this is the approach we have to take when in-memory (e.g., loading the entire dataset into R) or streaming approaches (e.g., using Unix pipes, as we did in Chapter 7) aren’t appropriate. Specifically, we’ll look at two tools to work with data out of memory: Tabix and SQLite databases. 

## Fast Access to Indexed Tab-Delimited Files with BGZF and Tabix

BGZF and Tabix solve a really important problem in genomics: we often need fast read-only random access to data linked to a genomic location or range. For the scale of data we encounter in genomics, retrieving this type of data is not trivial for a few reasons. First, the data may not fit entirely in memory, requiring an approach where data is kept out of memory (in other words, on a slow disk). Second, even powerful relational database systems can be sluggish when querying out millions of entries that overlap a specific region—an incredibly common operation in genomics. The tools we’ll see in this section are specially designed to get around these limitations, allowing fast random-access of tab-delimited genome position data. 

In chapter on alignment, we saw how sorted and indexed BAM files allow us to quickly access alignments from within a particular region. The technology that allows fast random access of alignments within a region is based on an ingenious compres‐ sion format called BGZF (Blocked GNU Zip Format), which uses the GNU Zip (gzip) compression format we first saw in “gzip” on page 119. However, while gzip compresses the entire file, BGZF files are compressed in blocks. These blocks provide BGZF files with a useful feature that gzip-compressed files don’t have: we can jump to and decompress a specific block without having to decompress the entire file. Block compression combined with file indexing is what enables fast random access of alignments from large BAM files with samtools view. In this section, we’ll utilize BGZF compression and a command-line tool called Tabix to provide fast random access to a variety of tab-delimited genomic formats, including GFF, BED, and VCF. 

To use Tabix to quickly retrieve rows that overlap a query region, we first must pre‐ pare our file containing the genomic regions. We can prepare a file using the follow‐ ing steps: 

1. Sort the file by chromosome and start position, using Unix sort. 

2. Use the bgzip command-line tool to compress the file using BGZF. 

3. Index the file with tabix, specifying which format the data is in (or by manually specifying which columns contain the chromosome, start, and end positions). 

Before getting started, check that you have the bgzip and tabix programs installed. These are both included with Samtools, and should already be installed if you’ve worked through Chapter 11. We’ll use both bgzip and tabix throughout this section. 

## Compressing Files for Tabix with Bgzip

Let’s use the gzipped Mus_musculus.GRCm38.75.gtf.gz file from this chapter’s direc‐ tory in the GitHub repository in our Tabix examples. First, note that this file is com‐ pressed using gzip, not BGZF. To prepare this file, we first need to unzip it, sort by chromosome and start position, and then compress it with bgzip. We can transform this gzipped GTF file into a sorted BGZF-compressed file using a single line built from piped commands. 

Unfortunately, we have to deal with one minor complication before calling sort—this GTF file has a metadata header at the top of the file: 

```powershell
$ gzcat Mus_musculus.GRCm38.75.gtf.gz | head -n5
#!genome-build GRCm38.p2
#!genome-version GRCm38
#!genome-date 2012-01
#!genome-build-accession NCBI:GCA_000001635.4
#!genebuild-last-updated 2013-09 
```

We’ll get around this using the subshell trick we learned in “Subshells” on page 169, substituting the gzip step with bgzip: 

```shell
$(zgrep "^#" Mus_musculus.GRCm38.75.gtf.gz; \
zgrep -v "^#" Mus_musculus.GRCm38.75.gtf.gz | \
sort -k1,1 -k4,4n) | bgzip > Mus_musculus.GRCm38.75.gtf.bgz 
```

Remember, subshells are interpreted in a separate shell. All standard output produced from each sequential command is sent to bgzip, in the order it’s produced. In the pre‐ ceding example, this has the effect of outputting the metadata header (lines that start with #), and then outputting all nonheader lines sorted by the first and fourth col‐ umns to bgzip. The end result is a bzip-compressed, sorted GTF file we can now index with tabix. Subshells are indeed a bit tricky—if you forget the specifics of how to use them to bgzip files with headers, the tabix man page (man tabix) has an example. 

## Indexing Files with Tabix

Once our tab-delimited data file (with data linked to genomic ranges) is compressed with BGZF, we can use the tabix command-line tool to index it. Indexing files with tabix is simple for files in standard bioinformatics formats—tabix has preset options for GTF/GFF, BED, SAM, VCF, and PSL (a tab-delimited format usually gen‐ erated from BLAT). We can index a file in these formats using the -p argument. So, we index a GTF file that’s been compressed with bgzip (remember, files must be sor‐ ted and compressed with BGZF!) with: 

$ tabix -p gff Mus_musculus.GRCm38.75.gtf.bgz 

Note that Tabix created a new index file, ending with the suffix .tbi: 

```txt
$ ls *tbi
Mus_musculus.GRCm38.75.gtf.bgz.tbi 
```

Tabix will also work with custom tab-delimited formats—we just need to specify the columns that contain the chromosome, start, and end positions. Run tabix without any arguments to see its help page for more information (but in general, it’s better to stick to an existing bioinformatics format whenever you can). 

## Using Tabix

Once our tab-delimited file is indexed with Tabix, we can make fast random queries with the same tabix command we used to index the file. For example, to access fea‐ tures in Mus_musculus.GRCm38.75.gtf.bgz on chromosome 16, from 23,146,536 to 23,158,028, we use: 

```tcl
$ tabix Mus_musculus.GRCm38.75.gtf.bgz 16:23146536-23158028 | head -n3
16 protein_coding UTR 23146536 23146641 [...] 
16 protein_coding exon 23146536 23146641 [...] 
16 protein_coding gene 23146536 23158028 [...] 
```

From here, we could redirect these results to a file or use them directly in a stream. For example, we might want to pipe these results to awk and extract rows with the feature column “exon” in this region: 

```txt
$ tabix Mus_musculus.GRCm38.75.gtf.bgz 16:23146536-23158028 | \
awk '$3 ~ /exon/ {print}'
16 protein_coding exon 23146536 23146641 [...] 
16 protein_coding exon 23146646 23146734 [...] 
16 protein_coding exon 23155217 23155447 [...] 
16 protein_coding exon 23155217 23155447 [...] 
16 protein_coding exon 23157074 23157292 [...] 
16 protein_coding exon 23157074 23158028 [...] 
```

A nice feature of Tabix is that it works across an HTTP or FTP server. Once you’ve sorted, bgzipped, and indexed a file with tabix, you can host the file on a shared server so others in your group can work with it remotely. Sharing files this way is out‐ side the scope of this book, but it’s worth noting Tabix supports this possibility. 

## Introducing Relational Databases Through SQLite

Many standard bioinformatics data formats (GTF/GFF, BED, VCF/BCF, SAM/BAM) we’ve encountered so far store tabular data in single fat fle formats. Flat file formats don’t have any internal hierarchy or structured relationship with other tables. While we’re able to join tables using Unix tools like join, and R’s match() and merge() functions, the flat files themselves do not encode any relationships between tables. Flat file formats are widely used in bioinformatics because they’re simple, flexible, and portable, but occasionally we do need to store and manipulate data that is best repre‐ sented in many related tables—this is where relational databases are useful. 

Unlike flat files, relational databases can contain multiple tables. Relational databases also support methods that use relationships between tables to join and extract specific records using specialized queries. The most common language for querying relational databases is SQL (Structured Query Language), which we’ll use throughout the remainder of this chapter. 

In this section, we’ll learn about a relational database management system (the soft‐ ware that implements a relational database, also known as RDBMS) called SQLite. SQLite is an entirely self-contained database system that runs as a single process on your machine. We’ll use SQLite in this section because it doesn’t require any setup— you can create a database and start making queries with minimal time spent on con‐ figuring and administrating your database. In contrast, other database systems like MySQL and PostgreSQL require extensive configuration just to get them up and run‐ ning. While SQLite is not as powerful as these larger database systems, it works sur‐ prisingly well for databases in the gigabytes range. In cases when we do need a relational database management system in bioinformatics (less often than you’d think, as we’ll see in the next section), SQLite performs well. I like to describe SQLite as hav‐ ing the highest ratio of power and usability to setup cost of any relational database system. 

## When to Use Relational Databases in Bioinformatics

As it turns out, you’ll need to directly interact with relational databases less often than you might think, for two reasons. First, many large bioinformatics databases provide user-friendly application programming interfaces (known more commonly by their acronym, API) for accessing data. It’s often more convenient to access data through an API than by interacting with databases directly. This is because APIs allow you to interact with higher-level applications so you don’t have to worry about the gritty details of how the database is structured to get information out. For example, both Ensembl and Entrez Global Query have APIs that simplify accessing data from these resources. 

Second, for many bioinformatics tasks, using a relational database often isn’t the best solution. Relational databases are designed for storing and managing multiple records in many tables, where users will often need to add new records, update exist‐ ing records, and extract specific records using queries. To get an idea of when rela‐ tional databases are a suitable choice, let’s compare working with two different types of data: a curated set of gene models and a set of alignment data from a sequencing experiment. 

## Adding and Merging Data

Suppose we need to merge a set of new gene models with an existing database of gene models. Simply concatenating the new gene models to the database of exist‐ ing gene models isn’t appropriate, as we could end up with duplicate exons, gene identifiers, gene names, and the like. These duplicate entries would lead to clut‐ tered queries in the future (e.g., fetching all exons from a certain gene would return duplicate exons). With datasets like these, we need a structured way to query existing records, and only add gene models that aren’t already in the data‐ set. Relational databases provide data insertion methods that simplify keeping consistent relationships among data. 

In contrast, when we merge two alignment files, we don’t have to maintain any consistency or structure—each alignment is essentially an observation in an experiment. To merge two alignment files, we can simply concatenate two align‐ ment files together (though with proper tools like samtools merge and attention to issues like the RG dictionary in the header). Additionally, we don’t have to pay attention to details like duplicates during the merge step. Duplicate alignments created by technical steps like PCR are best removed using specialized programs designed to remove duplicates. Overall, the nature of alignment data obviates the need for advanced data merging operations. 

## Updating Data

Suppose now that a collaborator has run a new version of gene finding software. After some data processing, you produce a list of gene models that differ from your original gene models. After some careful checking, it’s decided that the new gene models are superior, and you need to update all original gene models with the new models’ coordinates. As with merging data, these types of update opera‐ tions are much simpler to do using a relational database system. SQL provides an expressive syntax for updating existing data. Additionally, relational databases simplify updating relationships between data. If an updated gene model includes two new exons, these can be added to a relational database without modifying additional data. Relational databases also allow for different versions of databases and tables, which allows you to safely archive past gene models (good for repro‐ ducibility!). 

In contrast, consider that any updates we make to alignment data: 

• Are never made in place (changing the original dataset) 

• Usually affect all alignments, not a specific subset 

• Do not require sophisticated queries 

As a result, any updates we need to make to alignment data are made with speci‐ alized tools that usually operate on streams of data (which allows them to be included in processing pipelines). Using tools designed specifically to update cer‐ tain alignment attributes will always be faster than trying to store alignments in a database and update all entries. 

## Querying data

Querying information from a set of gene models can become quite involved. For example, imagine you had to write a script to find the first exon for all transcripts for all genes in a given set of gene identifiers. This wouldn’t be too difficult, but writing separate scripts for additional queries would be tedious. If you were working with a large set of gene models, searching each gene model for a match‐ ing identifier would be unnecessarily slow. Again, relational databases streamline these types of queries. First, SQL acts as a language you can use to specify any type of query. Second, unlike Python and R, SQL is a declarative language, mean‐ ing you state what you want, not how to get it (the RDBMS implements this). This means you don’t have to implement the specific code that retrieves gene models matching your query. Last, RDBMS allow you to index certain columns, which can substantially speed up querying data. 

On the other hand, the queries made to alignment data are often much simpler. The most common query is to extract alignments from a specific region, and as we saw in Chapter 11, this can be easily accomplished with samtools view. Querying alignments within a region in a position-sorted and indexed BAM file is much faster than loading all alignments into a database and extracting align‐ ments with SQL. Additionally, sorted BAM files are much more space efficient and other than a small index file, there’s no additional overhead in indexing alignments for fast retrieval. For other types of queries, we can use algorithms that stream through an entire BAM file. 

This comparison illustrates the types of applications where a relational database would be most suitable. Overall, databases are not appropriate for raw experimental data, which rarely needs to be merged or modified in a way not possible with a streaming algorithm. For these activities, specialized bioinformatics formats (such as SAM/BAM, VCF/VCF) and tools to work with these formats will be more computa‐ tionally efficient. Relational databases are better suited for smaller, refined datasets where relationships between records are easily represented and can be utilized while querying out data. 

## Installing SQLite

Unlike MySQL and PostgreSQL, SQLite doesn’t require separate server and client programs. You can install on OS X with Homebrew, using brew install sqlite, or on an Ubuntu machine with apt-get using apt-get install sqlite3. The SQLite website also has source code you can download, compile, and install yourself (but it’s preferable to just use your ports/packages manager). 

## Exploring SQLite Databases with the Command-Line Interface

We’ll learn the basics of SQL through SQLite, focusing on how to build powerful queries and use joins that take advantage of the relational structure of databases. We’ll start off with the gwascat.db example database included in this chapter’s directory in the book’s GitHub repository. This is a SQLite database containing a table of the National Human Genome Research Institute’s catalog of published Genome-Wide Association Studies (GWAS) with some modifications (Welter et al., 2014). In later examples, we’ll use another database using a different relational structure of this data. The code and documentation on how these databases were created is included in the same directory in the GitHub repository. The process of structuring, creating, and managing a database is outside the scope of this chapter, but I encourage you to refer to Jay A. Kreibich’s book Using SQLite and Anthony Molinaro’s SQL Cookbook. 

Each SQLite database is stored in its own file (as explained on the SQLite website). We can connect to our example database, the gwascat.db file, using SQLite’s command-line tool sqlite3. This leaves you with an interactive SQLite prompt: 

```txt
Enter SQL statements terminated with a ";"
sqlite> 
```

We can issue commands to the SQLite program (not the databases it contains) using dot commands, which start with a . (and must not begin with whitespace). For exam‐ ple, to list the tables in our SQLite database, we use .tables (additional dot com‐ mands are listed in Table 13-1): 

sqlite> .tables gwascat 


Table 13-1. Useful SQLite3 dot commands


<table><tr><td>Command</td><td>Description</td></tr><tr><td>.exit, .quit</td><td>Exit SQLite</td></tr><tr><td>.help</td><td>List all SQLite dot commands</td></tr><tr><td>.tables</td><td>Print a list of all tables</td></tr><tr><td>.schema</td><td>Print the table schema (the SQL statement used to create a table)</td></tr><tr><td>.headers</td><td>Turn column headers on, off</td></tr><tr><td>.import</td><td>Import a file into SQLite (see SQLite&#x27;s documentation for more information)</td></tr><tr><td>.indices</td><td>Show column indices (see “Indexing” on page 457)</td></tr><tr><td>.mode</td><td>Set output mode (e.g., csv, column, tabs, etc.; see .help for a full list)</td></tr><tr><td>.read</td><td>Execute SQL from file</td></tr></table>

From .tables, we know this database contains one table: gwascat. Our next step in working with this table is understanding its columns and the recommended type used to store data in that column. In database lingo, we call the structure of a database and its tables (including their columns, preferred types, and relationships between tables) the schema. Note that it’s vital to understand a database’s schema before query‐ ing data (and even more so when inserting data). In bioinformatics, we often run into the situation where we need to interact with a remote public SQL database, often with very complex schema. Missing important schema details can lead to incorrectly struc‐ tured queries and erroneous results. 

In SQLite, the .schema dot command prints the original SQL CREATE statements used to create the tables: 

sqlite> .schema 

CREATE TABLE gwascat( 

```txt
id integer PRIMARY KEY NOT NULL,
dbdate text,
pubmedid integer,
author text,
date text,
journal text,
link text,
study text,
trait text,
[...] 
```

Don’t worry too much about the specifics of the CREATE statement—we’ll cover this later. More importantly, .schema provides a list of all columns and their preferred type. Before going any further, it’s important to note an important feature of SQLite: columns do not have types, data values have types. This is a bit like a spreadsheet: while a column of height measurements should all be real-valued numbers (e.g., 63.4, 59.4, etc.), there’s no strict rule mandating this is the case. In SQLite, data values are one of five types: 

• text 

• integer 

• real 

• NULL, used for missing data, or no value 

• BLOB, which stands for binary large object, and stores any type of object as bytes 

But again, SQLite’s table columns do not enforce all data must be the same type. This makes SQLite unlike other database systems such as MySQL and PostgreSQL, which have strict column types. Instead, each column of a SQLite table has a preferred type called a type afnity. When data is inserted into a SQLite table, SQLite will try to coerce this data into the preferred type. Much like R, SQLite follows a set of coercion rules that don’t lead to a loss of information. We’ll talk more about the different SQLite type affinities when we discuss creating tables in “Creating tables” on page 455. 

![image](images/fig_001.jpg)


## Orderly Columns

Despite SQLite’s column type leniency, it’s best to try to keep data stored in the same column all the same type. Keeping orderly col‐ umns makes downstream processing much easier because you won’t need to worry about whether the data in a column is all dif‐ ferent types. 

With the skills to list tables with .tables and peek at their schema using .schema, we’re ready to start interacting with data held in tables using the SQL SELECT com‐ mand. 

## Querying Out Data: The Almighty SELECT Command

Querying out data using the SQL SELECT command is one of the most important data skills you can learn. Even if you seldom use relational database systems, the reasoning behind querying data using SQL is applicable to many other problems. Also, while we’ll use our example gwascat.db database in these examples, the SELECT syntax is fairly consistent among other relational database management systems. Conse‐ quently, you’ll be able to apply these same querying techniques when working with public MySQL biological databases like UCSC’s Genome Browser and Ensembl data‐ bases. 

In its most basic form, the SELECT statement simply fetches rows from a table. The most basic SELECT query grabs all rows from all columns from a table: 

```txt
sqlite> SELECT * FROM gwascat;
id|dbdate|pubmedid|author|date|journal| [...]
1|08/02/2014|24388013|Ferreira MA|12/30/2013|J Allergy Clin Immunol| [...]
2|08/02/2014|24388013|Ferreira MA|12/30/2013|J Allergy Clin Immunol| [...]
3|08/02/2014|24388013|Ferreira MA|12/30/2013|J Allergy Clin Immunol| [...]
4|08/02/2014|24388013|Ferreira MA|12/30/2013|J Allergy Clin Immunol| [...]
[...] 
```

The syntax of this simple SELECT statement is SELECT <columns> FROM <tablename>. Here, the asterisk (*) denotes that we want to select all columns in the table specified by FROM (in this case, gwascat). Note that SQL statements must end with a semicolon. 

![image](images/fig_002.jpg)


## Working with the SQLite Command-Line Tool

If you make a mistake entering a SQLite command, it can be frus‐ trating to cancel it and start again. With text entered, sqlite3 won’t obey exiting with Control-c;—the sqlite3 command-line client isn’t user-friendly in this regard. The best way around this is to use Control-u, which clears all input to the beginning of the line. If your input has already carried over to the next line, the only sol‐ ution is a hack—you’ll need to close any open quotes, enter a syn‐ tax error so your command won’t run, finish your statement with ;, and run it so sqlite3 errors out. 

The sqlite3 command-line tool can also take queries directly from the command line (rather than in the interactive SQLite shell). This is especially useful when writing processing pipelines that need to execute a SQL statement to retrieve data. For exam‐ ple, if we wanted to retrieve all data in the gwascat table: 

```powershell
$ sqlite3 gwascat.db "SELECT * FROM gwascat" > results.txt
$ head -n 1 results.txt
1|2014-08-02|24388013|Ferreira MA|2013-12-30|J Allergy Clin Immunol| [...] 
```

The sqlite3 program also has options that allow us to change how these results are returned. The option -separator can be used to specify how columns are separated (e.g., "," for CSV or "\t" for tab) and -header and -noheader can be used to display or omit the column headers. 

## Limiting results with LIMIT

To avoid printing all 17,290 rows of the gwascat table, we can add a LIMIT clause to this SELECT statement. LIMIT is an optional clause that limits the number of rows that are fetched. Like the Unix command head, LIMIT can be used to take a peek at a small subset of the data: 

```txt
sqlite> SELECT * FROM gwascat LIMIT 2;
id|dbdate|pubmedid|author|date|journal| [...] 
1|08/02/2014|24388013|Ferreira MA|12/30/2013|J Allergy Clin Immunol| [...] 
2|08/02/2014|24388013|Ferreira MA|12/30/2013|J Allergy Clin Immunol| [...] 
```

Unlike the rows in a file, the order in which rows of a table are stored on disk is not guaranteed, and could change even if the data stays the same. You could execute the same SELECT query, but the order of the resulting rows may be entirely different. This is an important characteristic of relational databases, and it’s important that how you process the results from a SELECT query doesn’t depend on row ordering. Later on, we’ll see how to use an ORDER BY clause to order by one or more columns. 

## Selecting columns with SELECT

There are 36 columns in the gwascat table, far too many to print on a page without wrapping (which is why I’ve cropped the previous example). Rather than selecting all columns, we can specify a comma-separated subset we care about: 

```sql
sqlite> SELECT trait, chrom, position, strongest_risk_snp, pvalue
...> FROM gwascat LIMIT 5;
trait|chrom|position|strongest_risk_snp|pvalue
Asthma and hay fever|6|32658824|rs9273373|4.0e-14
Asthma and hay fever|4|38798089|rs4833095|5.0e-12
Asthma and hay fever|5|111131801|rs1438673|3.0e-11
Asthma and hay fever|2|102350089|rs10197862|4.0e-11
Asthma and hay fever|17|39966427|rs7212938|4.0e-10 
```

When we’re selecting only a few columns, SQLite’s default list output mode isn’t very clear. We can adjust SQLite’s settings so results are a bit clearer: 

```txt
sqlite> .header on
sqlite> .mode column 
```

Now, displaying the results is a bit easier: 

```sql
sqlite> SELECT trait, chrom, position, strongest_risk_snp, pvalue
...> FROM gwascat LIMIT 5; 
```

<table><tr><td>trait</td><td>chrom</td><td>position</td><td>strongest_risk_snp</td><td>pvalue</td></tr><tr><td>Asthma and hay fever</td><td>6</td><td>32658824</td><td>rs9273373</td><td>4.0e-14</td></tr><tr><td>Asthma and hay fever</td><td>4</td><td>38798089</td><td>rs4833095</td><td>5.0e-12</td></tr><tr><td>Asthma and hay fever</td><td>5</td><td>111131801</td><td>rs1438673</td><td>3.0e-11</td></tr><tr><td>Asthma and hay fever</td><td>2</td><td>102350089</td><td>rs10197862</td><td>4.0e-11</td></tr><tr><td>Asthma and hay fever</td><td>17</td><td>39966427</td><td>rs7212938</td><td>4.0e-10</td></tr></table>

In cases where there are many columns, SQLite’s list mode is usually clearer (you can turn this back on with .mode list). 

## Ordering rows with ORDER BY

As mentioned earlier, the rows returned by SELECT are not ordered. Often we want to get order by a particular column. For example, we could look at columns related to the study, ordering by author’s last name: 

```txt
sqlite> SELECT author, trait, journal
...> FROM gwascat ORDER BY author LIMIT 5;
author trait journal
Aberg K Antipsychotic-induced QTc interval prolongation Pharmacogenomics J
Aberg K Antipsychotic-induced QTc interval prolongation Pharmacogenomics J
Aberg K Antipsychotic-induced QTc interval prolongation Pharmacogenomics J
Aberg K Response to antipsychotic therapy (extrapyramid Biol Psychiatry
Aberg K Response to antipsychotic therapy (extrapyramid Biol Psychiatry 
```

To return results in descending order, add DESC to the ORDER BY clause: 

```txt
sqlite> SELECT author, trait, journal
...> FROM gwascat ORDER BY author DESC LIMIT 5;
author trait journal
van der Zanden LF Hypospadias Nat Genet
van der Valk RJ Fractional J Allergy
van der Valk RJ Fractional J Allergy
van der Valk RJ Fractional J Allergy
van der Valk RJ Fractional J Allergy 
```

Remember, SQLite’s columns do not have strict types. Using ORDER BY on a column that contains a mix of data value types will follow the order: NULL values, integer and real values (sorted numerically), text values, and finally blob values. It’s especially important to note that NULL values will always be first when sorting by ascending or descending order. We can see this in action by ordering by ascending p-value: 

```txt
sqlite> SELECT trait, chrom, position, strongest_risk_snp, pvalue
...> FROM gwascat ORDER BY pvalue LIMIT 5;
trait    chrom    position    strongest_risk_snp   pvalue
Brain imaging    rs10932886 
```

```txt
Brain imaging rs429358
Brain imaging rs7610017
Brain imaging rs6463843
Brain imaging rs2075650 
```

In the next section, we’ll see how we can filter out rows with NULL values with WHICH clauses. 

As discussed in Chapter 8, ordering data to look for suspicious outliers is a great way to look for problems in data. For example, consider what happens when we sort pvalues (which as a probability, must mathematically be between 0 and 1, inclusive) in descending order: 

```sql
sqlite> SELECT trait, strongest_risk_snp, pvalue
...> FROM gwascat ORDER BY pvalue DESC LIMIT 5;
trait strongest_risk_snp pvalue 
```

Yikes! These two erroneous p-values likely occurred as a data entry mistake. It’s very easy to miss the negative when entering p-values in scientific notation (e.g., 9e-7 ver‐ sus 9e7). Problems like these once again illustrate why it’s essential to not blindly trust your data. 

## Filtering which rows with WHERE

Up until now, we’ve been selecting all rows from a database table and adding ORDER BY and LIMIT clauses to sort and limit the results. But the strength of a relational data‐ base isn’t in selecting all data from a table, it’s in using queries to select out specific informative subsets of data. We filter out particular rows using WHERE clauses, which is the heart of making queries with SELECT statements. 

Let’s look at a simple example—suppose we wanted to find rows where the strongest risk SNP is rs429358: 

```sql
sqlite> SELECT chrom, position, trait, strongest_risk_snp, pvalue
...> FROM gwascat WHERE strongest_risk_snp = "rs429358"; 
```

<table><tr><td>chrom</td><td>position</td><td>trait</td><td>strongest_risk_snp</td><td>pvalue</td></tr><tr><td>19</td><td>44908684</td><td>Alzheimer&#x27;s disease biomarkers</td><td>rs429358</td><td>5.0e-14</td></tr><tr><td>19</td><td>44908684</td><td>Alzheimer&#x27;s disease biomarkers</td><td>rs429358</td><td>1.0e-06</td></tr><tr><td></td><td></td><td>Brain imaging</td><td>rs429358</td><td></td></tr></table>

SQLite uses = or == to compare values. Note that all entries are case sensitive, so if your values have inconsistent case, you can use the lower() function to convert val‐ ues to lowercase before comparison: 

```sql
sqlite> SELECT chrom, position, trait, strongest_risk_snp, pvalue
...> FROM gwascat WHERE lower(strongest_risk_snp) = "rs429358";
chrom position trait strongest_risk_snp pvalue
19 44908684 Alzheimer's disease biomarkers rs429358 5.0e-14
19 44908684 Alzheimer's disease biomarkers rs429358 1.0e-06
Brain imaging rs429358 
```

In our case, this doesn’t lead to a difference because the RS identifiers all have “rs” in lowercase. It’s best to try to use consistent naming, but if you’re not 100% sure that all RS identifiers start with a lowercase “rs,” converting everything to the same case is the robust solution. 

The equals operator (=) is just one of many useful SQLite comparison operators; see Table 13-2 for a table of some common operators. 


Table 13-2. Common operators used in WHERE statements


<table><tr><td>Operator</td><td>Description</td></tr><tr><td>=,==</td><td>Equals</td></tr><tr><td>!=,&lt;&gt;</td><td>Not equals</td></tr><tr><td>IS, IS NOT</td><td>Identical to = and !=, except that IS and IS NOT can be used to check for NULL values</td></tr><tr><td>&lt;,&lt;=</td><td>Less than, less than or equal to</td></tr><tr><td>&gt;,&gt;=</td><td>Greater than, greater than or equal to</td></tr><tr><td>x IN (a, b, ...)</td><td>Returns whether x is in the list of items (a, b, ...)</td></tr><tr><td>x NOT IN (a, b, ...)</td><td>Returns whether x is not in the list of items (a, b, ...)</td></tr><tr><td>NOT</td><td>Logical negation</td></tr><tr><td>LIKE</td><td>Pattern matching (use % as a wildcard, e.g. like Bash&#x27;s *)</td></tr><tr><td>BETWEEN</td><td>x BETWEEN start AND END is a shortcut for x &gt;= start AND x &lt;= end</td></tr><tr><td>-</td><td>Negative</td></tr></table>

```txt
sqlite> SELECT chrom, position, trait, strongest_risk_snp, pvalue
...> FROM gwascat ORDER BY pvalue LIMIT 5;
chrom    position    trait    strongest_risk_snp   pvalue
------------ ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ---------- ----------- 
```

<table><tr><td>Operator</td><td>Description</td></tr><tr><td>+</td><td>Positive</td></tr></table>

Additionally, we can build larger expressions by using AND and OR. For example, to retrieve all rows with p-value less than 10 x 10<sup>-15</sup> and on chromosome 22: 

```sql
sqlite> SELECT chrom, position, trait, strongest_risk_snp, pvalue ...> FROM gwascat WHERE chrom = "22" AND pvalue < 10e-15; 
```

<table><tr><td>chrom</td><td>position</td><td>trait</td><td>strongest_risk_snp</td><td>pvalue</td></tr><tr><td>22</td><td>39351666</td><td>Rheumatoid arthritis</td><td>rs909685</td><td>1.0e-16</td></tr><tr><td>22</td><td>21577779</td><td>HDL cholesterol</td><td>rs181362</td><td>4.0e-18</td></tr><tr><td>22</td><td>39146287</td><td>Multiple myeloma</td><td>rs877529</td><td>8.0e-16</td></tr><tr><td>22</td><td>37185445</td><td>Graves&#x27; disease</td><td>rs229527</td><td>5.0e-20</td></tr><tr><td>22</td><td>40480230</td><td>Breast cancer</td><td>rs6001930</td><td>9.0e-19</td></tr><tr><td>[...]</td><td></td><td></td><td></td><td></td></tr></table>

It’s important to note that using colname = NULL will not work, as NULL is not equal to anything by definition; IS needs to be used in this case. This is useful when we combine create SELECT statements that use ORDER BY to order by a column that con tains NULLs (which show up at the top). Compare: 

to using WHERE pvalue IS NOT NULL to eliminate NULL values before ordering: 

```sql
sqlite> SELECT chrom, position, trait, strongest_risk_snp, pvalue
...> FROM gwascat WHERE pvalue IS NOT NULL ORDER BY pvalue LIMIT 5;
chrom position trait strongest_risk_snp pvalue 
```

<table><tr><td>16</td><td>56959412</td><td>HDL cholesterol</td><td>rs3764261</td><td>0.0</td></tr><tr><td>10</td><td>122454932</td><td>Age-related mac</td><td>rs10490924</td><td>0.0</td></tr><tr><td>1</td><td>196710325</td><td>Age-related mac</td><td>rs10737680</td><td>0.0</td></tr><tr><td>4</td><td>9942428</td><td>Urate levels</td><td>rs12498742</td><td>0.0</td></tr><tr><td>6</td><td>43957789</td><td>Vascular endoth</td><td>rs4513773</td><td>0.0</td></tr></table>

OR can be used to select rows that satisfy either condition: 

```txt
sqlite> SELECT chrom, position, strongest_risk_snp, pvalue FROM gwascat
...> WHERE (chrom = "1" OR chrom = "2" OR chrom = "3")
...> AND pvalue < 10e-11 ORDER BY pvalue LIMIT 5;
chrom    position    strongest_risk_snp    pvalue
1    196710325    rs10737680    0.0 
```

<table><tr><td>2</td><td>233763993</td><td>rs6742078</td><td>4.94065645</td></tr><tr><td>3</td><td>165773492</td><td>rs1803274</td><td>6.0e-262</td></tr><tr><td>1</td><td>196690107</td><td>rs1061170</td><td>1.0e-261</td></tr><tr><td>2</td><td>73591809</td><td>rs13391552</td><td>5.0e-252</td></tr></table>

This approach works, but is complex. It’s helpful to group complex statements with parentheses, both to improve readability and ensure that SQLite is interpreting your statement correctly. 

Rather than listing out all possible values a column can take with OR and =, it’s often simpler to use IN (or take the complement with NOT IN): 

```txt
sqlite> SELECT chrom, position, strongest_risk_snp, pvalue FROM gwascat
...> WHERE chrom IN ("1", "2", "3") AND pvalue < 10e-11
...> ORDER BY pvalue LIMIT 5;
chrom    position    strongest_risk_snp    pvalue
1    196710325    rs10737680    0.0
2    233763993    rs6742078    4.94065645
3    165773492    rs1803274    6.0e-262
1    196690107    rs1061170    1.0e-261
2    73591809    rs13391552    5.0e-252 
```

Finally, the BETWEEN … AND … operator is useful for retrieving entries between specific values. x BETWEEN start AND end works like x > = start AND x < = end: 

```txt
sqlite> SELECT chrom, position, strongest_risk_snp, pvalue
...> FROM gwascat WHERE chrom = "22"
...> AND position BETWEEN 24000000 AND 25000000
...> AND pvalue IS NOT NULL ORDER BY pvalue LIMIT 5;
chrom    position    strongest_risk_snp    pvalue
22    24603137    rs2073398    1.0e-109
22    24594246    rs4820599    7.0e-53
22    24600663    rs5751902    8.0e-20
22    24594246    rs4820599    4.0e-11
22    24186073    rs8141797    2.0e-09 
```

However, note that this approach is not as efficient as other methods that use special‐ ized data structures to handle ranges, such as Tabix, BedTools, or the interval trees in GenomicRanges. If you find many of your queries require finding rows that overlap regions and you’re running into bottlenecks, using one of these tools might be a bet‐ ter choice. Another option is to implement binning scheme, which assigns features to specific bins. To find features within a particular range, one can precalculate which bins these features would fall in and include only these bins in the WHERE clause. The UCSC Genome Browser uses this scheme (originally suggested by Lincoln Stein and Richard Durbin): see Kent et al., (2002) Te Human Genome Browser at UCSC for more information. We’ll discuss more efficiency tricks later on when we learn about table indexing. 

## SQLite Functions

So far, we’ve just accessed column data exactly as they are in the table using SELECT. But it’s also possible to use expressions to create new columns from existing columns. We use SQLite functions and operators to do this, and AS to give each new column a descriptive name. For example, we could create a region string in the format “chr4:165773492” for each row and convert all traits to lowercase: 

```sql
sqlite> SELECT lower(trait) AS trait,
...> "chr" || chrom || ":" || position AS region FROM gwascat LIMIT 5;
trait    region
asthma and hay fever chr6:32658824
asthma and hay fever chr4:38798089
asthma and hay fever chr5:11113180
asthma and hay fever chr2:10235008
asthma and hay fever chr17:3996642 
```

Here, || is the concatenation operator, which concatenates two strings. As another example, we may want to replace all NULL values with “NA” if we needed to write results to a file that may be fed into R. We can do this using the ifnull() function: 

```sql
sqlite> SELECT ifnull(chrom, "NA") AS chrom, ifnull(position, "NA") AS position,
...> strongest_risk_snp, ifnull(pvalue, "NA") AS pvalue FROM gwascat
...> WHERE strongest_risk_snp = "rs429358";
chrom    position    strongest_risk_snp    pvalue
19    44908684    rs429358    5.0e-14
19    44908684    rs429358    1.0e-06
NA    NA    rs429358    NA 
```

Later, we’ll see a brief example of how we can interact with SQLite databases directly in R (which automatically converts NULLs to R’s NAs). Still, if you’re interfacing with a SQLite database at a different stage in your work, ifnull() can be useful. 

We’re just scratching the surface of SQLite’s capabilities here; see Table 13-3 for other useful common functions. 


Table 13-3. Common SQLite functions


<table><tr><td>Function</td><td>Description</td></tr><tr><td>ifnull(x, val)</td><td>If x is NULL, return with val, otherwise return x; shorthand for coalesce() with two arguments</td></tr><tr><td>min(a, b, c, ...)</td><td>Return minimum in a, b, c, ...</td></tr><tr><td>max(a, b, c, ...)</td><td>Return maximum in a, b, c, ...</td></tr><tr><td>abs(x)</td><td>Absolute value</td></tr><tr><td>coalesce(a, b, c, ...)</td><td>Return first non-NULL value in a, b, c, ... or NULL if all values are NULL</td></tr><tr><td>length(x)</td><td>Returns number of characters in x</td></tr><tr><td>lower(x)</td><td>Return x in lowercase</td></tr><tr><td>upper(x)</td><td>Return x in uppercase</td></tr><tr><td>replace(x, str, repl)</td><td>Return x with all occurrences of str replaced with repl</td></tr><tr><td>round(x, digits)</td><td>Round x to digits (default 0)</td></tr><tr><td>trim(x, chars), ltrim(x, chars), rtrim(x, chars)</td><td>Trim off chars (spaces if chars is not specified) from both sides, left side, and right side of x, respectively.</td></tr><tr><td>substr(x, start, length)</td><td>Extract a substring from x starting from character start and is length characters long</td></tr></table>

## SQLite Aggregate Functions

Another type of SQLite function takes a column retrieved from a query as input and returns a single value. For example, consider counting all values in a column: a count() function would take all values in the column and return a single value (how many non-NULL values there are in the column). SQLite’s count() function given the argument * (and without a filtering WHERE clause) will return how many rows are in a database: 

```sql
sqlite> SELECT count(*) FROM gwascat;
count(*)
17290 
```

Using count(*) will always count the rows, regardless of whether there are NULLs. In contrast, calling count(colname) where colname is a particular column will return the number of non-NULL values: 

```sql
sqlite> SELECT count(pvalue) FROM gwascat;
count(pvalue)
17279 
```

We can use expressions and AS to print the number of NULL p-values, by subtracting the total number non-NULL p-values from the total number of rows to return the number of NULL p-values: 

```sql
sqlite> SELECT count(*) - count(pvalue) AS number_of_null_pvalues FROM gwascat;
number_of_null_pvalues
11 
```

Other aggregate functions can be used to find the average, minimum, maximum, or sum of a column (see Table 13-4). Note that all of these aggregating functions work with WHERE clauses, where the aggregate function is only applied to the filtered results. For example, we could find out how many entries have publication dates in 2007 (i.e., after December 31, 2006, but before January 1, 2008): 

```perl
sqlite> select "2007" AS year, count(*) AS number_entries
...> from gwascat WHERE date BETWEEN "2007-01-01" AND "2008-01-01";
year number_entries
2007 435 
```


Table 13-4. Common SQLite aggregate functions


<table><tr><td>Function</td><td>Description</td></tr><tr><td>count(x), count(*)</td><td>Return the number of non-NULL values in column x and the total number of rows, respectively</td></tr><tr><td>avg(x)</td><td>Return the average of column x</td></tr><tr><td>max(x), min(x)</td><td>Return the maximum and minimum values in x, respectively</td></tr><tr><td>sum(x), total(x)</td><td>Return the sum of column x; if all values of x are NULL, sum(x) will return NULL and total(x) will return 0. sum() will return integers if all data are integers; total() will always return a real value</td></tr></table>

Note that unlike other relational databases like MySQL and PostgreSQL, SQLite does not have a dedicated date type. Although the date is text, this works because dates in this table are stored in ISO 8601 format (introduced briefly in “Command Substitu‐ tion” on page 54), which uses leading zeros and is in the form: YYYY-MM-DD. Because this format cleverly arranges the date from largest period (year) to smallest (day), sorting and comparing this date as text is equivalent to comparing the actual dates. This is the benefit of storing dates in a tidy format (see XKCD’s ISO 8601). 

A nice feature of aggregate functions is that they allow prefiltering of duplicated val‐ ues through the DISTINCT keyword. For example, to count the number of unique non-NULL RS IDs in the strongest_risk_snp column: 

```sql
sqlite> SELECT count(DISTINCT strongest_risk_snp) AS unique_rs FROM gwascat;
unique_rs
13619 
```

## Grouping rows with GROUP BY

If you’re familiar with R (covered in Chapter 8), you’ve probably noticed that many of SQLite’s querying capabilities overlap R’s. We extract specific columns from a SQLite table using SELECT a, b FROM tbl;, which is similar to tbl[, c("a", "b")] to access columns a and b in an R dataframe. Likewise, SQL’s WHERE clauses filter rows much like R’s subsetting. Consider the similarities between SELECT a, b FROM tbl WHERE a < 4 AND b = 1; and tbl[tbl$a < 4 & b == 1, c("a", "b")]. In this section, we’ll learn about the SQL GROUP BY statement, which is similar to the splitapply-combine strategy covered in “Working with the Split-Apply-Combine Pattern” on page 239. 

As the name suggests, GROUP BY gathers rows into groups by some column’s value. Rather importantly, the aggregate functions we saw in the previous section are applied to each group separately. In this way, GROUP BY is analogous to using R’s split() function to split a dataframe by a column and using lapply() to apply an aggregating function to each split list item. Let’s look at an example—we’ll count how many associations there are in the GWAS catalog per chromosome: 

```sql
sqlite> SELECT chrom, count(*) FROM gwascat GROUP BY chrom;
chrom count(*)
-------- 70
1 1458
10 930
11 988
12 858
13 432
[...] 
```

We can order our GROUP BY results from most hits to least with ORDER BY. To tidy up our columns, we’ll also use AS to name the count(*) nhits: 

```sql
sqlite> SELECT chrom, count(*) as nhits FROM gwascat GROUP BY chrom
...> ORDER BY nhits DESC;
chrom    nhits
-------- ----
6    1658
1    1458
2    1432
3    1033
11    988
10    930
[...] 
```

As a more interesting example, let’s look at the top five most frequent strongest risk SNPs in the table: 

```txt
sqlite> select strongest_risk_snp, count(*) AS count
...> FROM gwascat GROUP BY strongest_risk_snp
...> ORDER BY count DESC LIMIT 5;
strongest_risk_snp count
rs1260326 36
rs4420638 30
rs1800562 28
rs7903146 27
rs964184 25 
```

It’s also possible to group by multiple columns. Suppose, for example, that you weren’t just interested in the number of associations per strongest risk SNP, but also the number of associations for each allele in these SNPs. In this case, we want to com‐ pute count(*) grouping by both SNP and allele: 

```txt
sqlite> select strongest_risk_snp, strongest_risk_allele, count(*) AS count
...> FROM gwascat GROUP BY strongest_risk_snp, strongest_risk_allele
...> ORDER BY count DESC LIMIT 10;
strongest_risk_snp strongest_risk_allele count 
```

<table><tr><td>rs1260326</td><td>T</td><td>22</td></tr><tr><td>rs2186369</td><td>G</td><td>22</td></tr><tr><td>rs1800562</td><td>A</td><td>20</td></tr><tr><td>rs909674</td><td>C</td><td>20</td></tr><tr><td>rs11710456</td><td>G</td><td>19</td></tr><tr><td>rs7903146</td><td>T</td><td>19</td></tr><tr><td>rs4420638</td><td>G</td><td>18</td></tr><tr><td>rs964184</td><td>G</td><td>15</td></tr><tr><td>rs11847263</td><td>G</td><td>14</td></tr><tr><td>rs3184504</td><td>T</td><td>12</td></tr></table>

All other aggregate functions similarly work on grouped data. As an example of how we can combine aggregate functions with GROUP BY, let’s look at the average log10 pvalue for all association studies grouped by year. The gwascat table doesn’t have a year column, but we can easily extract year from the date column using the substr() function. Let’s build up the query incrementally, first ensuring we’re extracting year correctly: 

```sql
sqlite> SELECT substr(date, 1, 4) AS year FROM gwascat GROUP BY year;
year
----
2005
2006
2007
2008
2009
2010 
```

```csv
2011
2012
2013 
```

Now that this is working, let’s add in a call to avg() to find the average -log10 pvalues per year (using round() to simplify the output) and count() to get the number of cases in each group: 

```txt
sqlite> SELECT substr(date, 1, 4) AS year,
...> round(avg(pvalue_mlog), 4) AS mean_log_pvalue,
...> count(pvalue_mlog) AS n
...> FROM gwascat GROUP BY year;
year    mean_log_pvalue   n
2005    6.2474    2
2006    7.234    8
2007    11.0973    434
2008    11.5054    971
2009    12.6279    1323
2010    13.0641    2528
2011    13.3437    2349
2012    9.6976    4197
2013    10.3643    5406 
```

The primary trend is an increase in -log10 p-values over time. This is likely caused by larger study sample sizes; confirming this will take a bit more sleuthing. 

It’s important to note that filtering with WHERE applies to rows before grouping. If you want to filter groups themselves on some condition, you need to use the HAVING clause. For example, maybe we only want to report per-group averages when we have more than a certain number of cases (because our -log10 p-value averages may not be reliable with too few cases). You can do that with: 

```txt
sqlite> SELECT substr(date, 1, 4) AS year,
...> round(avg(pvalue_mlog), 4) AS mean_log_pvalue,
...> count(pvalue_mlog) AS n
...> FROM gwascat GROUP BY year
...> HAVING count(pvalue_mlog) > 10;
year    mean_log_pvalue   n
2007    11.0973    434
2008    11.5054    971
2009    12.6279    1323
2010    13.0641    2528
2011    13.3437    2349
2012    9.6976    4197
2013    10.3643    5406 
```

We’ve filtered here on the total per-group number of cases, but in general HAVING works with any aggregate functions (e.g., avg(), sum(), max(), etc.). 

## Subqueries

As a teaser of the power of SQL queries, let’s look at a more advanced example that uses aggregation. The structure of the gwascat table is that a single study can have multiple rows, each row corresponding to a different significant association in the study. As study sample sizes grow larger (and have increased statistical power), intui‐ tively it would make sense if later publications find more significant trait associations. Let’s build up a query to summarize the data to look for this pattern. The first step is to count how many associations each study has. Because the PubMed ID (column pubmedid) is unique per publication, this is an ideal grouping factor (see Example 13-1). 

Example 13-1. An example subquery 

```txt
sqlite> SELECT substr(date, 1, 4) AS year, author, pubmedid, ...> count(*) AS num_assoc FROM gwascat GROUP BY pubmedid ...> LIMIT 5;
year    author    pubmedid    num_assoc
2005    Klein RJ    15761122    1
2005    Maraganore   16252231    1
2006    Arking DE    16648850    1
2006    Fung HC    17052657    3
2006    Dewan A    17053108    1 
```

I’ve included the author and pubmedid columns in output so it’s easier to see what’s going on here. These results are useful, but what we really want is another level of aggregation: we want to find the average number of associations across publications, per year. In SQL lingo, this means we need to take the preceding results, group by year, and use the aggregating function avg() to calculate the average number of asso‐ ciations per publication, per year. SQLite (and other SQL databases too) have a method for nesting queries this way called subqueries. We can simply wrap the pre‐ ceding query in parentheses, and treat it like a database we’re specifying with FROM: 

```sql
sqlite> SELECT year, avg(num_assoc)
...> FROM (SELECT substr(date, 1, 4) AS year,
...>    author,
...>    count(*) AS num_assoc
...>    FROM gwascat GROUP BY pubmedid)
...> GROUP BY year;
year    avg(num_assoc)
2005    1.0
2006    1.6
2007    5.87837837
2008    7.64566929
2009    6.90625
2010    9.21660649 
```

```txt
2011 7.49681528
2012 13.4536741
2013 16.6055045 
```

Indeed, it looks like there are more associations being found per study in more recent studies. While this subquery looks complex, structurally it’s identical to SELECT year, avg(num_assoc) FROM tbl GROUP BY year; where tbl is replaced by the SELECT query Example 13-1. While we’re still only scratching the surface of SQL’s capabilities, subqueries are good illustration of the advanced queries that can be done entirely within SQL. 

## Organizing Relational Databases and Joins

The solution to this redundancy is to organize data into multiple tables and use joins in queries to tie the data back together. If WHERE statements are the heart of making queries with SELECT statements, joins are the soul. 

## Organizing relational databases

If you poke around in the gwascat table, you’ll notice there’s a great deal of redun‐ dancy. Each row of the table contains a single trait association from a study. Many studies included in the table find multiple associations, meaning that there’s a lot of duplicate information related to the study. Consider a few rows from the study with PubMed ID “24388013”: 

```sql
sqlite> SELECT date, pubmedid, author, strongest_risk_snp
...> FROM gwascat WHERE pubmedid = "24388013" LIMIT 5;
date    pubmedid    author    strongest_risk_snp 
```

<table><tr><td>2013-12-30</td><td>24388013</td><td>Ferreira MA</td><td>rs9273373</td></tr><tr><td>2013-12-30</td><td>24388013</td><td>Ferreira MA</td><td>rs4833095</td></tr><tr><td>2013-12-30</td><td>24388013</td><td>Ferreira MA</td><td>rs1438673</td></tr><tr><td>2013-12-30</td><td>24388013</td><td>Ferreira MA</td><td>rs10197862</td></tr><tr><td>2013-12-30</td><td>24388013</td><td>Ferreira MA</td><td>rs7212938</td></tr></table>

I’ve only selected a few columns to include here, but all rows from this particular study have duplicated author, date, journal, link, study, initial_samplesize, and replication_samplesize column values. This redundancy is not surprising given that this data originates from a spreadsheet, where data is always organized in flat tables. The database community often refers to unnecessary redundancy in database tables as spreadsheet syndrome. With large databases, this redundancy can unnecessa‐ rily increase required disk space. 

Before learning about the different join operations and how they work, it’s important to understand the philosophy of organizing data in a relational database so as to avoid spreadsheet syndrome. This philosophy is database normalization, and it’s an exten‐ sive topic in its own right. Database normalization is a hierarchy of increasingly nor‐ malized levels called normal forms. Though the technical definitions of the normal forms are quite complicated, the big picture is most important when starting to orga‐ nize your own relational databases. I’ll step through a few examples that illustrate how reorganizing data in a table can reduce redundancy and simplify queries. I’ll mention the normal forms in passing, but I encourage you to further explore data‐ base normalization, especially if you’re creating and working with a complex rela‐ tional database. 

First, it’s best to organize data such that every column of a row only contains one data value. Let’s look at an imaginary table called assocs that breaks this rule: 

<table><tr><td>id</td><td>pubmedid</td><td>year</td><td>journal</td><td>trait</td><td>strongest_risk_snp</td></tr><tr><td>--</td><td>----</td><td>----</td><td>----</td><td>----</td><td>----</td></tr><tr><td>1</td><td>24388013</td><td>2013</td><td>J Allergy</td><td>Asthma, hay fever</td><td>rs9273373,rs4833095,rs1438673</td></tr><tr><td>2</td><td>17554300</td><td>2007</td><td>Nature</td><td>Hypertension</td><td>rs2820037</td></tr><tr><td>3</td><td>17554300</td><td>2007</td><td>Nature</td><td>Crohn&#x27;s disease</td><td>rs6596075</td></tr></table>

From what we’ve learned so far about SQL queries, you should be able to see why the organization of this table is going to cause headaches. Suppose a researcher asks you if rs4833095 is an identifier in the strongest risk SNP column of this table. Using the SQL querying techniques you learned earlier in this chapter, you might use SELECT * FROM assoc WHERE strongest_risk_snp = "rs4833095" to find matching records. However, this would not work, as multiple values are combined in a single column of the row containing rs4833095! 

A better way to organize this data is to create a separate row for each data value. We can do this by splitting the records with multiple values in a column into multiple rows (in database jargon, this is putting the table in first normal form): 

```csv
id pubmedid year journal trait strongest_risk_snp
-- ---- ---- ---- ----
1 24388013 2013 J Allergy Asthma, hay fever rs9273373
2 24388013 2013 J Allergy Asthma, hay fever rs4833095
3 24388013 2013 J Allergy Asthma, hay fever rs1438673
4 17554300 2007 Nature Hypertension rs2820037
5 17554300 2007 Nature Crohn's disease rs6596075 
```

You might notice something about this table now though: there’s now duplication in journal, year, and pubmedid column values. This duplication is avoidable, and arises because the year and journal columns are directly dependent on the pubmedid col‐ umn (and no other columns). 

A better way of organizing this data is to split the table into two tables (one for associ‐ ation results, and one for studies): 

```txt
assocs: 
```

```csv
id pubmedid trait strongest_risk_snp
-- ---- ----
1 24388013 Asthma, hay fever rs9273373
2 24388013 Asthma, hay fever rs4833095
3 24388013 Asthma, hay fever rs1438673
4 17554300 Hypertension rs2820037
5 17554300 Crohn's disease rs6596075 
```

studies: 

```txt
pubmedid year journal
24388013 2013 J Allergy
17554300 2007 Nature 
```

Organizing data this way considerably reduces redundancy (in database jargon, this scheme is related to the second and third normal forms). Now, our link between these two different tables are the pubmedid identifiers. Because pubmedid is a primary key that uniquely identifies each study, we can use these keys to reference studies from the assocs table. The pubmedid of the assocs table is called a foreign key, as it uniquely identifies a record in a different table, the studies table. 

While we used pubmedid as a foreign key in this example, in practice it’s also common to give records in the studies column an arbitrary primary key (like id in assocs) and store pubmedid as an additional column. This would look like: 

```txt
id study_id trait strongest_risk_snp
-- ---- ---- ----
1 1 Asthma, hay fever rs9273373
2 1 Asthma, hay fever rs4833095
3 1 Asthma, hay fever rs1438673
4 2 Hypertension rs2820037
5 2 Crohn's disease rs6596075
studies:
id pubmedid year journal
-- ---- ---- ----
1 24388013 2013 J Allergy
2 17554300 2007 Nature 
```

Now that we’ve organized our data into two tables, we’re ready to use joins to unite these two tables in queries. 

## Inner joins

Once data is neatly organized in different tables linked with foreign keys, we’re ready to use SQLite queries to join results back together. In these examples, we’ll use the joins.db database from this chapter’s GitHub directory. This small database contains the two tables we just organized, assocs and studies, with a few additional rows to illustrate some intricacies with different types of joins. 

Let’s load the joins.db database: 

```csv
$ sqlite3 joins.db
SQLite version 3.7.13 2012-07-17 17:46:21
Enter ".help" for instructions
Enter SQL statements terminated with a ";"
sqlite> .mode columns
sqlite> .header on 
```

Let’s take a peek at these example data: 

```txt
sqlite> SELECT * FROM assocs;
id study_id trait strongest_risk_snp
1 1 Asthma, hay fever rs9273373
2 1 Asthma, hay fever rs4833095
3 1 Asthma, hay fever rs1438673
4 2 Hypertension rs2820037
5 2 Crohn's disease rs6596075
6 Urate levels rs12498742 
```

```sql
sqlite> SELECT * FROM studies; 
```

<table><tr><td>id</td><td>pubmedid</td><td>year</td><td>journal</td></tr><tr><td>1</td><td>24388013</td><td>2013</td><td>J Allergy</td></tr><tr><td>2</td><td>17554300</td><td>2007</td><td>Nature</td></tr><tr><td>3</td><td>16252231</td><td>2005</td><td>Am J Hum G</td></tr></table>

By far, the most frequently used type of join used is an inner join. We would use an inner join to reunite the association results in the assocs table with the studies in the studies table. Here’s an example of the inner join notation: 

<table><tr><td>id</td><td>study_id</td><td>trait</td><td>strongest_risk_snp</td><td>id</td><td>pubmedid</td><td>year</td><td>journal</td></tr><tr><td>1</td><td>1</td><td>Asthma, hay fever</td><td>rs9273373</td><td>1</td><td>24388013</td><td>2013</td><td>J Allergy</td></tr><tr><td>2</td><td>1</td><td>Asthma, hay fever</td><td>rs4833095</td><td>1</td><td>24388013</td><td>2013</td><td>J Allergy</td></tr><tr><td>3</td><td>1</td><td>Asthma, hay fever</td><td>rs1438673</td><td>1</td><td>24388013</td><td>2013</td><td>J Allergy</td></tr><tr><td>4</td><td>2</td><td>Hypertension</td><td>rs2820037</td><td>2</td><td>17554300</td><td>2007</td><td>Nature</td></tr><tr><td>5</td><td>2</td><td>Crohn&#x27;s disease</td><td>rs6596075</td><td>2</td><td>17554300</td><td>2007</td><td>Nature</td></tr></table>

Notice three important parts of this join syntax: 

• The type of join (INNER JOIN). The table on the left side of the INNER JOIN state‐ ment is known as the lef table and the table on the right side is known as the right table. 

• The join predicate, which follows ON (in this case, it’s assocs.study_id = stud ies.id). 

• The new notation table.column we use to specify a column from a specific table. This is necessary because there may be duplicate column names in either table, so to select a particular column we need a way to identify which table it’s in. 

Inner joins only select records where the join predicate is true. In this case, this is all rows where the id column in the studies table is equal to the study_id column of the assocs table. Note that we must use the syntax studies.id to specify the column id from the table studies. Using id alone will not work, as id is a column in both assocs and studies. This is also the case if you select a subset of columns where some names are shared: 

<table><tr><td colspan="4">...&gt; INNER JOIN studies ON assocs.study_id = studies.id;</td></tr><tr><td>id</td><td>id</td><td>trait</td><td>year</td></tr><tr><td>1</td><td>1</td><td>Asthma, hay fever</td><td>2013</td></tr><tr><td>1</td><td>2</td><td>Asthma, hay fever</td><td>2013</td></tr><tr><td>1</td><td>3</td><td>Asthma, hay fever</td><td>2013</td></tr><tr><td>2</td><td>4</td><td>Hypertension</td><td>2007</td></tr><tr><td>2</td><td>5</td><td>Crohn&#x27;s disease</td><td>2007</td></tr></table>

To make the results table even clearer, use AS to rename columns: 

```sql
sqlite> SELECT studies.id AS study_id, assocs.id AS assoc_id, trait, year ...> FROM assocs INNER JOIN studies ON assocs.study_id = studies.id; 
```

<table><tr><td>study_id</td><td>assoc_id</td><td>trait</td><td>year</td></tr><tr><td>1</td><td>1</td><td>Asthma, hay fever</td><td>2013</td></tr><tr><td>1</td><td>2</td><td>Asthma, hay fever</td><td>2013</td></tr><tr><td>1</td><td>3</td><td>Asthma, hay fever</td><td>2013</td></tr><tr><td>2</td><td>4</td><td>Hypertension</td><td>2007</td></tr><tr><td>2</td><td>5</td><td>Crohn&#x27;s disease</td><td>2007</td></tr></table>

In both cases, our join predicate links the studies table’s primary key, id, with the study_id foreign key in assocs. But note that our inner join only returns five col‐ umns, when there are six records in assocs: 

```sql
sqlite> SELECT count(*) FROM assocs INNER JOIN studies
...> ON assocs.study_id = studies.id;
count(*)
----
5
sqlite> SELECT count(*) FROM assocs; 
```

```txt
count(*)
6 
```

What’s going on here? There’s one record in assocs with a study_id not in the studies.id column (in this case, because it’s NULL). We can find such rows using subqueries: 

```sql
sqlite> SELECT * FROM assocs WHERE study_id NOT IN (SELECT id FROM studies);
id    study_id    trait    strongest_risk_snp
6    Urate levels  rs12498742 
```

Likewise, there is also a record in the studies table that is not linked to any associa‐ tion results in the assocs table: 

```sql
sqlite> SELECT * FROM studies WHERE id NOT IN (SELECT study_id FROM assocs);
id pubmedid year journal
3 16252231 2005 Am J Hum Genet 
```

When using inner joins, it’s crucial to remember that such cases will be ignored! All records where the join predicate assocs.study_id = studies.id is not true are excluded from the results of an inner join. In this example, some records from both the left table (assocs) and right table (studies) are excluded. 

## Left outer joins

In some circumstances, we want to join in such a way that includes all records from one table, even if the join predicate is false. These types of joins will leave some col‐ umn values NULL (because there’s no corresponding row in the other table) but pair up corresponding records where appropriate. These types of joins are known as outer joins. SQLite only supports a type of outer join known as a lef outer join, which we’ll see in this section. 

![image](images/fig_003.jpg)


## Other Types of Outer Joins

In addition to the left outer joins we’ll discuss in this section, there are two other types of outer joins: right outer joins and full outer joins. Right outer joins are like left outer joins that include all rows from the right table. Right outer joins aren’t supported in SQLite, but we can easily emulate their behavior using left outer joins by swapping the left and right tables (we’ll see this in this section). 

Full outer joins return all rows from both the left and right table, uniting records from both tables where the join predicate is true. SQLite doesn’t support full outer joins, but it’s possible to emulate full outer joins in SQLite (though this is outside of the scope of this book). In cases where I’ve needed to make use of extensive outer joins, I’ve found it easiest to switch to PostgreSQL, which supports a variety of different joins. 

Left outer joins include all records from the left table (remember, this is the table on the left of the JOIN keyword). Cases where the join predicate are true are still linked in the results. For example, if we wanted to print all association records in the assocs table (regardless of whether they come from a study in the studies table), we would use: 

```txt
sqlite> SELECT * FROM assocs LEFT OUTER JOIN studies
...> ON assocs.study_id = studies.id;
id study_id trait [...]risk_snp id pubmedid year journal
1 1 Asthma, hay fever rs9273373 1 24388013 2013 J Allergy
2 1 Asthma, hay fever rs4833095 1 24388013 2013 J Allergy
3 1 Asthma, hay fever rs1438673 1 24388013 2013 J Allergy
4 2 Hypertension rs2820037 2 17554300 2007 Nature
5 2 Crohn's disease rs6596075 2 17554300 2007 Nature
6 Urate levels rs12498742 
```

As this example shows (note the last record), SQLite left outer joins cover a very important use case: we need all records from the left table, but want to join on data from the right table whenever it’s possible to do so. In contrast, remember that an inner join will only return records in which the join predicate is true. 

While SQLite doesn’t support right outer joins, we can emulate their behavior by swapping the left and right columns. For example, suppose rather than fetching all association results and joining on a study where a corresponding study exists, we wanted to fetch all studies, and join on an association where one exists. This can easily be done by making studies the left table, and assocs the right table in a left outer join: 

<table><tr><td>id</td><td>pubmedid</td><td>year</td><td>journal</td><td>id</td><td>study_id</td><td>trait</td><td>[...]_risk_snp</td></tr><tr><td>1</td><td>24388013</td><td>2013</td><td>J Allergy</td><td>3</td><td>1</td><td>Asthma, hay fever</td><td>rs1438673</td></tr><tr><td>1</td><td>24388013</td><td>2013</td><td>J Allergy</td><td>2</td><td>1</td><td>Asthma, hay fever</td><td>rs4833095</td></tr><tr><td>1</td><td>24388013</td><td>2013</td><td>J Allergy</td><td>1</td><td>1</td><td>Asthma, hay fever</td><td>rs9273373</td></tr><tr><td>2</td><td>17554300</td><td>2007</td><td>Nature</td><td>5</td><td>2</td><td>Crohn&#x27;s disease</td><td>rs6596075</td></tr><tr><td>2</td><td>17554300</td><td>2007</td><td>Nature</td><td>4</td><td>2</td><td>Hypertension</td><td>rs2820037</td></tr><tr><td>3</td><td>16252231</td><td>2005</td><td>Am J Hum G</td><td></td><td></td><td></td><td></td></tr></table>

Again, note that the assocs’s columns id, study_id, trait, and strongest_risk_snp have some values that are NULL, for the single record (with PubMed ID 16252231) without any corresponding association results in assocs. 

Finally, while our example joins all use join predicates that are simply connecting assocs’s study_id foreign key with studies’s primary key, it’s important to recognize that join predicates can be quite advanced if necessary. It’s easy to build more com‐ plex join predicates that join on multiple columns using AND and ON to link state‐ ments. 

## Writing to Databases

Most bioinformatics databases are primarily read-only: we read data more often than we add new data or modify existing data. This is because data we load into a database is usually generated by a pipeline (e.g., gene annotation software) that takes input data and creates results we load in bulk to a database. Consequently, bioinformati‐ cians need to be primarily familiar with bulk loading data into a database rather than making incremental updates and modifications. In this section, we’ll learn the basic SQL syntax to create tables and insert records into tables. Then in the next section, we’ll see how to load data into SQLite using Python’s sqlite3 module. 

Because we mainly load data in bulk into bioinformatics databases once, we won’t cover tools common SQL modification and deletion operations like DROP, DELETE, UPDATE, and ALTER used to delete and modify tables and rows. 

## Creating tables

The SQL syntax to create a table is very simple. Using the .schema dot command, let’s look at the statement that was used to create the table study in gwascat2table.db (exe‐ cute this after connecting to the gwascat2table.db database with sqlite3 gwas‐ cat2table.db): 

```sql
sqlite> .schema study
CREATE TABLE study(
    id integer primary key,
    dbdate text,
    pubmedid integer, 
```

```txt
author text,
date text,
journal text,
link text,
study text,
trait text,
initial_samplesize text,
replication_samplesize text
); 
```

This shows the basic syntax of the CREATE TABLE command. The basic format is: 

```sql
CREATE TABLE tablename(
    id integer primary key,
    column1 column1_type,
    column2 column2_type,
    ...
); 
```

Each column can have one of the basic SQLite data types (text, integer, numeric, real, or blob) or none (for no type affinity). In the preceding example (and all table defini‐ tions used throughout this chapter), you’ll notice that the first column always has the definition id integer primary key. Primary keys are unique integers that are used to identify a record in a table. In general, every table you create should have primary key column, so you can unambiguously and exactly refer to any particular record. The guaranteed uniqueness means that no two rows can have the same primary key— you’ll get an error from SQLite if you try to insert a row with a duplicate primary key. Primary keys are one type of table constraint; others like UNIQUE, NOT NULL, CHECK and FOREIGN KEY are also useful in some situations but are outside of the scope of this chapter. If you’re creating multiple tables to build a complex relational database, I’d recommend getting the basic idea in this section, and then consulting a book dedica‐ ted to SQL. Thoughtfulness and planning definitely pay off when it comes to organiz‐ ing a database. 

Let’s create a toy SQLite database, and create a new table we’ll use in a later example: 

```sql
$ sqlite3 practice.db
sqlite> CREATE TABLE variants(
...> id integer primary key,
...> chrom text,
...> start integer,
...> end integer,
...> strand text,
...> name text); 
```

Then, using the dot command .tables shows this table now exists (and you can check its schema with .schema variants if you like): 

```txt
sqlite> .tables
variants 
```

## Inserting records into tables

Like creating new tables, inserting records into tables is simple. The basic syntax is: 

```sql
INSERT INTO tablename(column1, column2)
VALUES (value1, value2); 
```

It’s also possible to omit the column names (column1, column2), but I wouldn’t rec‐ ommend this—specifying the column names is more explicit and improves readabil‐ ity. 

In the previous section, we learned how all tables should absolutely have a primary key. Primary keys aren’t something we need to create manually ourselves—we can insert the value NULL and SQLite will automatically increment the last primary key (as long as it’s an integer primary key) and use that for the row we’re inserting. With this in mind, let’s insert a record into the variants table we created in the previous sec‐ tion: 

```sql
sqlite> INSERT INTO variants(id, chrom, start, end, strand, name)
...> VALUES(NULL, "16", 48224287, 48224287, "+", "rs17822931"); 
```

Then, let’s select all columns and rows from this table (which comprises only the record we just created) to ensure this worked as we expect: 

<table><tr><td>id</td><td>chrom</td><td>start</td><td>end</td><td>strand</td><td>name</td></tr><tr><td>1</td><td>16</td><td>48224287</td><td>48224287</td><td>+</td><td>rs17822931</td></tr></table>

## Indexing

While querying records in our example databases takes less than a second, complex queries (especially those involving joins) on large databases can be quite slow. Under the hood, SQLite needs to search every record to see if it matches the query. For large queries, these full table scans are time consuming, as potentially gigabytes of data need to be read from your disk. Fortunately, there’s a computational trick SQLite can use: it can index a column of a table. 

A database index works much like the index in a book. Database indexes contain an sorted listing of all entries found in a particular row, alongside which row these entries can be found. Because the indexed column’s entries are sorted, it’s much faster to search for particular entries (compare searching for a word in an entire book, ver‐ sus looking it up in an alphabetized index). Then, once the entry is found in the data‐ base index, it’s easy to find matching records in the table (similar to turning directly to a page where a word occurs in a book index). Indexes do come at a cost: just as book indexes add additional pages to a text, table indexes take up additional disk space. Indexes for very large tables can be quite large (something to be aware of when indexing bioinformatics data). 

Creating an index for a particular table’s column is quite simple. Here, we create an index on the strongest_risk_snp column of the table assocs in the gwascat2table.db database: 

sqlite> CREATE INDEX snp_idx ON assocs(strongest_risk_snp); 

We can use the SQLite dot command .indices to look at all table indexes: 

```txt
sqlite> .indices
snp_idx 
```

This index will automatically be updated as new records are added to the table. Behind the scenes, SQLite will utilize this indexed column to more efficiently query records. The databases used in this chapter’s examples are quite small, and queries on indexed tables are unlikely to be noticeably faster. 

Note that SQLite automatically indexes the primary key for each table—you won’t have to index this yourself. In addition, SQLite will not index foreign keys for you, and you should generally index foreign keys to improve the performance joins. For example, we would index the foreign key column study_id in assocs with: 

```txt
sqlite> CREATE INDEX study_id_idx ON assocs(study_id);
sqlite> .indices
snp_idx
study_id_idx 
```

If, for some reason, you need to delete an index, you can use DROP INDEX: 

```txt
sqlite> DROP INDEX snp_idx;
sqlite> .indices
study_id_idx 
```

## Dropping Tables and Deleting Databases

Occasionally, you’ll create a table incorrectly and need to delete it. We can delete a table old_table using: 

```sql
sqlite> DROP TABLE old_table; 
```

It’s also possible to modify existing tables with ALTER TABLE, but this is outside of the scope of this chapter. 

Unlike SQLite and PostgreSQL, which support multiple databases, each database in SQLite is a single file. The best way to delete a database is just to delete the entire SQLite database file. 

## Interacting with SQLite from Python

The SQLite command-line tool sqlite3 we’ve used in examples so far is just one way to interact with SQLite databases. The sqlite3 tool is primarily useful for extracting data from SQLite databases from a script, or quick exploration and interaction with a SQLite database. For more involved tasks such as loading numerous records into a database or executing complex queries as part of data analysis, it’s often preferable to interact with a SQLite database through an API. APIs allow you to interact with a database through an interface in your language of choice (as long as there’s an API for that language). In this section, we’ll take a quick look at Python’s excellent API. 

## Connecting to SQLite databases and creating tables from Python

Python’s standard library includes the module sqlite3 for working with SQLite data‐ bases. As with all of Python’s standard library modules, the documentation is thor‐ ough and clear, and should be your primary reference (using an API is largely about mastering its documentation!). 

Let’s take a quick look at the basics of using this Python module. Because Python is well suited to processing data in chunks, it’s a good language to reach for when bulkloading data into a SQLite database. In contrast, while it’s certainly possible to use R to bulk-load into a SQLite database, it’s a bit trickier if the data is too large to load into the database all at once. The following script is a simple example of how we initi‐ alize a connection to a SQLite database and execute a SQL statement to create a sim‐ ple table: 

```python
import sqlite3

# the filename of this SQLite database
db_filename = "variants.db"

# initialize database connection
conn = sqlite3.connect(db_filename) ①
c = conn.cursor() ②
table_def = """\ ③
CREATE TABLE variants(
    id integer primary key,
    chrom test,
    start integer,
    end integer,
    strand text,
    rsid text);
"""

c.execute(table_def) ④ 
```

```txt
conn.commit() 5
conn.close() 6 
```

0 First, we use the connect() function to establish a connection with the database (provided by the file in db_filename; in this case, we’re using variants.db). con nect() returns an object with class Connection, which here we assign to conn. Connection objects have numerous methods for interacting with a database con‐ nection (the exhaustive list is presented in the Python documentation). 

When interacting with a SQLite database through Python’s API, we use a Cursor object. Cursors allow us to retrieve results from queries we’ve made. In this script, we’re just executing a single SQL statement to create a table (and thus there are no results to fetch), but we’ll see more on how cursors are used later on. 

This block of text is the SQL statement to create a table. For readability, I’ve for‐ matted it across multiple lines. 

To execute SQL statements, we use the execute() method of the Cursor object c. In cases where there are results returned from the database after executing a SQL statement (as with a SELECT statement that has matching records), we’ll use this cursor object to retrieve them—(we’ll see an example of this later). 

We need to commit our statement, using the Connection.commit() method. This writes the changes to the SQL database. 

Finally, we close our connection to the database using Connection.close(). 

We’ll save this script to create_table.py. Before proceeding, let’s check that this worked. First, we run this script: 

```txt
$ python create_table.py 
```

This creates the variants.db SQLite database in the current directory. Let’s check that this has the correct schema: 

```txt
$ sqlite3 variants.db
SQLite version 3.7.13 2012-07-17 17:46:21
Enter ".help" for instructions
Enter SQL statements terminated with a ";"
sqlite> .tables
variants
sqlite> .schema variants
CREATE TABLE variants(
    id integer primary key,
    chrom test,
    start integer,
    end integer, 
```

```txt
strand text, rsid text); 
```

## Loading data into a table from Python

Next, we need to write code to load some data into the variants table. Included in this chapter’s directory in the GitHub repository is a tab-delimited example data file, variants.txt: 

```txt
$ cat variants.txt
chr10 114808901 114808902 + rs12255372
chr9 22125502 22125503 + rs1333049
chr3 46414946 46414978 + rs333
chr2 136608645 136608646 - rs4988235 
```

There are a couple important considerations when bulk-loading data into a SQLite table. First, it’s important to make sure data loaded into the database is clean, has the correct data type, and missing values are converted to NULL (which are represented in Python by None). Note that while SQLite is very permissive with data types, it’s usually best to try to stick with one type per column—as mentioned earlier, this makes downstream work that utilizes data from a table much simpler. 

Let’s now write a quick script to load data from the file variants.txt into our newly created table. Normally, you might fold this code into the same script that created the tables, but to prevent redundancy and simplify discussion, I’ll keep them separate. A simple script that reads a tab-delimited file, coerces each column’s data to the appro‐ priate type, and inserts these into the variants table would look as follows: 

```python
import sys
import sqlite3
from collections import OrderedDict

# the filename of this SQLite database
db_filename = "variants.db"

# initialize database connection
conn = sqlite3.connect(db_filename)
c = conn.cursor()

## Load Data
# columns (other than id, which is automatically incremented)
tbl_cols = OrderedDict([("chrom", str), ("start", int), ①
    ("end", int), ("strand", str),
    ("rsid", str)])
with open(sys.argv[1]) as input_file:
    for line in input_file:
    # split a tab-delimited line
    values = line.strip().split("\t")

    # pair each value with its column name 
```

```python
cols_values = zip(tbl_cols.keys(), values) ②
# use the column name to lookup an appropriate function to coerce each
# value to the appropriate type
coerced_values = [tbl_cols[col](value) for col, value in cols_values] ③
# create an empty list of placeholders
placeholders = ["?"] * len(tbl_cols) ④
# create the query by joining column names and placeholders quotation
# marks into comma-separated strings
colnames = ", ".join(tbl_cols.keys())
placeholders = ", ".join( placeholders )
query = "INSERT INTO variants(%s) VALUES (%s); "% (colnames, placeholders)

# execute query
c.execute(query, coerced_values) ⑤
conn.commit() # commit these inserts
conn.close() 
```

First, we use an OrderedDict from the collections module to store each col‐ umn in the table (and our variants.txt file) with its appropriate type. Functions str() and int() coerce their input to strings and integers, respectively. We use these functions to coerce data from the input data into its appropriate table type. Additionally, if the data cannot be coerced to the appropriate type, these func‐ tions will raise a loud error and stop the program. 

② Using the zip() function, we take a list of column names and a list of values from a single line of the tab-delimited input file, and combine them into a list of tuples. Pairing this data allows for easier processing in the next step. 

③ Here, we use a Python list comprehension to extract the appropriate coercion function from tbl_cols for each column. We then call this function on the value: this is what tbl_cols[col](value) does. While storing functions in lists, dictionaries, or OrderedDicts may seem foreign at first, this strategy can drasti‐ cally simplify code. The end result of this list comprehension is a list of values (still in the order as they appear in the input data), coerced to the appropriate type. To reduce the complexity of this example, I’ve not handled missing values (because our test data does not contain them). See the code-examples/ README.md file in this chapter’s GitHub repository for more information on this. 

While we could directly insert our values into a SQL query statement, this is a bad practice and should be avoided. At the very least, inserting values directly into a SQL query statement string can lead to a host of problems with quotations in the data being interpreted as valid SQL quotations. The solution is to use parameter substitution, or parameterization. Python’s sqlite3 module supports two methods to parameterize a SQL statement. In this case, we replace the values in an INSERT statement with ?, and then pass our values directly as a list to the Cursor.execute() method. Note too that we ignore the primary key column id, as SQLite will automatically increment this for us. 

Finally, we use the Cursor.execute() method to execute our SQL INSERT state‐ ment. 

Let’s run this script, and verify our data was loaded: 

$ python load_variants.py variants.txt 

Then, in SQLite: 

<table><tr><td colspan="6">sqlite&gt; .header on</td></tr><tr><td colspan="6">sqlite&gt; .mode column</td></tr><tr><td colspan="6">sqlite&gt; select * from variants;</td></tr><tr><td>id</td><td>chrom</td><td>start</td><td>end</td><td>strand</td><td>rsid</td></tr><tr><td>1</td><td>chr10</td><td>114808901</td><td>114808902</td><td>+</td><td>rs12255372</td></tr><tr><td>2</td><td>chr9</td><td>22125502</td><td>22125503</td><td>+</td><td>rs1333049</td></tr><tr><td>3</td><td>chr3</td><td>46414946</td><td>46414978</td><td>+</td><td>rs333</td></tr><tr><td>4</td><td>chr2</td><td>136608645</td><td>136608646</td><td>-</td><td>rs4988235</td></tr></table>

In this example, we’ve manually parsed and loaded multiple lines of a tab-delimited file into a table using Cursor.execute() and Connection.commit(). However, note that we’re only committing all of these INSERT statements at the end of the for loop. While this wouldn’t be a problem with small datasets that fit entirely in memory, for larger datasets (that may not fit entirely in memory) we need to pay attention to these technical details. 

One solution is to commit after each INSERT statement, which we could do explicitly by pushing conn.commit() inside the loop. sqlite3 also supports autocommit mode (which can be enabled in sqlite.connect()), which automatically commits SQL statements. Unfortunately, using either conn.commit() or autocommit mode leads to each record being committed one at a time, which can be inefficient. To get around this limitation, Python’s sqlite3 Cursor objects have an executemany() method that can take any Python sequence or iterator of values to fill placeholders in a query. Cur sor.executemany() is the preferable way to bulk-load large quantities of data into a SQLite database. Loading data with this approach is a bit more advanced (because it relies on using generator functions), but I’ve included an example script in this chap‐ ter’s directory in the GitHub repository. Note that it’s also possible to interface Python’s csv module’s reader objects with executemany(). 

```txt
R's RSQLite Package
Like Python, R has a library to interface with SQLite databases: RSQLite. Like sqlite3, RSQLite is very easy to use. The primary difference is that RSQLite connects well with R's dataframes. As a simple demonstration of the interface, let's connect with the variants.db database and execute a query:

> library(RSQLite)
> sqlite <- dbDriver("SQLite")
> variants_db <- dbConnect(sqlite, "variants.db")

> dbListTables(variants_db)
[1] "variants"
> dbListFields(variants_db, "variants")
[1] "id"    "chrom"  "start"  "end"    "strand"  "rsid"

> d <- dbGetQuery(variants_db, "SELECT * FROM variants;")
> head(d)
    id chrom    start    end strand    rsid
1   1 chr10  114808901  114808902    + rs12255372
2   2 chr9    22125502  22125503    + rs1333049
3   3 chr3    46414946  46414978    + rs333
4   4 chr2    136608645  136608646    - rs4988235
> class(d)
[1] "data.frame"

dbSendQuery() grabs all results from a SQL statement at once—much like using fetchall() in sqlite3. RSQLite also has methods that support incrementally accessing records resulting from a SQL statement, through dbSendQuery() and fetch(). Overall, the RSQLite's functions are similar to those in Python's sqlite3; for further information, consult the manual. 
```

Finally, let’s look at how to work with Python’s Cursor objects to retrieve data. We’ll step through this interactively in the Python shell. First, let’s connect to the database and initialize a Cursor: 

```python
python
>>> import sqlite3
>>> conn = sqlite3.connect("variants.db")
>>> c = conn.cursor()

Next, let's use Cursor.execute() to execute a SQL statement:

>>> statement = ""\ 
... SELECT chrom, start, end FROM variants WHERE rsid IN ('rs12255372', 'rs333')
... ""
>>> c.execute(statement)
<sqlite3.Cursor object at 0x10e249f80> 
```

Finally, we can fetch data from this query using the Cursor.fetchone(), Cursor.fetchmany() (which takes an integer argument of how many records to fetch), and Cursor.fetchall() methods. The benefit of the Cursor object is that it keeps track of which row’s you’ve fetched and which rows you haven’t, so you won’t accidentally double process a row. Let’s look at Cursor.fetchone(): 

```txt
>>> c.fetchone()
(u'chr10', 114808901, 114808902)
>>> c.fetchone()
(u'chr3', 46414946, 46414978)
>>> c.fetchone() # nothing left
>>> 
```

## Dumping Databases

Finally, let’s talk about database dumps. A database dump is all SQL commands neces‐ sary to entirely reproduce a database. Database dumps are useful to back up and duplicate databases. Dumps can also be useful in sharing databases, though in SQLite it’s easier to simply share the database file (but this isn’t possible with other database engines like MySQL and PostgreSQL). SQLite makes it very easy to dump a database. We can use the sqlite3 command-line tool: 

```sql
$ sqlite3 variants.db ".dump"
PRAGMA foreign_keys=OFF;
BEGIN TRANSACTION;
CREATE TABLE variants(
    id integer primary key,
    chrom test,
    start integer,
    end integer,
    strand text,
    rsid text);
INSERT INTO "variants" VALUES(1,'chr10',114808901,114808902,'+', 'rs12255372');
INSERT INTO "variants" VALUES(2,'chr9',22125502,22125503,'+', 'rs1333049');
INSERT INTO "variants" VALUES(3,'chr3',46414946,46414978,'+', 'rs333');
INSERT INTO "variants" VALUES(4,'chr2',136608645,136608646,'-', 'rs4988235');
COMMIT; 
```

The .dump dot command also takes an optional table name argument, if you wish to dump a single table and not the entire database. We can use database dumps to create databases: 

```txt
$ sqlite3 variants.db ".dump" > dump.sql
$ sqlite3 variants-duplicate.db < dump.sql 
```

This series of commands dumps all tables in the variants.db database to a SQL file dump.sql. Then, this SQL file is loaded into a new empty database variantsduplicate.db, creating all tables and inserting all data in the original variants.db data‐ base.