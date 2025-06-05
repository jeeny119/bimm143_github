# Class 6: R functions
Yoonjin Lim (PID: A16850635)

- [1. Function basics](#1-function-basics)
- [2. Generate DNA sequence](#2-generate-dna-sequence)
- [3. Generate Protein function](#3-generate-protein-function)

## 1. Function basics

Let’s start writing our first silly function to add some numbers:

Every R function has 3 things:

- name (we get to pick this)
- input arguments (there can be loads of these separated by a comma)
- the body (the R code that does the work)

``` r
add <- function(x,y=10, z=0){
  x + y + z
  }
```

I can just use this function like any other function as long as R knows
about it (i.e. run the code chunk)

``` r
add(1, 100)
```

    [1] 101

``` r
add( x=c(1,2,3,4), y=100)
```

    [1] 101 102 103 104

``` r
add(1)
```

    [1] 11

Functions can have “required” input arguments and “optional” input
arguments. The optional arguments are defined with an equals default
value (`y=10`) in the function definition.

``` r
add(x=1, y=100, z=10)
```

    [1] 111

> Q. Write a function to return a DNA sequence of a user speficified
> length? Call it `generate_dna()`

The `sample()` function can help here

``` r
#generate_dna <- function(size=5){ }

students <- c("jeff", "jeremy", "peter")

sample(students, size = 5, replace=TRUE)
```

    [1] "peter"  "jeremy" "jeremy" "jeff"   "jeremy"

## 2. Generate DNA sequence

Now work with `bases` rather than `students`

``` r
bases <- c("A", "C", "G", "T")
sample(bases, size=10, replace=TRUE)
```

     [1] "T" "C" "T" "C" "C" "C" "A" "T" "T" "C"

Now I have a working ‘snippet’ of code I can use this as the body of my
first function version here:

``` r
generate_dna <- function(size=5){ 
  bases <- c("A", "C", "G", "T")
  sample(bases, size=size, replace=TRUE)
  }
```

``` r
generate_dna(100)
```

      [1] "C" "C" "C" "A" "C" "T" "T" "T" "G" "G" "T" "C" "C" "A" "G" "C" "C" "G"
     [19] "T" "C" "T" "A" "T" "T" "T" "T" "A" "G" "T" "G" "C" "C" "C" "A" "T" "G"
     [37] "C" "G" "A" "A" "C" "C" "G" "C" "G" "G" "A" "T" "G" "A" "A" "A" "T" "T"
     [55] "T" "T" "C" "G" "G" "C" "A" "C" "C" "A" "C" "G" "A" "C" "C" "T" "C" "T"
     [73] "G" "G" "C" "G" "C" "G" "T" "G" "T" "C" "T" "C" "T" "G" "G" "T" "A" "A"
     [91] "T" "A" "G" "T" "A" "C" "G" "C" "T" "A"

``` r
generate_dna()
```

    [1] "C" "G" "A" "A" "A"

I want the ability to return a sequence like “AGTACCTG” i.e. a one
element vector where the bases are all together.

``` r
generate_dna <- function(size=5, together=TRUE){ 
  bases <- c("A", "C", "G", "T")
  sequence <- sample(bases, size=size, replace=TRUE)
  
  if(together) {
    sequence <- paste(sequence, collapse= "")
  }
  return(sequence)
  }
```

``` r
generate_dna()
```

    [1] "CACAG"

``` r
generate_dna(together=F)
```

    [1] "T" "A" "T" "T" "C"

## 3. Generate Protein function

We can get the set of 20 natural amino-acids from the **bio3d** package.

``` r
aa <- bio3d::aa.table$aa1[1:20]
```

> Q. Write a protein sequence generating function that will return
> sequences of a user specified length?

``` r
generate_protein <- function(size=6, together=TRUE){
  
  ## Get the 20 amino-acids as a vector 
  aa <- bio3d::aa.table$aa1[1:20]
  sequence_protein <- sample(aa, size=size, replace=TRUE)
  
  ## Optionally return a single element string
  if(together) {
    sequence_protein <- paste(sequence_protein, collapse= "")
  }
  return(sequence_protein)
  }
```

``` r
generate_protein()
```

    [1] "GVGNLW"

> Q. Generate random protein sequences of length 6 to 12 amino acids.

``` r
generate_protein(7)
```

    [1] "EIYQWKY"

``` r
generate_protein(8)
```

    [1] "ETTLYIYE"

``` r
generate_protein(9)
```

    [1] "SFIEYWMPQ"

We can fix this inability to generate multiple sequences by either
editing and adding to the function body code (e.g. a for loop) or by
using the R **apply** family of utility functions.

``` r
sapply(6:12, generate_protein)
```

    [1] "LQALTD"       "PFLNPMR"      "IFELWKQS"     "PMCWDYGMY"    "IEIKANFWCI"  
    [6] "KFQHLQTVTGT"  "IWGDINPWGNQG"

It would be cool and useful if I could get FASTA format output

``` r
ans <- sapply(6:12, generate_protein)
ans
```

    [1] "WPTSFI"       "YNTPIYC"      "EEMNITAA"     "IFLICDAWD"    "MFMCATGTAI"  
    [6] "RFIPAFSATQL"  "CESAWQQYEPDS"

``` r
cat(ans, sep="\n")
```

    WPTSFI
    YNTPIYC
    EEMNITAA
    IFLICDAWD
    MFMCATGTAI
    RFIPAFSATQL
    CESAWQQYEPDS

I want this to look like

    >ID.6 
    YEMMAH
    >ID.7
    FCMSIMN
    >ID.8
    QLDAFIGY

The functions `paste()` and `cat()` can help us here…

``` r
cat(paste(">ID.", 6:12,"\n", ans, sep= ""), sep="\n")
```

    >ID.6
    WPTSFI
    >ID.7
    YNTPIYC
    >ID.8
    EEMNITAA
    >ID.9
    IFLICDAWD
    >ID.10
    MFMCATGTAI
    >ID.11
    RFIPAFSATQL
    >ID.12
    CESAWQQYEPDS

``` r
id.line <- paste(">ID.", 6:12, sep="")
id.line
```

    [1] ">ID.6"  ">ID.7"  ">ID.8"  ">ID.9"  ">ID.10" ">ID.11" ">ID.12"

``` r
id.line <- paste(">ID.", 6:12, sep="")
seq.line <- paste(id.line, ans, sep="\n")
cat(seq.line, sep="\n")
```

    >ID.6
    WPTSFI
    >ID.7
    YNTPIYC
    >ID.8
    EEMNITAA
    >ID.9
    IFLICDAWD
    >ID.10
    MFMCATGTAI
    >ID.11
    RFIPAFSATQL
    >ID.12
    CESAWQQYEPDS

``` r
id.line <- paste(">ID.", 6:12, sep="")
seq.line <- paste(id.line, ans, sep="\n")
cat(seq.line, sep="\n", file="myseq.fa")
```

> Q. Determine if these sequences can be found in nature or are they
> unique? Why or why not?

I BLASTp searched my FASTA format sequences against NR and found that
length 6, 7, 8 are not unique and can be found in the databases with
100% coverage and 100% identity.

Random sequences of length 9 and above are unique and can’t be found in
the databases.
