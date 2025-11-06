---
title: "BASH and Others"	
---

[Home](../index.md)

- [Unix commands](#unix-commands)
  - [grep](#grep)
    - [Select several words in a line](#select-several-words-in-a-line)
  - [Select a range of lines from a file](#select-a-range-of-lines-from-a-file)
  - [Sort](#sort)
  - [Copy when links](#copy-when-links)
  - [Obtain the fastq files from NG6](#obtain-the-fastq-files-from-ng6)
- [BASH](#bash)
  - [Loop](#loop)
    - [Print a list of bamfiles from a serie of folders:](#print-a-list-of-bamfiles-from-a-serie-of-folders)
        - [Output in columns:](#output-in-columns)
        - [Output in lines, separator = blank space:](#output-in-lines-separator--blank-space)
        - [Output in lines, separator = blank tab:](#output-in-lines-separator--blank-tab)
  - [Variables](#variables)
    - [General](#general)
    - [Separate using internal field separators](#separate-using-internal-field-separators)
    - [Separate using internal field separators on an array](#separate-using-internal-field-separators-on-an-array)
    - [Internal field separator](#internal-field-separator)
        - [Only used during word splitting of data, for example, when coming from a variable](#only-used-during-word-splitting-of-data-for-example-when-coming-from-a-variable)
    - [Uppercase and lowercase](#uppercase-and-lowercase)
  - [Arrays](#arrays)
    - [Declaration](#declaration)
    - [Accession to elements](#accession-to-elements)
    - [Assignment of values](#assignment-of-values)
    - [for loop on all elements](#for-loop-on-all-elements)
    - [Rename files (change extentions)](#rename-files-change-extentions)
        - [Can also be done with sed or rename](#can-also-be-done-with-sed-or-rename)
    - [Search and replace](#search-and-replace)
    - [Add element at the end](#add-element-at-the-end)
  - [echo](#echo)
        - [The -e argument allows for escaping:](#the--e-argument-allows-for-escaping)
  - [Testing time on files](#testing-time-on-files)
- [Diverse applications](#diverse-applications)
  - [Obtain a backuped file on genologin](#obtain-a-backuped-file-on-genologin)
  - [Transpose a line into a column with datamash](#transpose-a-line-into-a-column-with-datamash)
- [One way synchronisation with rsync](#one-way-synchronisation-with-rsync)
  - [#main options:](#main-options)
    - [typical copy from work to save:](#typical-copy-from-work-to-save)
    - [To download fasq files from NG6 by using the links:](#to-download-fasq-files-from-ng6-by-using-the-links)
- [nohup](#nohup)
- [SED](#sed)
  - [Remove blank lines](#remove-blank-lines)
- [Notes on AWK](#notes-on-awk)


# Unix commands
## grep
### Select several words in a line
* Both present in the line:

```
grep "word1" file.txt | grep "word2" #slow if many occurences of word1 in large file
grep  'word1.word2' file.txt #If word1 always before word2
grep  'word1.*word2|word2.*word1' file.txt #
```
* One or the other in the line:

```
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
## Select a range of lines from a file
```
sed -n '5,20p' TestHiveChr16.sync
awk 'NR>=5&&NR<=20' TestHiveChr16.sync 
```
## Sort
* Text sort column 1 and numeric on column 2

```
sort -k1,1 -k2,2n file.txt
```
## Copy when links
* To copy symbolic links in the present directory, while still pointing to the same files

```
cp -P */RawData/* .
```
## Obtain the fastq files from NG6
* Copy the symbolic links to a directory (see above)
* In that directory, to copy the fastq files in the directory fastqs:

```
nohup rsync -avz --copy-unsafe-links *.fastq.gz fastqs &
```

# BASH
## Loop
### Print a list of bamfiles from a serie of folders:
##### Output in columns:
```
for i in `ls ITSAP*/*.bam`
do
echo $i
done
```
##### Output in lines, separator = blank space:
```
for i in `ls ITSAP*/*.bam`; do printf  "$i "; done
```
##### Output in lines, separator = blank tab:
```
for i in `ls ITSAP*/*.bam`; do printf  "$i\t"; done
```
## Variables
### General
```
var=2
echo $var
```
### Separate using internal field separators
```
var=One_Two_Three
echo $var
echo ${var%_*}
echo ${var%%_*}
echo ${var#*_}
echo ${var##*_}
```

### Separate using internal field separators on an array
```
BAMS=(${DIR}/*L00*/*.bam)	#generates a list of paths
SAMPLES=(${BAMS[@]##*/})
```

### Internal field separator
##### Only used during word splitting of data, for example, when coming from a variable
```
printf "%s" "$IFS" | od -bc
IFS=:
printf "%s" "$IFS" | od -bc
```
Back to standard:
```
IFS=$' \t\n'
printf "%s" "$IFS" | od -bc
```
### Uppercase and lowercase
```
myString="heLLo wOrld"
echo $myString
```

```
myString="heLLo wOrld"
echo $myString | tr [A-Z] [a-z]
echo $myString | tr [a-z] [A-Z]
```
## Arrays
### Declaration
```
list=(Tom Porter John Lennon)
echo ${list[1]}
list=('Tom Porter' 'John Lennon')		#Allows for spaces
echo ${list[1]}
list=("Tom Porter" "John Lennon")		#Allows for spaces
echo ${list[1]}
```
### Accession to elements
```
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
### Assignment of values
```
arr[2]=2
echo ${arr[2]}			#print the second element
echo ${arr[*]}			#is the only element
arr[7]=7
echo ${arr[*]}			#only two elements
echo ${#arr[*]}			#number of elements is two
```
### for loop on all elements
```
list=(Tom Porter John Lennon)
for i in ${list[*]:0:${#list[*]}};do
	echo "Name "$i
done
```
### Rename files (change extentions)
##### Can also be done with sed or rename
```
ls Directory/
```
```
for i in Directory/*.txt; do mv "$i" "${i/.txt/}.csv"; done 
ls Directory/
```
```
for i in Directory/*.csv; do mv "$i" "${i/.csv/}.txt"; done
ls Directory/
```

### Search and replace
```
array=(1 2 3 4 5 6 7 8 9)
array=(${array[*]/5/50})
echo ${array[*]}
```
```
array=(1 2 3 4 5 6 7 8 9)
array[5]=60
echo ${array[*]}
```
```
Unix=('Debian' 'Red hat' 'Ubuntu' 'Suse' 'Fedora' 'UTS' 'OpenLinux')
echo ${Unix[@]}
echo ${Unix[@]/Ubuntu/SCO Unix}
```
```
Unix=('Debian' 'Red hat' 'Ubuntu' 'Suse' 'Fedora' 'UTS' 'OpenLinux')
Unix2=(${Unix[@]/Ubuntu/SCO Unix})
echo ${Unix2[*]}
echo ${Unix[2]}
echo ${Unix2[2]} 	#the spaces have become separators in the process
```

### Add element at the end
```
array=(1 2 3 4 5 6 7 8 9)
array=("${array[*]}" "10" "11")
echo ${array[*]}
array=(${array[*]} 12 13)
echo ${array[*]}
echo ${array[11]}
```
## echo

##### The -e argument allows for escaping:
```
echo "\nText\there\n--------------\n\n"
```
```
echo -e "\nText\there\n--------------\n\n"
```

## Testing time on files
* To check if the *.vcf.gz.tbi file is more recent than the *.vcf.gz:
    * -nt : newer than
    * -ot : older than

```
for i in `ls *.vcf.gz`; do if [ ${i} -nt ${i}.tbi ] ; then echo ${i} NO; else echo ${i} OK; fi; done | grep NO$
```

# Diverse applications

## Obtain a backuped file on genologin
* find the file

```
ls /work/.snapshots/
```

* then copy from the directory with the date corresponding to the snapshot we are interested in:

```
cp /work/.snapshots/snap_gpfs_2018-09-06-00-00-23/project/cytogen/Alain/seqapipopOnHAV3_AV/callingDiplo/combinedVcf/combineGVCFsHAV3Called_slurm.bash .
```

## Transpose a line into a column with datamash
```
datamash -W transpose < wholeList > wholeList2
```

# One way synchronisation with rsync
## #main options:

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
### typical copy from work to save:
* directory_or_file will be written in directory, giving /save/path/to/directory/directory_or_file

```
rsync -avzu /work/path/to/directory_or_file /save/path/to/directory
```

### To download fasq files from NG6 by using the links:
* -L option will transform symlink into referent file/dir

```
nohup sh -c 'for i in `ls /home/gencel/vignal/work/Analysis/Project_ROYALBEE.301/Run*/RawData/*fastq.gz`; do rsync -avLz ${i} /genphyse/cytogen/seqapipop/FastqFromNG6 1>>rsyncLog; done' &
```
# nohup
* nohup sh -c allows a for loop:

```
nohup sh -c 'for i in `ls /home/gencel/vignal/work/Analysis/Project_ROYALBEE.301/Run*/RawData/*fastq.gz`; do rsync -avLz ${i} /genphyse/cytogen/seqapipop/FastqFromNG6 1>>rsyncLog; done' &
```
* call a script:

```
 nohup sh copyGvcfsToSave &
```
* call a script, screen output to file:

```
 nohup sh copyGvcfsToSave 1>>log &
```

# SED
## Remove blank lines
```
sed -i '/^$/d' file.txt
```
The -i means it will edit the file in-place

# Notes on [AWK](../notes/AWK.md)
