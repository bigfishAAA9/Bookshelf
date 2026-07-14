## A2.1 Introduction

At the start of each chapter, we describe the data that will be used so that readers can ensure they are able to actively follow all exercises. For the practical exercises in part II of this book in chapters 8, 9, and 10, we use a combination of publicly available data that you can download and additional data that we have simulated for an individual phenotype for body mass index (BMI). To the download data used in this book, refer to the dedicated companion website for this book: http://www.intro-statistical-genetics.com. 

## A2.2 Description of simulated data

For part II of the book, we use publicly available molecular genetic data from the 1000 Genome Project and simulated phenotypes based on publicly available genome-wide association study (GWAS) results for BMI [1]. We use PLINK [2] for the data preparation and GCTA [3] software to simulate the data. For the theory behind the simulations, see also [4]. 

First, we downloaded the data, extracted HapMap3 SNPs, and ran some basic QC: 

```txt
### Download 1000G data here: https://www.dropbox.com/s/k9ptc4kep9hmvz5/1kg_phase1_all.tar.gz#In Linux wget https://www.dropbox.com/s/k9ptc4kep9hmvz5/1kg_phase1_all.tar.gz
### Unzip
tar -xzf 1kg_phase1_all.tar.gz
### Extract hm3 snps
./plink \ 
```

```shell
--bfile 1kg _ phase1 _ all \
--out 1kg _ hm3 \
--extract w _ hm3.snplist \
--make-bed
### Run some basic QC
.\plink \
--bfile 1kg _ hm3 \
--geno 0.02 \
--mind 0.1 \
--hwe 0.001 \
--out 1kg _ hm3 _ qc \
--make-bed 
```

Next, we downloaded the meta results from the BMI GWAS at http://portals.broadinstitute.org/collaboration/giant/images/c/c8/Meta-analysis_Locke_et_al%2BUKBiobank_2018_UPDATED.txt.gz. We subsequently aligned the column names for SNPs and reference alleles and switched the sign of the effect in case risk and reference alleles are coded in the opposite direction between GWAS results and genetic data. We saved the rs number and the effect size in a tab-separated text file named “causal_loci_BMI.txt” Finally, we simulated a BMI phenotype using GCTA software based on the effect sizes of the GWAS results per SNP and a heritability of 20%. 

```markdown
### Simulate BMI phenotype
./gcta64 \
--bfile 1kg _ hm3 _ qc \
--simu-qt \
--simu-causal-loci causal _ loci _ BMI.txt \
--simu-hsq 0.2 \
--simu-rep 1 \
--out pheno _ BMI _ sim \
--thread-num 5 
```

A file named pheno_BMI_sim.phen will contain the simulated phenotypes for all individuals together with the family id and an individual id. 

## A2.3 Health and Retirement Study

In part III of this book, and specifically in chapter 11, we turn to more advanced applications such as causal modeling and regression analysis using polygenic scores. Here we provide several practical examples based on real findings from the research literature using the publicly available data from the Health and Retirement Study (HRS), a longitudinal large-scale study of the U.S. population age 50 and above and household members and spouses (see box 7.1). 

To obtain access to the data, you first need to register on the HRS website (see box A.1 for full instructions). You need to provide your e-mail address, username, and some additional information to receive a password that, in tandem with your username, allows you to access the data download platform. Eventually, you can enter the download portal with your username and password and most likely, we anticipate that you will get very nervous. It seems like a jungle of data, because HRS is a long-time, ongoing study with excellent (and multiple) resources. Luckily, the different data waves in HRS have been merged and harmonized into the so-called RAND datasets. Nonetheless, do read the introduction and documentation information that is provided before you start using the data. 

As you know, throughout this book, we use R since it is not only an excellent working environment but also open source and freely available. On the download page of HRS, data are mainly provided in SPSS, Stata, and SAS format. It is possible to download these files and follow the instructions in the accompanied document to combine the ASCII files containing the data with the respective software information and subsequently read these files into R, for example, with the foreign package for SPSS and Stata format. However, ideally, try to use the following script to directly download the data via RStudio. Again, all you need is your username and password. But first it is necessary to install the required software packages: 

Box A.1
Obtaining access to HRS data 

There are two types of HRS data products when applying for access—public data and restricted/sensitive data. Many phenotypes are found in the public data, including but not limited to demographic, socioeconomic, health information, and genetic PGS. To access the HRS public data, go to: https://hrs.isr.umich.edu/data-products/access-to-public-data to register and obtain a username and password. Once registration has been confirmed, use the username/password to download data files. Restricted/sensitive data include biomarker and health data, Medicare and Social Security, and genotypic data. The application process varies from signing supplemental agreement, to access only through remote virtual desktop or encrypted physical media. To apply for genetic data other than PGSs, follow the steps on https://hrs.isr.umich.edu/data-products/genetic-data/products. 

```txt
### Install the Health and Retirement Study on your computer
# First we need to install and activate a couple of helper packages:
# devtools works in the background for us, allowing us to download
# The packages (lodown, etc.) to download the data
# Important: for the R code, all quotation marks need to be in this style: ""
install.packages("devtools")
library(devtools)

# We download and activate the helper package, called lodown

install_github("ajdamico/lodown")
library(lodown)

### Let's download data

# If you would like to download all of the HRS data that you see on
# the website, this would be the command
# Attention! This might take a while

lodown("hrs", output_dir=getwd(),
    your_username="your username",
    your_password="your password") 
```

For this exercise, we do not need all of the data available from HRS; rather, you can focus on specific versions and subsets, which we can download like this: 

```txt
### Focus on specific datasets
# First, create a list of available datasets and define the download
# destination using output_dir. In our case we park it in the R
# project folder
hrs_cat<- 
    get_catalog("hrs", 
```

```r
output_dir=getwd(),
    your_username="your username",
    your_password="your password")

# If you execute the command:
View(hrs_cat)
# in RStudio, you can see the available datasets in
# parallel to the website
# First, define the first dataset we wish to download,
namely the
# PGSs: PGENSCORE3.zip
PGS<- subset(hrs_cat, grepl("PGENSCORE3.zip", file_name))
# Download the data
PGS<- lodown("hrs", PGS,
    your_username="your username",
    your_password="your password")
# Next define the second dataset we wish to download,
# namely the RAND longitudinal data:
randhrs1992_2016v1_STATA.zip.
# Please note that the version of the dataset can change
-- check this viewing the
# hrs_cat file and adapt the script accordingly.
Pheno<- subset(hrs_cat, grepl("randhrs1992_2016v1_STATA.zip", file_name))
###
Download the data
Pheno<- lodown("hrs", Pheno,
    your_username="your username",
    your_password="your password") 
```

The next step is to read the data into RStudio. Since we want to analyze the rich environmental (socioeconomic and demographic) information in the RAND data together with the information from the PGSs, we need to merge both data frames. In HRS, there is a household identifier (hhid) and a person identifier (pn) and together they uniquely identify individuals in the data, and therefore can be used to merge the PGSs with the phenotypes. 

```r
# Read in the data frame for the PGS
hrs_PGS<- readRDS("PGENSCORE3/pgenscore3e_r.rds")
# Read in the data frame for the Phenos
hrs_Phenos<- readRDS("randhrs1992_2016v1_STATA/randhrs1992_2016v1.rds")
# Merge data frames
hrs_GePhen<- merge(hrs_PGS,hrs_Phenos,by=c("hhid","pn")) 
```

Note that the HRS PGS data are available for African ancestry (PGENSCORE3A_R) and European ancestry (PGENSCORE3E_R), which we will focus on the latter in our examples. While the RAND data contains 37,495 individuals, genotype information is available for 12,090 individuals of European ancestry and only those who were in both original data frames end up in the merged one. 

The detailed description of the genetic data collection, QC, and score construction are accompanied in your zipped download folder under “docs” and can be found online $[5]$ . Briefly, genotyping was conducted by the Center for Inherited Disease Research (CIDR) in 2011, 2012, and 2015. Genotype data on over 19,000 HRS participants was obtained using the Illumina HumanOmni2.5 BeadChips (HumanOmni2.5-4v1, HumanOmni2.5-8v1, HumanOmni2.5-8v1.1), which measures approximately 2.4 million SNPs. Individuals with missing call rates >2%, SNPs with call rates <98%, HWE p-value <0.0001, chromosomal anomalies, and first-degree relatives in the HRS were removed. PGSs have been constructed with PRSice (see chapter 10), and all available information has been included in the scores. All of this type of should make sense to you and if it does not we strongly encourage that you read the introductory chapters before embarking upon any kind of data analysis. 

For the PGS-phenotype file, we end up with 12,090 unrelated individuals. However, there are still multiple members of the household, and we want to focus on individuals who do not share their micro environment, because this might bias estimates due to factors such as gene-environment correlation (see also chapter 6). We therefore select only one individual per household. 

```r
#### Select one individual per household
# First, we install and load the helper package "dplyr"
install.packages("dplyr")
library(dplyr) 
```

```r
# Then we create a variable (whh_count), which numbers individuals
# within households

hrs_GePhen_ci<- hrs_GePhen %>% group_by(hhid) %>% dplyr::mutate(wf_count=row_number())

# We keep arbitrarily always the first person in the
# household

hrs_GePhen_uni<- hrs_GePhen_ci[which(hrs_GePhen_ci$wf_count=="1"),]

# Finally, we clean the working space in RStudio rm(hrs_PGS,hrs_GePhen_ci,hrs_GePhen,hrs_Phenos) 
```

The data file we are using from here is named hrs_GePhen_uni and contains 8,451 individuals and more than 10,000 variables. Be sure before embarking on the analyses in chapter 11 that you also have the same number. 

## A2.4 Data used by chapter

## Chapter 8

All chapter 8 data can be downloaded from this book's companion website, http://www.intro-statistical-genetics.com: 

- ALL.chr21.vcf.gz 

- BMI_pheno.txt 

• 1kg_EU_qc.bim, 1kg_EU_qc.bed, 1kg_EU_qc.fam 

- hapmap-ceu.bim, hapmap-ceu.bed, hapmap-ceu.fam 

- list.txt 

• 1kg_hm3.bim, 1kg_hm3.bed, 1kg_hm3.fam 

- individuals_failQC.txt 

- hello_world.sh 

The hapmap-ceu file can be found at http://zzz.bwh.harvard.edu/plink/dist/hapmap-ceu.zip. This file contains the genotypes, map files, and two extra phenotype files that we describe shortly and is called hapmap-ceu.zip. 

All of the populations can be found at http://zzz.bwh.harvard.edu/plink/res.shtml. 

## Chapter 9

• 1kg_EU_BMI 

- 1kg_EU_Overweight 

• 1kg_hm3_qc 

• 1kg_hm3_pruned 

- 1kg_samples.txt 

- BMI_pheno.txt 

- 1kg_samples_EUR.txt 

- hapmap-ceu 

## Chapter 10

- 1kg_hm3_qc 

- score_rs9930506.txt 

- BMI.txt 

- BMI_pheno.txt 

- Obesity_pheno.txt 

• 1kg_EU_qc 

- pca.eigenvec 

- 1kg_samples.txt 

• BMI_score_MULTIANCESTRY.best 

- BMI_LDpred.txt 

## Chapter 11

See the previous section. 

## Chapter 12

• EA2_results.txt.gz 

• Giant_Height2018.txt.gz 

- eur_w_ld_chr.tar.bz2 

• w_hm3.snplist.bz2 

- GWAS_EA_example.txt 

- GWAS_CP_example.txt 

• LD-Hub_genetic_correlation_example.txt 

## References



1. A. E. Locke et al., Genetic studies of body mass index yield new insights for obesity biology. Nature 518, 197–206 (2015). 





2. S. M. Purcell et al., PLINK: A tool set for whole-genome association and population-based linkage analyses. Am. J. Hum. Genet. 81, 559–575 (2007). 





3. J. Yang, S. H. Lee, M. E. Goddard, and P. M. Visscher, GCTA: A tool for genome-wide complex trait analysis. Am. J. Hum. Genet. 88, 76–82 (2011). 





4. M. R. Robinson et al., Genotype-covariate interaction effects and the heritability of adult body mass index. Nat. Genet. 49, 1174 (2017). 





5. E. B. Ware, L. L. Schmitz, A. M. Gard, and J. D. Faul, HRS Polygenic Scores—Release 3 (2018) (available at http://hrsonline.isr.umich.edu/modules/meta/xyear/pgs/desc/PGENSCORES3DD.pdf). 



Active gene-environment correlation (rGE). Also known as niche creation, this is when individuals actively select or create environments that are associated with their own genetic predispositions. 

Additive genetic effects. When two or more genes contribute to a phenotype or when alleles of a single gene combine such that their combined effects on the phenotype equal the sum of their individual effects. 

Admixture. See Genetic admixture. 

Allele. Refers to each of the two or more alternative forms of a gene found at the same place on a chromosome that arise by mutation. 

Ancestry components. A predefined number of subgroups with distinctive allele frequencies, inferred from genome-wide data, which are then used to assign the ancestry of each individual without specifying the population to which the individual belongs. 

APOE gene. This gene provides the instructions to make a protein called apolipoprotein E, which combines with fats to form molecules called lipoproteins, which are responsible for packaging cholesterol and other fats and carrying them through the bloodstream. Normal levels of cholesterol are important to prevent cardiovascular diseases, including heart attack and stroke. There are different alleles of the APOE gene (e2, e3, e4), with e3 found in around half of the population. The e4 is well known for increasing the risk of late-onset Alzheimer's disease. 

Assortative mating. In genetic research refers to a mating structure in which pairs of individuals that are (genetically) similar to each other mate with a higher probability than expected under random mating. 

Autosomal chromosomes. The 22 numbered non-sex chromosomes. 

Bayesian approach. A statistical approach that uses methods that combine a likelihood function with a prior probability to calculate a posterior probability. The posterior probability can be used to estimate parameters and to quantify available knowledge regarding the parameter. Given a set of assumptions about the underlying model, it can provide a rigorous assessment of uncertainty. 

Bonferroni correction. A correction used to solve the multiple testing problem in genome-wide association studies (GWASs) because millions of regression tests are performed in parallel and would be likely to reap many false positives if a standard significance threshold was adopted. In a GWAS, the Bonferroni-corrected p-value of significance is $p < 5 \times 10^{-8}$ . 

Bottleneck effect. A temporary reduction in population size that causes the loss of genetic variation. 

Broad-sense heritability $(H^2)$ . The ratio of the total genetic variance $(V_{G})$ to the total phenotypic variance $(V_{P})$ where $V_{P} = V_{G} + V_{E}$ , and where $V_{E}$ is environmental variance. 

Candidate gene studies. These refer to work that focused on predefined genetic loci of interest based on what was thought to be a priori knowledge of the loci's biological function or impact on the trait being examined. Particularly, many early studies could not be replicated. 

Chromosome. A single molecule of DNA that comprises part of the genome. 

Clumping. A procedure in which only the most significant SNP (i.e., lowest p-value) in each LD block is identified and selected for further analyses, clumping reduces the correlation between the remaining SNPs, while retaining SNPs with the strongest statistical evidence. 

Common ancestors. An ancestor shared by two or more individuals. 

Cryptic relatedness. This refers to the presence of close relatives (second cousin and closer) in data sources. Such observations cannot be considered as independent. For analyses, typically, one of the individuals in a pair is removed or the dependence structure is considered in the statistical model, (e.g., including a genetic-relatedness matrix in a mixed-linear model). 

Deep phenotyping. A precise and comprehensive analysis of the phenotype by providing more specificity, linkage of new types of data and measurements, and new connections between trait subtypes and genetic variations. It is phenotyping in a much more fine-grained and multifaceted way, often using advanced algorithms and data linkage. 

Denisovans. Denisovans are an extinct subspecies of archaic hominins represented by fossil remains from a Denisova Cave in southern Siberia; genome sequence data indicate that Denisovans are a sister group to Neanderthals. 

Differential susceptibility model. This model is sometimes also referred to as the level of plasticity or the orchid–dandelion hypothesis, which hypothesizes that there are genotype groups that are sensitive to both negative and positive environments. 

DNA. Deoxyribonucleic acid is a molecule carrying the genetic instructions to build and reproduce living organisms in the form of a double helix. 

Environmental variance $(V_{E})$ . Environmental quantitative variance among individuals with the same genotype. 

Evocative (also known as reactive) gene-environment correlation (rGE). When an individual's partially genetically inherited traits evoke reactions from others in the environment (e.g., being shy, aggressive). 

Evolution. Refers to the change in heritable characteristics of populations over successive generations. It forms the basis of our understanding not only about the origin of the human species but also the underlying genetic architecture and disease mutations. 

Exogeneous variable. Often used in G×E studies to refer to a variable whose value is determined by factors outside of the causal system under study. 

Experimental factor ontology (EFO). EFO is an ontology of variables used in molecular biology including aspects of disease, anatomy, cell type, cell lines, chemical compounds, and assay information. In this book we discuss EFO in relation to groupings in the NHGRI-EBI GWAS Catalog. 

Fitness. Also sometimes referred to as evolutionary fitness. This is how well a species adapts to its environment. It can be defined differently in different models. In humans it is generally defined as the expected number of offspring of an individual of that genotype left to reproduce in the next generation. 

Founder effect. The effect of genetic variation as a new population, or species, is founded. It is a type of genetic drift, when a small group splits from the main population to found a colony, where the founders may be selective and not represent the full genetic diversity of the original group. 

Gene. A gene is a sequence of nucleotides in the DNA that codes for a molecule (e.g., a protein). 

Gene-environment correlation (rGE). This is the process by which an individual's genotype influences or is associated with exposure to the environment. 

Gene-environment interaction (G×E). G×E defines an interplay between a gene and an environmental factor in which the effect of the gene on a phenotype is modifiable by the environment, and vice versa. 

Genetic admixture. A population is admixed if it received gene-flow from another population. This happens when two or more previously isolated and genetically differentiated populations interbreed. The results are new genetic lineages, and it is usually only recent flow. The concept is sometimes also used to describe individuals that have ancestors from several different populations. 

Genetic drift. A change in allele frequencies over time in a population of finite size due to random transmission of parental alleles from parents to offspring and due to the fact that some individuals randomly produce more offspring than others, irrespective of their genotype. 

Genetic recombination. A process during meiosis that produces new combinations of alleles, by breaking pieces of DNA and recombining them. This process leads to diversity of DNA sequences. 

Genome. The genome is the collection of DNA of an individual. 

Genome-wide association study (GWAS). A GWAS is designed to adopt an unbiased hypothesis-free approach to discover genetic loci that are associated with a phenotype. They often combine data from multiple studies to gather the largest sample possible. First introduced in 2005, over 4,000 GWASs have been published by 2020 identifying thousands of genetic associations and their biological function. 

Genotype. Describes part of an individual's DNA that influences their phenotype. 

Germ line mutation. A mutation that will be inherited by the offspring of the organism. 

GWAS Catalog. Includes data from all published GWASs (https://www.ebi.ac.uk/gwas/). 

GWAS-heritability. The fraction of phenotypic variance of a trait explained by genome-wide significant genetic variants—sometimes also by polygenic scores based on GWAS findings. 

Haplotype. This is a multilocus genotype on a single chromosome. If three successive genotypes at polymorphic sites are AT, CG, and CA, the haplotypes may be ACA and TGC (or ACC and TGA, AGC and TCA, or AGA and TCC). 

HapMap. HapMap is an international project to develop a haplotype map, with the goal of identifying genetic similarities and differences among human populations and providing a reference panel for human genetic variation. The project has made large amounts of data publicly available. 

Hardy–Weinberg equilibrium (HWE). The HWE states that allele and genotype frequencies are constant over generations in the absence of evolutionary forces such as natural selection, mutation, or migration. Violation of the HWE law indicates that genotype frequencies are significantly different from expectations. In a GWAS, it is generally assumed that deviations from HWE are the result of genotyping errors. 

Heritability. A population measure defining the proportion of variance in a phenotype explained by genetic variance within a population. We can differentiate between broad-sense heritability, including both additive and non-additive genetic effects such as epistasis and dominance, and narrow-sense heritability focusing on additive genetic effects only. 

Heterozygous. The carrying of two different alleles of a specific SNP. The heterozygosity rate of an individual is the proportion of heterozygous genotypes. High levels of heterozygosity within an individual might be an indication of low sample quality whereas low levels of heterozygosity may be due to inbreeding. 

Hominidae. Modern humans, their fossil ancestors, and extinct relatives thereof, up to (but not including) chimpanzees. 

Homozygous. The proportion of individuals in a population that are homozygous at a particular locus. In other words, when an individual has two of the same allele, regardless of whether it is dominant or recessive. 

Inbreeding. When individuals who are related to one another produce offspring. The inbreeding coefficient measures the excess of homozygous individuals in a population relative to the expectation under the Hardy–Weinberg equilibrium. 

Linkage disequilibrium (LD). A measure of nonrandom association between alleles at different loci at the same chromosome in a given population. SNPs are in LD when the frequency of association of their alleles is higher than expected under random assortment. LD concerns patterns of correlations between SNPs. 

Major allele. It is the allele of a SNP with the highest frequency. 

Manhattan plot. This is a type of scatterplot of location of a genetic variant and the transformed p-value that is one of the main graphical techniques in a GWAS used to display the distribution of genetic hits. 

Meiosis. The process by which haploid gametes are generated, during which genetic recombination occurs. 

Mendelian traits. In contrast to polygenic or complex traits, Mendelian traits are only related to a single locus (e.g., Huntington's disease). 

Minor allele. The allele of a SNP with the lowest frequency. 

Minor allele frequency (MAF). The frequency of the least often occurring allele at a specific location. Most studies are underpowered to detect associations with SNPs with a low MAF and therefore exclude these SNPs. 

Mutation. A permanent change in the sequence that makes up a gene (see box 1.1 for explanation and definition of various types of mutations). 

Narrow-sense heritability $(h^{2})$ . The ratio of the additive genetic variance to the total variance of a quantitative character or $V_{A}/V_{P}$ . It is the proportion of variance in a phenotype within a population that is associated with additive genetic variance. 

Natural selection. The increase or decrease of particular genetic traits as a function of the differential fitness and the reproductive success of individuals. 

NHGRI-EBI GWAS Catalog. See GWAS Catalog. 

Null hypothesis. Refers to the testable hypothesis that there is no difference between a hypothesized value and an estimated statistic (for example, a value in a population or the correlation between two variables). 

Observational study. Refers to a sample of the population where the researcher—in contrast to an experiment—does not create variation in the independent or exposure variable. 

Pairwise linkage disequilibrium (pairwise LD). The strength of association or co-occurrence between alleles at two different genetic markers. 

Passive gene-environment correlation (rGE). The association between the genotype a child inherits from her or his parents and the environment in which the child is raised. 

Phenotype. The observable trait of an individual, ranging from physical traits (hair color, height) to disease status (diabetic) to behavior (risk-taker, age at first sexual intercourse, educational attainment). 

Population stratification. The presence of multiple subpopulations (e.g., individuals with different ancestral background) in a study. Because allele frequencies can differ between subpopulations, population stratification can lead to false positive associations and/or mask true associations. An excellent example is the chopstick gene, where a SNP, due to population stratification, would be wrongly assumed to be a true association due to differences in allele frequencies of those of Asian and European ancestry who have a different usage of chopsticks for purely cultural rather than biological reasons. 

Population structure. There is population structure when mating is more likely to occur between some subsets of the population than between others, typically due to geographical structure. Individuals located in geographical proximity to each other are more likely to mate. Population structure is also used to describe a population in which allele frequencies differ between different geographic regions. 

Power. The power of a statistical test is the probability of correctly rejecting a false null hypothesis. 

Principal component analysis (PCA). A statistical technique used to emphasize variation and bring out strong patterns underlying data, with the aim of minimal loss of information. It is a visualization technique and way to reduce the dimensionality of data from high to lower dimensional data stepwise, extracting independent dimensions with the highest variance explanation in the data. 

Pruning. A method to select a subset of markers that are in approximate linkage equilibrium. In PLINK, this method uses the strength of LD between SNPs within a specific window (region) of the chromosome and selects only SNPs that are approximately uncorrelated, based on a user-specified threshold of LD. In contrast to clumping, pruning does not take the p-value of a SNP into account. 

Random variable. A variable that takes on different values (for example, possible outcomes of an experiment) and for which each value can be associated with a probability. 

Relatedness. Indicates how strongly a pair of individuals are genetically related. A conventional GWAS assumes that all subjects are unrelated (i.e., no cryptic relatedness). Without appropriate correction, the inclusion of relatives could lead to biased estimations of standard errors of SNP effect sizes. Note that specific tools for analyzing family data have been developed. 

Significance level ( $\alpha$ ). A concept used in statistical hypothesis testing to determine when to reject a null hypothesis. If the probability of observing an outcome is extreme or more extreme than the observed outcome under the null hypothesis is less than the significance level, then the null hypothesis is rejected. It is chosen to be the greatest probability of type I error or false positive findings tolerated for a statistical test. 

Single-nucleotide polymorphism (SNP). A variation in a single nucleotide (i.e., A, C, G, or T) that occurs at a specific position in the genome. A SNP exists as two different forms (e.g., A vs. T). These different forms are called alleles. A SNP with two alleles has three different genotypes (e.g., AA, AT, and TT). 

SNP arrays. Microarrays that are used to simultaneously genotype several thousand to several hundred thousand SNPs for a single sample. 

SNP-heritability. The fraction of phenotypic variance of a trait explained by all SNPs in the analysis. 

SNP-level missingness. The number of individuals in the sample for whom information on a specific SNP is missing. SNPs with a high level of missingness can potentially lead to bias and are typically excluded from analysis. 

Social control model. Also referred to as the social push model, this is a G×E model that hypothesizes that genetic associations are attenuated or dampened in the presence of socially restrictive environmental contexts. 

Spurious relationship. When two variables are correlated and it is mistakenly believed that this association is causal. 

Summary statistics (in a GWAS). The results obtained after conducting a GWAS, including information on chromosome number, position of the SNP, SNP(rs)-identifier, MAF, effect size (odds ratio/beta), standard error, and p-value. These statistics are used, for example, to create polygenic scores. 

Test statistic. A numerical summary of the data used to measure support for the null hypothesis. A test statistic may have a known probability distribution (e.g., such as $x^{2}$ ) under the null hypothesis or its null distribution is estimated. 

Type 1 error. The rejection of a true null hypothesis. 

Type 2 error. Accepting a false null hypothesis. 

Variance. Signified by $\sigma^2$ and is the average of the squared deviation of the observations of an outcome of variable (often random variable) from its mean, divided by the number of individuals (sample size). 

Variance-covariance matrix. Used to describe the variances and covariances between variables. It is sometimes also referred to as the dispersion matrix or simply the covariance matrix. 

Variant Call Format (VCF). A type of data format that can store genomic information for genotyped, imputed data, and sequencing data. It is very flexible because various types of information can be stored (see chapter 7). 

Weak instrument assumption. One of the statistical assumptions underlying the instrumental variable (IV) and Mendelian Randomization approach to evaluate whether the polygenic score is a good instrument. The IV needs to be correlated with the treatment and generate sufficient variation in the independent variable (see chapter 13). 

Within-family analysis (sometimes also called family fixed-effect regression). An analysis that examines the effect of different genotypes among family members. Used to establish a true genetic effect that is not confounded by population stratification or genetic nurture, such as the differences in genotype across siblings as a random experiment (i.e., because alleles are transmitted randomly through meiosis). 

## Chapter 1

1. Somatic cells refer to any cell of a living organism other than reproductive cells (i.e., other than a gamete, germ cell, gametocyte, or undifferentiated stem cell). There are around 220 types of somatic cells in humans. 

2. It is worth pointing out here that the independent assortment is violated by genetic linkage. That is, genes on a chromosome will be inherited together with a frequency in inverse proportion to the physical distance between them. 

3. An insertion or deletion of bases in the genome is referred to as indel in molecular biology. 

4. Although we note that an RNA gene could have alleles. RNA is defined elsewhere in this chapter. 

5. Amino acids are the building blocks of proteins that catalyse most chemical reactions that occur in the cell. They provide the structural elements of cells and help to bind cells together into tissues. 

6. Polymers are large molecules that are made when many small molecules join together. 

7. Diploid cells contain two complete sets of chromosomes. This is opposed to haploid cells that have half the number and only contain one complete set of chromosomes. 

8. We realize that this is an oversimplification because of introns and alternative splicing, which we do not cover in this introductory book. 

9. Exons are any gene that encodes a part of the final mature RNA that is produced by that gene (after introns have been removed). 

10. The exome is the part of the genome that consists of exons (i.e., the coding regions). 

11. Introns are the noncoding sections of an RNA transcript that are spliced out before the RNA molecule is translated into a protein. Introns are also in the gene and do not remain in the transcript for long before they are spliced out. 

12. A conserved gene is one that has remained essentially unchanged throughout evolution and indicates that it is unique and essential. Note that conservation is always relative to a phylogenetic group. 

13. A peptide bond is a chemical bond that is formed between two molecules. Amino acids are joined by a series of peptide bonds which in turn form a peptide. 

14. A eukaryote is an organism that has complex cells where the genetic material is organized into a membrane-bound nucleus, such as animals, plants, and fungi. In contrast, prokaryotes are organisms that lack nuclei and most other complex cell structures, such as bacteria. 

15. Cytoplasm is the material within a cell excluding the nucleus. 

16. A polypeptide is a sequence of amino acids that are linked. A single polypeptide chain may comprise the entire primary structure of a protein, but more complex proteins are formed when two or more polypeptides link together. 

## Chapter 2

1. A random variable is one in where the possible values are outcomes of a random phenomenon. 

2. Latent variables (from hidden in Latin) refer to those that are not directly observed; rather they are inferred from other variables are directly observed and measured. One common example is using measures of the Big 5 personality traits to infer personality or others such as well-being, quality of life, or social belonging. 

3. Some of this basic terminology is used differently across the disciplines. In the social sciences, for instance, sample often refers to the dataset that is used (i.e., a sample of individuals from a larger population). In genetics, sample often refers to data from one individual. Another term that is used very differently is cohort. In genetic research the term cohort is often used to refer to what social scientists would call a sample or dataset and demographers would define as a birth cohort. Geneticists often use the term cohort to refer to the data regardless of whether the research design is a cohort design, which may be confusing for outsiders. 

## Chapter 3

1. Hominidae refers to the taxonomic family of primates that includes the living (extant) and extinct humans, chimpanzees, gorillas and orangutans. 

2. Recall from chapter 1 that a germ line is a series of germ cells that have developed or descended from earlier cells in a particular species. They continue through each successive generation. 

3. We note that these classic theories are often framed in purely heterosexual terms, since they are based on biological reproduction. We recognize that many gay and lesbians reproduce, most often through previous heterosexual relationships or increasingly via assisted reproductive technology [40]. 

## Chapter 4

1. http://depts.washington.edu/chargeco/wiki/ResultsSharingFormat. 

## Chapter 5

1. A recent study tested Cheverud's Conjecture, which states that phenotypic correlations are reasonable proxies for genetic correlation among humans with evidence for an association between both phenomena [77]. 

2. Cis-regulatory modules (CRMs) refers to the part of the DNA, often 100–1,000 DNA base pairs in length, where transcription factors bind and regulate the expression of nearby genes. 

3. https://rdrr.io/github/DudbridgeLab/avengeme/man/sampleSizeForGeneScore.html. 

## Chapter 7

1. We note that probe design is a very complex topic, and we simplify the discussion here. If a site is biallelic, only one variant may be targeted but you might have probes complementary to each strand to allow for cross-checking. 

2. Within the FTO gene, rs9930506 showed the strongest association with BMI $p=8.6*10^{-7}$ , hip circumference $p=3.4*10^{-8}$ , and weight $p=9.1*10^{-7}$ . Homozygotes for the rare “G” allele were 1.3 BMI units heavier than homozygotes for the common “A” allele. Source: https://www.snpedia.com/index.php/FTO. 

3. https://www.ncbi.nlm.nih.gov/assembly?term=GRCh38&cmd=DetailsSearch. 

## Chapter 8

1. The prefix of a file is the first part of the name of the file before the dot (.). The suffix refers to the file type that is listed after the dot. For example, in the file text.txt, text is the prefix and .txt is the suffix. 

2. Output is printed to stdout, which means standard output in Unix-like operating systems such as Linux and macOSx, where stdout is defined by the POSIX standard. POSIX stands for Portable Operating 

System Interface for Unix, which is a set of standards that define how Unix operating systems operate with one another and the command structure. In the terminal, stdout defaults to the user's screen. 

3. Alternatively, we can also remove one random individual from pairs that have a high degree of relatedness. 

4. Strand orientation refers to the direction in which genes are read (transcribed). In different genotyping platforms as well SNP databases, the same SNP can be defined as being on the forward (plus) or as being on the reverse (minus) strand. 

## Chapter 9

1. PLINK requires three parameters for pruning: a window size in variant count or kilobase (if the "kb" modifier is present) units, a variant count to shift the window at the end of each step, and a variance inflation factor (VIF) threshold. At each step, all variants in the current window with VIF exceeding the threshold are removed. 

2. Note that we are using the terminology from the 1000 Genomes Project, where American, for instance, refers to the CEU group of Utah residents with Northern and Western European ancestry. For more information, see http://www.internationalgenome.org/category/population/. 

## Chapter 10

1. https://github.com/crahal/GWASReview/blob/master/tables/Manually_Curated_Cohorts.csv. 

2. Bootstrapping is a technique that allows us to assign measures of accuracy to sample estimates. 

3. A point normal mixture statistical distribution (also called spike and slab) is a probability distribution derived by the combination of two probability distributions. Part of the SNPs are distributed according to a point mass distribution while the others are distributed according to a continuous normal distribution. 

4. MCMC methods refer to a class of algorithms that sample from a particular probability distribution. You are able to obtain a sample of the desired distribution by constructing and then observing a Markov chain after a number of steps. A larger number of steps indicates a closer match to the desired distribution. A Markov chain refers to a stochastic model of a sequence of possible events that is “memoryless” or, in other words, where the probability of each event only depends on the state of the previous event. 

5. Remember that an infinitesimal model is the polygenic model that assumes that a quantitative (continuous) trait is controlled by an infinite number of loci, all of which has an infinitely small effect. 

Note: Page numbers in italics indicate figures. 

Adaptation, 63, 117, 138
Additive model
    applied example, 300–301
    in association analysis, 218, 220
    graphical depiction of, 43–44
    upper bound of, 112
Admixture
    genetic, 58, 61–63, 73, 93
    and GWAS results, 142
    human dispersal, 57, 58
Africa. See also African distribution population, 60
East Africa, 60
in GWAS, 96
human dispersal out of, 55–58, 226
PCAs, 61, 229, 230
population structure, 264
sub-Saharan, 56, 58
West African, 60–61
African. See also Africa ancestry groups, 61, 264–265, 266
difference in allele frequencies, 60–61, 226
genetic diversity, 58, 72
in GWAS, 95, 96
inability to apply European ancestry polygenic scores, 264–266
LD in, 84
in 1000 Genomes Project, 157
scientists, 163
African Americans, 61, 74, 103, 366
Amino acids, 15–16, 18–20
definition of, 405n5
Anaconda, 267. See also Python installation of, 383–385
LDSC and MTAG, 333
Ancestors, 56, 57
of Australian Aborigines, 56
colonizing, 61
common, 70 

Anthropometric traits, 78, 115, 155, 315
and GIANT consortium, 16
Area under the curve (AUC), 120, 365
Asia
calculating LD scores, 324
data, 96, 161
East, 229, 230, 266
in GWAS, 95, 96
migration out of, 56, 57
PCAs, 61, 230
polygenic score distribution in, 266
population stratification, 226, 230
rise of genetic research, 96
Southeast, 226
Asian. See Asia
Assortative mating
in Hardy–Weinberg equation assumption, 70–71
in MR assumptions, 347–349
by traits, 68, 144
Autism, 6, 115
genetic overlap with, 162
heritability of, 26
incorrect controlling for population stratification, 110
polygenic score applications, 103
Autosomal chromosomes
definition of, 9
heterozygosity across, 90, 203
to identify duplicated or related individuals, 206
in PLINK, 239 

Behavioral traits
complex, 65, 78, 113, 115, 130, 315, 366
and polygenicity, 250
problem in predicting complex ones,
360–363, 365
Belsky, Daniel W., 137, 139
Bias
ascertainment, 367
differential, 104, 110 

Bias (cont.)
due to assortative mating, 349
due to duplicated or related individuals, 218, 236
due to gene-environment correlation, 394
due to heterogeneity in sample or selection, 104, 341
due to pleiotropy, 351
due to population stratification, 206, 231
by genetic confounding, 27, 295
by genetic drift, 60
healthy volunteer, 143, 310
mortality, 65, 367
in MR due to canalization, 348
in MR due to weak instrument, 347
by not considering genetic inheritance, 121
omitted variable, 308, 339–340
publication, 135, 351
reduction by controlling for family fixed effects, 134
in results, 85, 110
simultaneity, 346
systematic by chip differences, study design errors,
or low-quality variants, batch effects, 90,
202–203, 206–207, 211, 249
in use of polygenic score, 110, 244, 273
Bioecological model. See Social compensation model
Bipolar disorder, 44, 104, 113–114, 116
heritability of, 26
Birth weight, 132
Boardman, Jason D., 131, 133, 136, 137, 141, 309–310
Body mass index (BMI), 4, 14, 23, 27, 113, 115, 155,
209, 352, 354
association analysis, 218–222
by birth cohort, 46, 277, 299–307
and FTO gene, 138, 195–196, 245–246
in GCTA relatedness matrix, 238–240
gene-environment correlation, 277, 289, 299–307
heritability of, 26
and mortality in MR, 353
and pleiotropy, 118
polygenic score applications and calculations,
252–253, 257–272, 289–295
in sample data in this book, 175–176, 184, 218,
239, 245
simulation in example data, 389
Bonferroni correction, 84–85
Bottleneck effect, 55, 56, 68–69, 73
Broad Institute, 166, 327

Candidate-gene research, 134–135
Cardiovascular disease, 4, 91, 92, 130, 161, 353
Cell, 3, 5, 11, 15–16, 18, 20, 91, 112, 115, 378–379
cycles, 11
diploid, 12, 16
division and mitosis, 10–12
egg and sperm, 9, 12, 72
eukaryotic, 19
sickle cell disease, 117
somatic, 8, 11 

Centimorgan, 169, 172
Central dogma of molecular biology, 3, 8, 15–20
Cholesterol, 115, 116, 120, 354
Clumping, 226, 248–249, 252–255, 259, 267
Complex traits, 8, 15, 23, 28, 42, 78, 105, 111, 130, 131, 310. See also Genome-wide Complex Trait Analysis
Conley, Dalton C., 142
Consent, 80, 155, 359, 367–369, 370–371, 373
Contextual triggering model. See Diathesis-stress model
Coronary heart disease, 28, 60–61, 91, 353
Correlation, 33–34
bivariate, 25
versus causation, 40–50, 87, 112
estimating genetic, 328–333
gene-environment (rGE), 7, 129, 136, 143–146, 295, 308, 315–316, 320
genetic, 7, 27, 101, 114–117, 288, 294, 348, 350
between genotypes and phenotypes, 339
LD and SNPs, 72, 85, 156, 217, 223–225, 267
phenotypic, 24, 114–115, 134
positive or negative, 41, 42, 93
in twin models, 24–25, 79
zero-order, 131
Covariance, 36–40, 59, 330, 335. See also Variance
Covariance matrix. See Variance-covariance matrix
Culture and cultural, 23, 59, 65, 93, 131–132, 141, 143 

Daw, J., 309
dbGaP—Database of Genotypes and Phenotypes, 163
Deep phenotyping, 124, 179, 371, 379
Dementia, 119, 352, 354–355
Demographic
diversity, 77, 79, 96–97, 162
history, 62, 364
science, 66, 309
variables, 155, 300, 360, 363
Denisovans, 56, 57
Diabetes, 130, 353, 364
heritability of, 26
type 1, 26, 113
type 2, 4, 6, 14, 26, 60–61, 78, 112–113, 115, 116, 159, 168, 362
Diathesis-stress model, 122, 129, 136–140, 138, 307
Differential susceptibility model, 129, 137, 138, 139–140, 305, 310
Diploid cells, 10, 12, 16, 405n7
Direct pleiotropy, 117–119, 120–121, 294, 348, 351–353. See also Indirect pleiotropy
Discordant sex information, 90, 203
identification of individuals with, 205–206
Dominance, 20, 22
Domingue, Benjamin W., 122
Dudbridge, Frank, 103, 120, 406n3 (ch. 5) 

Educational attainment and education, 4, 14, 43, 78, 354
and causal relationship with fertility, 340, 346
conducting GWAS of, 317–320
estimation intergenerational transmission of, 296–299
in G×E models, 139, 176, 250
genetic scores compared to other typical variables, 287
GWAS of, 112, 279, 348
and health, 123, 136
heritability of, 26
and MTAG, 119, 333–336
and natural selection, 65
and number of children ever born, 117
polygenic score applications, 279–295
in rGE models, 144
and social policy applications, 359–360, 362, 365
Endogeneity, 45, 308, 339, 340, 345–346, 352
Epistasis, 22, 105, 244, 349
Erlich, Yaniv, 359, 360, 368
Ethics, 120, 160, 341
and application of polygenic scores, 365
and data governance and management, 163, 180, 369
and data security, 82
and data sharing, 177
and equity, 361
in genomics research, 359–373
and informed consent, 368–369
and personal information risks, 366
and privacy, 367–368
review, 79, 164, 370–372
Ethnicity, 132–133, 367. See also Race
as confounder, 142
as not interchangeable with ancestry, 59
Exome, 18, 405n10
sequencing, 158, 179

Family and families
environment, 140, 146
heritability, 24–26, 104–105, 111, 239
history, 11, 63, 359, 361, 363–365
identifier (ID) in data, 167, 168, 193–194, 200–201, 246, 254
interaction with, 4, 132
members, 16, 23–25, 63, 370
relatedness, 166, 168
secrets and genetics, 366–369
studies, 24–25, 48, 81, 120, 133–134, 231
and within-family analysis (within-family fixed-effect model), 47, 223, 231, 355
Fisher, R. A., 342
infinitesimal model, 107
Fitness, 55–56, 63, 66, 70–71, 73, 111, 117, 378
Founder effect, 55–56, 61, 68–69, 73, 190–191, 202, 236
Freese, Jeremy, 309 

GEDmatch, 367–368
Gene-environment correlation (rGE)
active, 145
definition of, 129, 143–144
evocative, 145
passive, 144
research designs, 146
Gene-environment interaction (G×E)
applied models of, 299–307
bioecological theory of, 137, 138
and causal modeling, 119
challenges and solutions, 141–142, 308–310
and defining environment, 130–133
definition of, 129
diathesis-stress theory of, 136–137
differential susceptibility theory of, 139–140
and history of research, 133–136
research designs, 140
social control theory of, 140
theories of, 136–140
Gene-environment interplay, 129–147
correlation (rGE), 129
interaction (G×E), 118, 129
interaction and heterogeneity, 122–123
Genetic instrumental variable regression (GIV), 352, 355
Genome-wide association study (GWAS), 77–98
correction for multiple testing, 84–85
definition of, 6, 78
heritability, 26, 111, 324–328
heterogeneity, 89–90, 130
history of, 91–93
and lack of diversity, 93–97, 159
Manhattan plots, 85–87, 316–319
meta-analysis, 82, 209–210
NHGRI-EBI GWAS Catalog, 77, 78–79, 91, 93, 97, 164
and polygenic scores, 105
QC, 90–91, 209–210
and Q-Q plot, 36, 320, 321
research design, 79–80
and sample selectivity, 161–162
and sample size, 94, 108
significant SNPs, 108
summary statistics, 114, 164–165, 336
and transferability of results across ancestry groups, 59–61, 110
Genome-wide Complex Trait Analysis (GCTA), 25, 48, 175, 178, 185, 214, 217, 218, 241
calculating genetic relatedness with, 237–238
calculating heritability with, 238–240
installing, 218
Genomic Relationship Matrix (GRM), 48, 238–239
Genotype
calls, 171, 202, 204
correlation between, 85
definition of, 5–6 

Genotype (cont.)
formats and files in PLINK, 166–170, 172–173, 196
frequencies in a population, 59, 69–71, 171, 208
heterozygous or homozygous, 20, 69
imputation of, 156, 158, 171
and interaction with environment, 130–147
in MR, 342–343
proxy, 217, 224–225
quality, 174, 203
rate, 81, 190–192
and relationship to phenotype, 5, 39, 43, 48, 85,
117–118, 218, 220, 367
Genotyping and sequencing arrays, 154–155, 158–159
limitations of arrays, 158
Geography, 55–56, 61–63, 73, 142, 226, 241, 285, 368
ggplot and ggplot2, 229–231, 292–293, 331–332
Goddard, Mike, 382
Guo, Guang, 137 

data, 184, 188–191, 194–196, 199–202, 205, 207, 218, 224 

project, 73, 155–156, 177, 180, 226, 324 

Health and Retirement Survey (HRS), 158, 176, 180  
access to, 163  
cross-trait prediction and genetic covariation using, 288–295  
description of, 155  
gene-environment interaction using, 300–307  
genetic confounding using, 295–299  
genomic data, 166  
polygenic score applications using, 278–288 

Height, 4, 6, 9, 11, 23, 27, 34, 35–39, 43, 60, 66–67, 78, 87–89, 93, 110–111, 116, 155, 250, 323–324, 348 and NBA player Shawn Bradley, 16, 17 and polygenic score application, 289–295, 317–319, 326–331 

Heritability 

broad-sense, 22 

common misconceptions of, 23 

definition of, 22 

estimated from summary statistics, 316, 324–328, 330 

estimated with GCTA, 238–240 

estimates for selected phenotypes, 26 

GWAS-based, 25, 286 

hidden, 27, 111, 288, 309, 362 

missing, 27, 111, 288, 362, 352, 362 

narrow-sense, 22 

SNP-based, 25, 79, 178, 299, 324 

twin or family, 24–25, 79, 111 

Heterogeneity, 27, 46–47, 77, 83–87, 89–90, 110, 112, 122, 164, 204–205, 211, 288, 309 

Homo sapiens, 56–58, 174 

Illumina, 25, 90, 154, 155, 158 

and LD, 155, 179 

quality, 82, 91, 156, 158, 171, 191, 207, 209–210, 268 

reference panels, 159, 180 

software, 180 

uncertainty, 223, 254 

Indirect pleiotropy, 117, 124, 294, 352. See also Direct pleiotropy 

Inheritance, 3, 8, 16, 30, 72, 107, 121, 139, 166, 295, 299, 342, 348 

distinction from heritability, 23 

rules of, 5 

Instrumental Variable (IV) approach, 43, 120, 339–340 

in an MR framework, 343–346 

statistical assumptions, 347–349 

Insurance, 359–360, 367, 369–370 

Intelligence, 60, 139, 266, 359–362 

Intergenerational transmission, 295–297 

Keller, Matthew, 104, 141, 142, 308 

Lambda, 315, 320, 323, 327–328, 330 

LD. See Linkage disequilibrium 

LDpred, 243–245, 267–273, 278, 350 

Lee, Hong, 382 

Linear regression, 39, 82–83, 87, 218–220, 251, 261–265, 283, 324 

Linkage disequilibrium (LD), 55–56, 59, 60, 68, 72, 78, 84, 111, 114, 115, 146, 153, 171, 207, 217, 241, 244, 348 

accounting for in calculating polygenic scores using LDPred, 267–272 

definition of, 21 

and haplotype blocks, 71–73 

identifying independent SNPs through, 223–226 

and imputation, 155–158 

score regression, 114–115, 116, 316, 324, 327–328, 331, 348 

Linux, 74, 184–186, 251, 325, 334 

LocusZoom, 315, 320–321, 322 

Longevity, 90, 379 

macOS, 184–187, 325, 334
Manhattan plot, 7, 79, 336
description of, 85–87
plotting a, 316–319
Martin, Alicia, 60, 103, 110, 266, 367
Mediation model, 34, 43, 45–47, 117
Meiosis, 10–12, 10, 72, 223, 231, 342
Mendelian Randomization (MR), 7, 12, 42–43, 45,
119–120, 244, 299, 339–356
applications, 352–355
bidirectional, 352
in the IV model, 332–347
and randomized control trails, 341–342
using polygenic scores in, 351–352
violation of statistical assumptions of, 347–351 

Mendelian traits, 13–14, 23, 30, 363 

Meta-analysis. See Genome-wide association study: meta-analysis 

Mills, Melinda C., 93, 96, 142, 161 

Minor allele frequency (MAF), 12, 13, 28, 48, 81, 84, 88, 89, 159, 199, 207–208, 210, 224, 225 

Missing heritability. See Heritability, missing 

Mitosis, 10–12, 10. See also Meiosis 

Moderation model and moderators, 34, 43, 45–47, 123, 131, 133, 136, 139, 141, 146, 307 

Molecules, 8, 16, 18–20, 169 

Monogenic traits, 3, 11, 13–15, 78, 101, 105, 243, 245, 359, 361, 372 

Mortality, 65, 118, 162, 352–353, 363, 367, 379 

Multi-Trait Analysis of Genome-wide association summary statistics (MTAG), 7, 119, 316, 333–336 

Mutation, 10–12, 58, 63, 66, 69–73, 115, 117, 208, 223, 361, 363 

de novo, 11 

detailed description of, 11, 66 

germ line, 11, 66 

hereditary, 11 

rare, 158 

somatic or acquired, 10 

Neale, Benjamin M., 79, 98, 109, 165, 315 

Neanderthals, 56, 57, 74 

Neuroticism, 4, 6, 93, 115, 116, 119 

Nonadditive genetic effects, 22–23, 27 

Novembre, John, 60, 61, 226 

Null hypothesis, 39, 83, 85, 240, 256, 265, 320, 331 

Obesity, 23, 46, 96, 104, 113, 123, 132, 134, 138, 141, 195, 245, 250, 259, 300, 307, 352–353, 362–363, 365 

Okbay, Aysu, 279, 280, 317 

Omnigenic traits, 3, 15 

Paternal effects, 296, 299
Personality, 6, 28, 115, 144–145, 340, 346, 406n2 (ch. 2)
Phenotype
binary or dichotomous, 83, 87
complex, 15 (see also Complex traits)
definition of, 6
and genetic correlation, 114
heritability of, 22–26, 48, 111–112
influence of environment on, 22
intermediate, 45
measurement of, 34, 80, 83
polygenic (see Polygenic)
quantitative or continuous, 83, 87
and relationship to genotype, 5, 39, 43, 48, 85,
117–118, 218, 220, 367
shared genetic architecture of, 113–114
sources of heterogeneity of, 89
types of, 13–14
Pleiotropy, 101, 333, 347–348, 362
antagonistic, 117 

definition of 15, 115–119
developmental, 117
direct, 117, 120–121, 294, 330, 348, 350–353
directional, 104
indirect, 117, 294
molecular-gene, 117
selectional, 117
PLINK software, 166–169
association analysis in, 218–223
attaching a phenotype in, 197
binary files, 170, 244, 254, 268
calculating LD scores, 272
checking for LD between two markers in, 223–226
command line environment, 184, 186–187
data formats, 166–170, 172–175
data management, 193–198
descriptive statistics in, 199–200
genetic formats for imputed data, 171–172
genetic relatedness in, 236–238
importing data, 191–192
installation, 184, 382
introduction to, 184
merging genetic files in, 196
missing values in, 200–201
monogenic score construction in, 245–247
notes, warnings, and error messages, 191
opening files, 189
Oxford file formats, 172–173
population stratification in, 226–231, 232–235
QC of genetic data in, 202–211
recoding binary files, 189
running, 186
running scripts in, 188
selecting individuals and markers, 193–196
10 commandments for new users, 187
variant call format (VCF) files, 174–175
in various operating systems, 185
version 2.0, 171
Plomin, Robert, 144, 359, 360
Polygenic. See also Polygenic scores versus monogenic and omnigenic, 13–15, 245, 359, 361
traits, 8, 28
Polygenic scores, 27, 45, 65. See also Polygenic accounting for LD in, 267–272
and application across ancestry groups, 59–61, 110, 264–266
and AUC, 365
Bayesian approach to, 267–272
calculating using LDPred, 267–272
calculating using PRSice, 247–259
challenges and solutions for, 103–104
and common variants, 111
confidence intervals of, 263–264
construction of, 107–108, 243–273
for continuous phenotype, 252
controlling for confounders with, 120–122 

Polygenic scores (cont.)
definition of, 101, 105
and differential bias, 110–111
for education, 65
and gene-environment interaction, 122–123, 130, 135, 299–310
and genetic confounding, 119–120
for height, 16, 38
in HRS, 155, 176
and independent target sample, 109–110
and intelligence, 360–361
as IV (instrumental variable), 346, 351–352
and missing and hidden heritability, 111–112
monogenic, 245–246, 361
and multitrait analysis, 119, 333
normal distribution of, 106
origins of, 105–106
out-of-sample prediction, 278–288
overview of working with, 102
and pleiotropy, 115–119, 362
and policy, 359
and population stratification, 110–111
and predicting other phenotypes, 113–114
pruning and thresholding method in creating, 247–251 $R^{2}$ of, 261–262, 285–286, 287
in regression model with covariates, 261
and relatedness, 110
selection of SNPs for, 108
standardization, 260, 291
summary statistics to create, 164–165
using different p-value thresholds, 255–257, 316
validation and prediction of, 108–109, 260
Polymorphic sites, 3, 8, 20, 210
Population stratification. See also Population structure
and ancestry, 93, 103–104, 110, 206, 230, 231–235
and assumption of independence in MR, 349
controlling for, 82, 87, 221, 228
and geography, 89, 133, 285
and inflation of polygenic score, 110
and inflation of $R^{2}$ , 110, 262–265
and number of principal components to control for, 251
and PCA, 58, 109, 226–235
Q-Q plots to reveal for unaccounted, 211
and use of lambda to detect inflation, 323
use of linear mixed models instead of PCA to control for, 231
Population structure. See also Population stratification
assumptions of, 70
common misnomers of, 59
different LD in, 60
and geography, 62, 285
and stratification, 58–59, 103, 226, 264 

Prediction. See also Predictive power
accuracy and precision, 104, 120, 273,
360
across ancestry groups, 59
cross-trait, 288–295
and environmental G×E influence, 309,
360
and missing and hidden heritability, 111
out-of-sample or independent target sample, 49,
109, 277–279
for personalized medicine, 97, 363–366
polygenic, 16, 102, 104, 107–108, 250
power required for, 119, 251, 256
public understanding of genetic, 366–367
and R², 103–104, 109–110, 251, 256
and understanding biological mechanisms
trade-off, 112–113, 348, 355
and weak instrument assumption in MR, 347
Predictive power, 4, 107, 111, 113, 279, 348–349, 351,
355, 360. See also Prediction
Principal components. See also Principal
Component Analysis (PCA)
and ancestry, 110
graphical representation of, 62
inspection of, 229–235
number to control for, 61, 109, 165, 251, 262,
285
and PCA, 58–59, 226
ranking of, 228
Principal Components Analysis (PCA). See also
Principal components
calculating, 214
definition of, 58–59, 226
example in applied model, 262–265
and geography, 61–62
number to control for, 61, 109, 165, 251, 262,
285
and population stratification, 226–235
Privacy
and consent, 80, 359, 361
protection, 369, 373
and public genetics to solve crimes and find people,
367–368
PRSice
commands, 185, 252, 254–255
to construct polygenic scores, 251–259
for different operating systems, 185, 251–252
how to install, 251, 382–383
input files for, 254–255
types of output in, 253–254
Python
Anaconda distribution, 333
how to install, 267, 383–386
for LDpred software, 267
for LDSC, 324–329
for MTAG, 333–336
versions of, 267 

Quality control (QC)
of genetic data, 90, 202–210
genome-wide association meta-analysis, 80, 82, 165, 171, 209–210
per-individual, 203–206
per-marker, 206–209
protocol, 202
Quantile-Quantile (Q-Q) plot, 36–37, 37, 51, 87, 211, 315, 320–322, 323 

$R^2$ ,72,103-104,109,110-112,120,123,223-225, 249-257,261-265,273,279,285-286,287,311, 320,348 

Race. See also Ethnicity  
is not a biological construct, 56, 59  
is not ancestry, 59  
is a social construct, 56, 59  
and misconceptions in genetics, 60, 133 

Rahal, Charles, 93, 96, 142, 161 

Rare variants, 13–14, 104, 147, 158, 159, 207, 210, 363  
common versus, 16, 97  
and imputation, 156  
and missing and hidden heritability, 27, 111  
in QC, 90 

Replication of research, 48–49, 78, 95, 134–135, 139, 143, 161, 315 

RStudio, 204, 381 

Sample selectivity, 143, 162, 309–310, 367
Sample size
in candidate-gene studies, 134–135
in GWAS, 26–27, 79–80, 89, 91, 93–94, 95,
103–104, 107–108, 110, 119, 159, 279, 288, 333
MAF threshold dependent upon, 208, 210
p-values influenced by, 87–88
required to detect G×E, 142–143, 146, 309
and statistical tests, 34–36, 87–88, 114
target to apply a polygenic score, 120
Sanger sequencing method, 155, 180
Schizophrenia, 14, 26, 116
Sex chromosomes, 9, 196, 199, 205
Sexual dimorphism, 56, 68, 162
Sexual reproduction, 3, 8, 9–10, 28, 144, 342
Shanahan, Michael, 136, 137, 138, 140
Single-nucleotide polymorphisms (SNPs)
ambiguous, 249
chips, 111, 154, 158
definition of, 6, 12–13
diagnostic checks for, 210–211
and GWAS, 78, 159, 164
heritability, 24–26, 48, 111–112, 115, 178, 238–239,
299, 324
in high LD, 224–225
and imputation, 156
to include in polygenic scores, 106, 108, 112, 141,
250, 252, 279
LD pruning to remove redundant, 224–225 

low-call rate of, 207  
with a low MAF, 207  
missingness by, 200  
online browsers, 29, 79, 98  
and polygenic scores, 105  
select independent, 247–248, 267  
selecting one, 195  
and SNPedia, 79, 98  
visualization of, 8  
Smith, George Davey, 343  
Smoking, 11, 14, 42, 45, 115, 122, 131–132, 136, 140–141, 147, 155, 310, 341, 354, 362–363  
Social compensation model, 129, 137–139, 140  
Social control or social push model, 129, 137, 138, 140  
Somatic cells, 8, 10–11, 405n1 

Tropf, Felix C., 104, 142 

Turkheimer, Eric, 119–120, 132–133, 137, 139 

Turley, Patrick, 119, 333 

23andme, 97 

ancestral diversity, 63 

health tests from, 360 

large data, 161, 211 

importing personal data from, 192–193 

privacy and public genetics, 368 

selecting markers, 195 

selective release summary statistics, 288 

Twin studies, 24–25, 48, 78–79, 112–113, 206, 231, 237, 288, 339 

heritability, 24, 299, 328 

and missing heritability, 27, 111, 288 

UK Biobank, 49, 79, 93, 97, 98, 109, 122, 158, 161, 163, 165, 178, 231, 309–310, 315, 336, 353, 367 

Unix, 74, 184–187, 381 

Variance, 34–36, 48, 67, 107, 109, 111, 119, 134, 240, 250, 344. See also Covariance 

dominance, 22 

environmental, 22 

genetic, 22–23, 26–27, 58, 114, 117, 377 

interaction, 22 

inverse weighting, 88–89 

partitioning or decomposition of, 22, 240, 250, 251, 262–265, 285, 288, 36 

phenotypic, 22–25, 120, 130, 133 

sources of, 22, 377 

Variance-covariance matrix, 36–37, 335–336, 403 

Visscher, Peter, 23, 111, 118, 250, 343, 382 

Vulnerability model. See Diathesis-stress model 

Windows, 388 

Wray, Naomi, 103, 110