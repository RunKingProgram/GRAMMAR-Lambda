# GRL

## 1.Getting started

### 1.1	Downloading GRL

GRL can be downloaded from https://github.com/YuxinSong-prog/GRL. It can be installed as a regular R package.

### 1.2	Installing GRL

**GRL** links to R packages Rcpp, RcppEigen and RcppArmadillo, and also imports R packages BEDMatrix and data.table. These dependencies should be installed before installing GRL. In addition, GRL **requires PLINK2.0 Software (http://www.cog-genomics.org/plink/2.0/) with name “plink2.0” under your working directory**. Here is an example for installing GRL and all its dependencies in an R session(assuming none of the R packages other than the default has been installed):
```
install.packages(c("BEDMatrix ", " data.table ", "Rcpp", " RcppEigen ", " RcppArmadillo "), repos = "https://cran.r-project.org/")
system(“R CMD install GRL_1.0.tgz”)
```

## 2. Input

GRL requires the phenotype and genotype files in an **PLINK BED** format which also called PLINK 1 binary file and the structure of these files is described in http://www.cog-genomics.org/plink/1.9/formats#bed. Preparation of these data are described below.

### 2.1 Phenotype

Phenotype should place in the sixth column of “.fam” file. 

### 2.2 Genotypes

GRL can only take genotype files in PLINK “.bed” and “.bim” format.

## 3. Running GRL
If **GRL** has been successfully installed, you can load it in an R session using:<br>
```
library(GRL)
```
and then loading "BEDMatrix "and " data.table " <br>
```
library(BEDMatrix)
library(data.table)
```
We provide three functions in **GRL**: **Data** function for basic data management including extracting phenotypic values, sampling markers, calculating allele frequencies and GRM (saved in external files); **Grammar** function for estimating heritability (optional) and breeding values, implementing association tests using GRAMMAR method and **Grl** function for adjusting statistics using *Lambda*, implementing joint analysis based on the results of GRAMMAR-Lambda, and outputting association result files.

### 3.1 Data function
#### Usage
```
Data (filename, nsmar, msampling)
```
#### Arguments
#### filename
An object class of character: the filename before the suffix of plink files. The three plink file must have a same filename, for example, “filename.bed”, “filename.bim” and “filename.bam”.<br>
#### nsmar
An optional numeric for the number of sampling markers. If this is unset, the “nsmar” default is 5,000.<br>
#### msampling
logicals. If FLASE, entire markers will be used to calculate GRM. If this is unset or for large scale dataset, the “msampling” default is TURE.

### 3.2 Grammar function
#### Usage
```
Grammar (Data, Estimateh2, AssoTst)
```

#### Arguments

#### Data
An object class of list from the 1st step.
#### Estimateh2
logicals. If TURE, heritability will be estimated and the estimate would be used for estimating breeding value. If this is unset or for large scale dataset, a moderate heritability is set by default at 0.5.
#### AssoTst
You can choose correlation analysis software “GCTA” or “PLINK2.0” as prefer, “PLINK2.0” by default.


### 3.3 GRL
#### Usage
```
Grl(Grammar, Test, QQ,Manh)
```
#### Arguments

#### Grammar
An object class of list from the 2st step.
#### Test
An optional for association test, a test at once or joint analysis. You can choose “Separate” for a test at once and “Joint” for further joint analysis based on the result of “Separate”.
#### QQ
logicals. If TURE, Q-Q plot would be drawn.
#### Manh
logicals. If TURE, Q-Q plot would be drawn.

## 4. Output

The **Grammar** function generates a plink association text output file called “Grammar.PHENO1.glm.linear”. Here we show the header and the first five rows of the example output:

CHROM|	POS|	ID|	REF|	ALT|	A1|	TEST|	OBS_CT|	BETA|	SE|	T_STAT|	P|	ERRCODE
---- | ----- | ------ | ------| ------| ------| ------| ------| ------| ------| ------| ------| ------
1|	10390|	S1_10390	|G|	A|	A|	ADD|	2648|	-0.00784112|	0.238845|	-0.0328293	|0.973813|	|.
1|	10590|	S1_10590	|T|	A|	A|	ADD|	2648|	-0.202364|	0.249746|	-0.810281	|0.417852|	|.
1|	128373|	S1_128373|	C|	T|	T|	ADD|	2648|	0.033819|	0.124565|	0.271498	|0.78603|	|.
…


In addition to the above file, a QTN candidate file, named “QTNs”, with Bonferroni as the threshold is the output of **Grl** function. If joint analysis, a file, named “jointQTNs”, will display candidate QTNs resulting from joint analysis, which is the same format as “QTNs”.

## 5. Example
```
library(GRL)
library(BEDMatrix)
library(data.table)

setwd("./example")
plinkfilename = "geno"
gdata <- Data(plinkfilename)
grammar <- Grammar(gdata)
Grl(grammar)
```
