---
title: "BASH and Others"
output:
  html_document:
    toc: true		
---

#Other documents
###[Beginner's guide](file:///Users/alainvignal/Documents/Documentations/BASH/GuideBeginners/tldp.org/LDP/Bash-Beginners-Guide/html/index.html)

#Unix commands
##grep
###Select several words in a line
Both present in the line:
```{bash, eval = FALSE}
grep "word1" file.txt | grep "word2" #slow if many occurences of word1 in large file
grep  'word1.word2' file.txt #If word1 always before word2
grep  'word1.*word2|word2.*word1' file.txt #
```
One or the other in the line:
```{bash, eval = FALSE}
grep 'word1\|word2\|word3' /path/to/file
### Search all text files ###
grep 'word*' *.txt
### Search all python files for 'wordA' or 'wordB' ###
grep 'wordA*'\''wordB' *.py
grep -E 'word1|word2' *.doc
grep -e string1 -e string2 *.pl
egrep "word1|word2" *.c
ls | grep '_L\|_merged' | grep -v ALG | wc
```
##Select a range of lines from a file
```{bash, eval = FALSE}
sed -n '5,20p' TestHiveChr16.sync
awk 'NR>=5&&NR<=20' TestHiveChr16.sync 
```
##Sort
+ Text sort column 1 and numeric on column 2
```{bash, eval = FALSE}
sort -k1,1 -k2,2n file.txt
```
##Copy when links
* To copy symbolic links in the present directory, while still pointing to the same files
```{bash, eval = FALSE}
cp -P */RawData/* .
```
##Obtain the fastq files from NG6
* Copy the symbolic links to a directory (see above)
* In that directory, to copy the fastq files in the directory fastqs:
```{bash, eval = FALSE}
nohup rsync -avz --copy-unsafe-links *.fastq.gz fastqs &
```

#BASH
##Loop
###Print a list of bamfiles from a serie of folders:
#####Out put in columns:
```{bash, eval = FALSE}
for i in `ls ITSAP*/*.bam`
do
echo $i
done
```
#####Output in lines, separator = blank space:
```{bash, eval = FALSE}
for i in `ls ITSAP*/*.bam`; do printf  "$i "; done
```
#####Output in lines, separator = blank space:
```{bash, eval = FALSE}
for i in `ls ITSAP*/*.bam`; do printf  "$i\t"; done
```
##Variables
###General
```{bash}
var=2
echo $var
```
###Separate using internal field separators
```{bash}
var=One_Two_Three
echo $var
echo ${var%_*}
echo ${var%%_*}
echo ${var#*_}
echo ${var##*_}
```

###Separate using internal field separators on an array
```{bash, eval = FALSE}
BAMS=(${DIR}/*L00*/*.bam)	#generates a list of paths
SAMPLES=(${BAMS[@]##*/})
```

###Internal field separator
#####Only used during word splitting of data, for example, when coming from a variable
```{bash}
printf "%s" "$IFS" | od -bc
IFS=:
printf "%s" "$IFS" | od -bc
```
Back to standard:
```{bash}
IFS=$' \t\n'
printf "%s" "$IFS" | od -bc
```
###Uppercase and lowercase
```{bash}
myString="heLLo wOrld"
echo $myString
```

```{bash}
myString="heLLo wOrld"
echo $myString | tr [A-Z] [a-z]
echo $myString | tr [a-z] [A-Z]
```
##Arrays
[Arrays essential](file:///Users/alainvignal/Documents/Documentations/BASH/The%20Ultimate%20Bash%20Array%20Tutorial%20with%2015%20Examples.html)

###Declaration
```{bash}
list=(Tom Porter John Lennon)
echo ${list[1]}
list=('Tom Porter' 'John Lennon')		#Allows for spaces
echo ${list[1]}
list=("Tom Porter" "John Lennon")		#Allows for spaces
echo ${list[1]}
```
###Accession to elements
```{bash}
array=(one two three four five)
echo $array				#only prints the first element
echo $array[1]			#prints the first element followed by the text
echo ${array[1]}		#prints the second element
echo ${array[*]}		#all the array
echo ${array[@]}		#allthe array
echo ${#array[@]}		#number of elements
echo ${#array[1]}		#length of element 2
echo ${array[@]:1:3}	#extract 3 elements, offset 1
echo ${array[2]:1:3}	#extract 3 characters, offset 1, fo element three
```
###Assignment of values
```{bash}
arr[2]=2
echo ${arr[2]}			#print the second element
echo ${arr[*]}			#is the only element
arr[7]=7
echo ${arr[*]}			#only two elements
echo ${#arr[*]}			#number of elements is two
```
###for loop on all elements
```{bash}
list=(Tom Porter John Lennon)
for i in ${list[*]:0:${#list[*]}};do
	echo "Name "$i
done
```
###Rename files (change extentions)
#####Can also be done with sed or rename
```{bash}
ls Directory/
```
```{bash}
for i in Directory/*.txt; do mv "$i" "${i/.txt/}.csv"; done 
ls Directory/
```
```{bash}
for i in Directory/*.csv; do mv "$i" "${i/.csv/}.txt"; done
ls Directory/
```

###Search and replace
```{bash}
array=(1 2 3 4 5 6 7 8 9)
array=(${array[*]/5/50})
echo ${array[*]}
```
```{bash}
array=(1 2 3 4 5 6 7 8 9)
array[5]=60
echo ${array[*]}
```
```{bash}
Unix=('Debian' 'Red hat' 'Ubuntu' 'Suse' 'Fedora' 'UTS' 'OpenLinux')
echo ${Unix[@]}
echo ${Unix[@]/Ubuntu/SCO Unix}
```
```{bash}
Unix=('Debian' 'Red hat' 'Ubuntu' 'Suse' 'Fedora' 'UTS' 'OpenLinux')
Unix2=(${Unix[@]/Ubuntu/SCO Unix})
echo ${Unix2[*]}
echo ${Unix[2]}
echo ${Unix2[2]} 	#the spaces have become separators in the process
```

###Add element at the end
```{bash}
array=(1 2 3 4 5 6 7 8 9)
array=("${array[*]}" "10" "11")
echo ${array[*]}
array=(${array[*]} 12 13)
echo ${array[*]}
echo ${array[11]}
```
##echo
###
#####The -e argument allows for escaping:
```{bash}
echo "\nText\there\n--------------\n\n"
```
```{bash}
echo -e "\nText\there\n--------------\n\n"
```

##Testing time on files
* To check if the *.vcf.gz.tbi file is more recent than the *.vcf.gz:
    * -nt : newer than
    * -ot : older than
```{bash, eval=FALSE}
for i in `ls *.vcf.gz`; do if [ ${i} -nt ${i}.tbi ] ; then echo ${i} NO; else echo ${i} OK; fi; done | grep NO$
```

#Obtain a backuped file on genologin
* find the file
```{bash, eval=FALSE}
ls /work/.snapshots/
```

* then copy from the directory with the date corresponding to the snapshot we are interested in:
```{bash, eval=FALSE}
cp /work/.snapshots/snap_gpfs_2018-09-06-00-00-23/project/cytogen/Alain/seqapipopOnHAV3_AV/callingDiplo/combinedVcf/combineGVCFsHAV3Called_slurm.bash .
```

#Transpose a line into a column with datamash
```{bash, eval=FALSE}
datamash -W transpose < wholeList > wholeList2
```

#One way synchronisation with rsync
##main options:

        -a : equivalent to -rlptgoD
            -r : recusrive
            -l : copy symlinks as symlinks
            -p : copy permissions
            -t : copy timestamp
            -g : copy group
            -o : copy owner
            -D : equivalent to --devices --specials
        -z : compression
        -u : skip files that are more recent on the reciever
        -v : verbose
        -L : transform symlink into referent file/dir
        --stats
        --log-file=/path/to/logFileTes
##typical copy from work to save:
    * directory_or_file will be written in directory, giving /save/path/to/directory/directory_or_file
```{bash, eval = FALSE}
rsync -avzu /work/path/to/directory_or_file /save/path/to/directory
```

##To download fasq files from NG6 by using the links:
    * -L option will transform symlink into referent file/dir
```{bash, eval = FALSE}
nohup sh -c 'for i in `ls /home/gencel/vignal/work/Analysis/Project_ROYALBEE.301/Run*/RawData/*fastq.gz`; do rsync -avLz ${i} /genphyse/cytogen/seqapipop/FastqFromNG6 1>>rsyncLog; done' &
```
#nohup
* nohup sh -c allows a for loop:
```{bash, eval = FALSE}
nohup sh -c 'for i in `ls /home/gencel/vignal/work/Analysis/Project_ROYALBEE.301/Run*/RawData/*fastq.gz`; do rsync -avLz ${i} /genphyse/cytogen/seqapipop/FastqFromNG6 1>>rsyncLog; done' &
```
* call a script:
```{bash, eval = FALSE}
 nohup sh copyGvcfsToSave &
```
* call a script, screen output to file:
```{bash, eval = FALSE}
 nohup sh copyGvcfsToSave 1>>log &
```

#SED
##Remove blank lines
```{bash, eval=FALSE}
sed -i '/^$/d' file.txt
```
The -i means it will edit the file in-place

#AWK
```{bash}
more table1.txt
```
###Sum of third column values
```{bash}
awk 'BEGIN{somme=0}{somme +=$3}END{print somme}' table1.txt
```

```{bash}
awk 'BEGIN {OFS="\t";sum=0}{print $1, $3; sum += $3} END {print "Sum", sum}' table1.txt
```
###Select columns on the base of values
```{bash}
awk '$5 == 9{print$1,$2,$6}' table1.txt
```
```{bash}
awk -F "\t" '$5 == 9{print$1"\t"$2"\tOK",$6}' table1.txt
```
```{bash}
awk '$5 == 9{print$0}' table1.txt
```
```{bash}
awk -F "\t" '$7 != "NO" {print $1"\t"$2}' table1.txt
```
####Multiple column selection. Example in pileup file
```{bash, eval = FALSE}
awk '($1 == "NC_007079.3" && $2 >= 152380 && $2 < 152400 ) {print$0}' file.pileup
```
###Sorting while keeping header
```{bash}
more table2.txt
```
```{bash}
awk 'NR == 1; NR > 1 {print $0 | "sort -n"}'  table2.txt  	#-n => numeric
```
```{bash}
awk 'NR == 1; NR > 1 {print $0 | "sort -k7"}' table2.txt	# -k7 => column 7
```
###Selection while keeping a header
```{bash}
awk -F"\t" 'NR == 1;NR > 1 {if($7 == "OK") {print $0}}' table2.txt
```
###Selection, limited columns and sort
```{bash}
awk -F"\t" 'NR == 1{print $1,$2,$3};NR > 1 {if($7 == "OK") {print $1,$2,$3 | "sort -k3" }}' table2.txt
```
###Replace characters (solves the problem when inserting tabulations with sed)
```{bash}
more table3.txt
awk '{gsub(";","\t",$0);print}' table3.txt
```
```{bash, eval=FALSE}
awk '{gsub("Primary Assembly","PrimaryAssembly");print}' Galgal5Stats.txt
```
###Input and output separators in lists
```{bash}
more list.txt
awk 'BEGIN{FS="_";OFS=";"}{print $2,$4}' list.txt
awk 'BEGIN{FS="_";OFS="\t"}{print $2,$4}' list.txt
```

###Alleles on 2 lines in input file. Print to 2 columns
#####awk '{ if (NR%2==0) {print $1,$3,$4,prev ;} else { prev=$4 ;}}'

###One column into a line
#####With a separator specified by ORS
```{bash}
cat Col1.txt
awk 'BEGIN{ORS=" | "}{print $1}' Col1.txt
```
#####and remove last separator:
```{bash}
awk 'BEGIN{ORS=" | "}{print $1}' Col1.txt | sed s/...$//
```

###Delete a list of jobs on the cluster
```{bash}
for i in `qstat -u vignal | grep Eqw | awk 'BEGIN{ORS=" "}{print $1}'`; do qdel ${i}; done
```

###Length distribution of PacBio FASTQ file
```{bash, eval = FALSE}
awk 'NR%4 == 2 {lengths[length($0)]++} END {for (l in lengths) {print l, lengths[l]}}' file.fastq
```
It reads like this: every second line in every group of 4 lines (the sequence line), measure the length of the sequence and increment the array cell corresponding to that length. When all lines have been read, loop over the array to print its content. In awk, arrays and array cells are initialized when they are called for the first time, no need to initialize them before.

Other solution (easier, especially if gzipped files by using zcat) :
```{bash, eval = FALSE}
cat reads.fastq | awk '{if(NR%4==2) print length($1)}' | sort -n | uniq -c > read_length.txt
```

###Coverage in a pileup file
#####Problem: many more bases whan there is an insertion. In fact, the coverage is simply in the 4th column !
```{bash, eval = FALSE}
awk -F "\t" -v fld=5 '{print NR "\t" gsub(/[ACGTacgt]/,"",$fld)}' test.pileup
```
#####Or for a limited number of bases:
```{bash, eval = FALSE}
awk -F "\t" -v fld=5 '($1 == "NC_007079.3" && $2 >= 3000000 && $2 <= 3100000) {print $2 "\t" gsub(/[ACGTacgt]/,"",$fld)}' ITSAP100-1B-50_CTGAAGCT-AGGCTATA_L003.pileup > coverageChr10Part.pileup
```

###Extract the two fasta file names from the NG6 links in a project folder
#####To prepare SRA_metadata.xlsx file for submitting to SRA 

In the project folder: /home/gencel/vignal/work/Analysis/Project_ROYALBEE.301

* Find the files with the name (here: Sav for the Savoie sequences)
* Select only the fastq files (grep fast)
* Get the file names (4th column with "/"" as Fiels Separator)
* From one list into two columns
* Copy and paste in Excel

```{bash, eval = FALSE}
find . -name Sav* | grep fast | awk 'BEGIN {FS="/"} ; {print$4}' | awk '{ if (NR%2==0) {print $1,"\t",prev ;} else { prev=$1 ;}}'
```

###Change values in 2 columns of an agp file
```{bash, eval = FALSE}
awk -F '\t' 'BEGIN{OFS="\t"} $8=="no" {$8="yes";$9="align_xgenus"}{print$0}' chromosomes.agp
```

###Join

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

