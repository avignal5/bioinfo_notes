
[Home](../index.md)

# AWK  <!-- omit in toc -->
- [Basic AWK: select lines, columns and choose field separator](#basic-awk-select-lines-columns-and-choose-field-separator)
  - [Select one or several columns](#select-one-or-several-columns)
  - [Select columns and lines on the basis of values](#select-columns-and-lines-on-the-basis-of-values)
  - [Output separated by tabulations: "\\t"](#output-separated-by-tabulations-t)
  - [Add a column with some text](#add-a-column-with-some-text)
  - [Print all columns](#print-all-columns)
  - [lines where column 7 is not "NO"](#lines-where-column-7-is-not-no)
  - [Multiple column selection](#multiple-column-selection)
  - [Selection while keeping a header](#selection-while-keeping-a-header)
  - [Selection while keeping a header with sorting](#selection-while-keeping-a-header-with-sorting)
  - [Selection, limited columns and sort](#selection-limited-columns-and-sort)
  - [Using different field separators](#using-different-field-separators)
  - [Replace characters (solves the problem when inserting tabulations with sed) same as using FS and OFS (above)?](#replace-characters-solves-the-problem-when-inserting-tabulations-with-sed-same-as-using-fs-and-ofs-above)
- [Sum of third column values](#sum-of-third-column-values)
- [One column into a line](#one-column-into-a-line)
- [Delete a list of jobs on the cluster](#delete-a-list-of-jobs-on-the-cluster)
- [Length distribution of PacBio FASTQ file](#length-distribution-of-pacbio-fastq-file)
- [Coverage in a pileup file](#coverage-in-a-pileup-file)
  - [Or for a limited number of bases:](#or-for-a-limited-number-of-bases)
- [Extract the two fasta file names from the NG6 links in a project folder](#extract-the-two-fasta-file-names-from-the-ng6-links-in-a-project-folder)
  - [To prepare SRA\_metadata.xlsx file for submitting to SRA](#to-prepare-sra_metadataxlsx-file-for-submitting-to-sra)
- [Change values in 2 columns of an agp file](#change-values-in-2-columns-of-an-agp-file)
- [Join](#join)

# Basic AWK: select lines, columns and choose field separator

* Created a few tables, such as "table1.txt for the examples

```bash
more /Users/avignal/Documents/Documentations/BASH/Directory/table1.txt
```

    1       3       6       3       8       4       OK
    5       6       7       1       9       0       NO
    4       5       2       8       9       3       OK

## Select one or several columns
```bash
awk '{print $2, $4, $7}' /Users/avignal/Documents/Documentations/BASH/Directory/table1.txt
```

    3 3 OK
    6 1 NO
    5 8 OK


## Select columns and lines on the basis of values

```bash
# Columns 1, 2 and 6 if column 5 equals to 5
awk '$5 == 9{print $1, $2, $6}' /Users/avignal/Documents/Documentations/BASH/Directory/table1.txt
```

    5 6 0
    4 5 3

## Output separated by tabulations: "\t"
```bash
# Same, but output OFS separated by tabulations: "\t"
awk 'BEGIN{OFS="\t"}$5 == 9{print$1,$2,$6}' /Users/avignal/Documents/Documentations/BASH/Directory/table1.txt
```

    5	6	0
    4	5	3

## Add a column with some text
```bash
# Add a column with some text
awk 'BEGIN{OFS="\t"} $5 == 9{print$1,$2,"text",$6}' /Users/avignal/Documents/Documentations/BASH/Directory/table1.txt
```

    5	6	text	0
    4	5	text	3

## Print all columns

```bash
# print all columns
awk '$5 == 9{print}' /Users/avignal/Documents/Documentations/BASH/Directory/table1.txt
```

    5	6	7	1	9	0	NO
    4	5	2	8	9	3	OK


## lines where column 7 is not "NO"

```bash
# lines where column 7 is not "NO"
awk 'BEGIN{OFS="\t"} $7 != "NO" {print $1,$2}' /Users/avignal/Documents/Documentations/BASH/Directory/table1.txt
```

    1	3
    4	5


## Multiple column selection
```bash
# Multiple column selection
awk 'BEGIN{OFS="\t"} $7 != "NO" && $2 == 3 {print $1,$2}' /Users/avignal/Documents/Documentations/BASH/Directory/table1.txt
```

    1	3


## Selection while keeping a header


```bash
more /Users/avignal/Documents/Documentations/BASH/Directory/table2.txt
```

    One     Two     Three   Four    Five    Six     Seven
    1       3       6       3       8       4       OK
    5       6       7       1       9       0       NO
    4       5       2       8       9       3       OK
    [K[?1l>/avignal/Documents/Documentations/BASH/Directory/table2.txt (END)[27m[K


```bash
awk 'BEGIN{OFS="\t"} NR == 1;NR > 1 {if($7 == "OK") {print}}' /Users/avignal/Documents/Documentations/BASH/Directory/table2.txt
```

    One	Two	Three	Four	Five	Six	Seven
    1	3	6	3	8	4	OK
    4	5	2	8	9	3	OK


## Selection while keeping a header with sorting


```bash
awk 'BEGIN{OFS="\t"} NR == 1;NR > 1 {if($7 == "OK") {print | "sort -k 3"}}' /Users/avignal/Documents/Documentations/BASH/Directory/table2.txt
```

    One	Two	Three	Four	Five	Six	Seven
    4	5	2	8	9	3	OK
    1	3	6	3	8	4	OK

## Selection, limited columns and sort

```
awk -F"\t" 'NR == 1{print $1,$2,$3};NR > 1 {if($7 == "OK") {print $1,$2,$3 | "sort -k3" }}' table2.txt
```

## Using different field separators
* FS = input file separator
* OFS = output file separator


```bash
more /Users/avignal/Documents/Documentations/BASH/Directory/list.txt

```

    One_Two_Three_Four
    Five_Six_Seven_Eigh
    Nine_Ten_Eleven_Twelve


```bash
awk 'BEGIN{FS="_";OFS=";"}{print $2,$4}' /Users/avignal/Documents/Documentations/BASH/Directory/list.txt
```

    Two;Four
    Six;Eigh
    Ten;Twelve



```bash
awk 'BEGIN{FS="_";OFS="\t"}{print $2,$4}' /Users/avignal/Documents/Documentations/BASH/Directory/list.txt
```

    Two	Four
    Six	Eigh
    Ten	Twelve

## Replace characters (solves the problem when inserting tabulations with sed) same as using FS and OFS (above)?
```
more table3.txt
awk '{gsub(";","\t",$0);print}' table3.txt
```
```
awk '{gsub("Primary Assembly","PrimaryAssembly");print}' Galgal5Stats.txt
```

# Sum of third column values
```
awk 'BEGIN{somme=0}{somme +=$3}END{print somme}' table1.txt
```

# One column into a line
* With an output separator specified by ORS
```
cat Col1.txt
awk 'BEGIN{ORS=" | "}{print $1}' Col1.txt
```
* and remove last separator:
```
awk 'BEGIN{ORS=" | "}{print $1}' Col1.txt | sed s/...$//
```

# Delete a list of jobs on the cluster
```
for i in `qstat -u vignal | grep Eqw | awk 'BEGIN{ORS=" "}{print $1}'`; do qdel ${i}; done
```

# Length distribution of PacBio FASTQ file
```
awk 'NR%4 == 2 {lengths[length($0)]++} END {for (l in lengths) {print l, lengths[l]}}' file.fastq
```
It reads like this: every second line in every group of 4 lines (the sequence line), measure the length of the sequence and increment the array cell corresponding to that length. When all lines have been read, loop over the array to print its content. In awk, arrays and array cells are initialized when they are called for the first time, no need to initialize them before.

Other solution (easier, especially if gzipped files by using zcat) :
```
cat reads.fastq | awk '{if(NR%4==2) print length($1)}' | sort -n | uniq -c > read_length.txt
```

# Coverage in a pileup file
* Problem: many more bases whan there is an insertion. In fact, the coverage is simply in the 4th column !
```{bash, eval = FALSE}
awk -F "\t" -v fld=5 '{print NR "\t" gsub(/[ACGTacgt]/,"",$fld)}' test.pileup
```
## Or for a limited number of bases:
```{bash, eval = FALSE}
awk -F "\t" -v fld=5 '($1 == "NC_007079.3" && $2 >= 3000000 && $2 <= 3100000) {print $2 "\t" gsub(/[ACGTacgt]/,"",$fld)}' ITSAP100-1B-50_CTGAAGCT-AGGCTATA_L003.pileup > coverageChr10Part.pileup
```

# Extract the two fasta file names from the NG6 links in a project folder
## To prepare SRA_metadata.xlsx file for submitting to SRA 

In the project folder: /home/gencel/vignal/work/Analysis/Project_ROYALBEE.301

* Find the files with the name (here: Sav for the Savoie sequences)
* Select only the fastq files (grep fast)
* Get the file names (4th column with "/"" as Fiels Separator)
* From one list into two columns
* Copy and paste in Excel

```{bash, eval = FALSE}
find . -name Sav* | grep fast | awk 'BEGIN {FS="/"} ; {print$4}' | awk '{ if (NR%2==0) {print $1,"\t",prev ;} else { prev=$1 ;}}'
```

# Change values in 2 columns of an agp file
```{bash, eval = FALSE}
awk -F '\t' 'BEGIN{OFS="\t"} $8=="no" {$8="yes";$9="align_xgenus"}{print$0}' chromosomes.agp
```
# Join

file A

chr1   123 aa b c d
chr1   234 a  b c d
chr1   345 aa b c d
chr1   456 a  b c d
....

file B

xxxx  abcd    chr1   123    aa    c    d    e
yyyy  defg    chr1   345    aa    e    f    g
...

I want to join the two files based on 3 columns with "chr1", "123" and "aa" and add first two columns from file B to file A, such that output looks as shown below: output:

chr1   123    aa    b    c    d    xxxx    abcd
chr1   234    a     b    c    d
chr1   345    aa    b    c    d    yyyy    defg
chr1   456    a    b    c    d

```{bash, eval=FALSE}
$ awk 'NR==FNR{a[$3,$4]=$1OFS$2;next}{$6=a[$1,$2];print}' OFS='\t' fileb filea
chr1    123     a    b    c     xxxx    abcd
chr1    234     a    b    c 
chr1    345     a    b    c     yyyy    defg
chr1    456     a    b    c 
```

Explanation:

NR==FNR             # current recond num match the file record num i.e in filea => from start to end of file 2
a[$3,$4]=$1OFS$2    # Create entry in array with fields 3 and 4 as the key => assign columns 1 and 2 of file 2, using OFS as separator, to a hash having columns 2 and 4 as keys
next                # Grab the next line (don't process the next block)
$6=a[$1,$2]         # Assign the looked up value to field 6 (+rebuild records)  => value of the hash assigned to field 6 of file 1
print               # Print the current line & the matching entry from fileb ($6)

OFS='\t'            # Seperate each field with a single TAB on output

