---
title: "BASH and Others"	
---

[Home](../index.md)

# Unix commands
## grep
### Select several words in a line
* Both present in the line:
```{bash, eval = FALSE}
grep "word1" file.txt | grep "word2" #slow if many occurences of word1 in large file
grep  'word1.word2' file.txt #If word1 always before word2
grep  'word1.*word2|word2.*word1' file.txt #
```
* One or the other in the line:
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
## Select a range of lines from a file
```{bash, eval = FALSE}
sed -n '5,20p' TestHiveChr16.sync
awk 'NR>=5&&NR<=20' TestHiveChr16.sync 
```
## Sort
+ Text sort column 1 and numeric on column 2
```{bash, eval = FALSE}
sort -k1,1 -k2,2n file.txt
```
## Copy when links
* To copy symbolic links in the present directory, while still pointing to the same files
```{bash, eval = FALSE}
cp -P */RawData/* .
```
## Obtain the fastq files from NG6
* Copy the symbolic links to a directory (see above)
* In that directory, to copy the fastq files in the directory fastqs:
```{bash, eval = FALSE}
nohup rsync -avz --copy-unsafe-links *.fastq.gz fastqs &
```

# BASH
## Loop
### Print a list of bamfiles from a serie of folders:
##### Output in columns:
```{bash, eval = FALSE}
for i in `ls ITSAP*/*.bam`
do
echo $i
done
```
##### Output in lines, separator = blank space:
```{bash, eval = FALSE}
for i in `ls ITSAP*/*.bam`; do printf  "$i "; done
```
##### Output in lines, separator = blank tab:
```{bash, eval = FALSE}
for i in `ls ITSAP*/*.bam`; do printf  "$i\t"; done
```
## Variables
### General
```{bash}
var=2
echo $var
```
### Separate using internal field separators
```{bash}
var=One_Two_Three
echo $var
echo ${var%_*}
echo ${var%%_*}
echo ${var#*_}
echo ${var##*_}
```

### Separate using internal field separators on an array
```{bash, eval = FALSE}
BAMS=(${DIR}/*L00*/*.bam)	#generates a list of paths
SAMPLES=(${BAMS[@]##*/})
```

### Internal field separator
##### Only used during word splitting of data, for example, when coming from a variable
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
### Uppercase and lowercase
```{bash}
myString="heLLo wOrld"
echo $myString
```

```{bash}
myString="heLLo wOrld"
echo $myString | tr [A-Z] [a-z]
echo $myString | tr [a-z] [A-Z]
```
## Arrays
### Declaration
```{bash}
list=(Tom Porter John Lennon)
echo ${list[1]}
list=('Tom Porter' 'John Lennon')		#Allows for spaces
echo ${list[1]}
list=("Tom Porter" "John Lennon")		#Allows for spaces
echo ${list[1]}
```
### Accession to elements
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
### Assignment of values
```{bash}
arr[2]=2
echo ${arr[2]}			#print the second element
echo ${arr[*]}			#is the only element
arr[7]=7
echo ${arr[*]}			#only two elements
echo ${#arr[*]}			#number of elements is two
```
### for loop on all elements
```{bash}
list=(Tom Porter John Lennon)
for i in ${list[*]:0:${#list[*]}};do
	echo "Name "$i
done
```
### Rename files (change extentions)
##### Can also be done with sed or rename
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

### Search and replace
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

### Add element at the end
```{bash}
array=(1 2 3 4 5 6 7 8 9)
array=("${array[*]}" "10" "11")
echo ${array[*]}
array=(${array[*]} 12 13)
echo ${array[*]}
echo ${array[11]}
```
## echo

##### The -e argument allows for escaping:
```{bash}
echo "\nText\there\n--------------\n\n"
```
```{bash}
echo -e "\nText\there\n--------------\n\n"
```

## Testing time on files
* To check if the *.vcf.gz.tbi file is more recent than the *.vcf.gz:
    * -nt : newer than
    * -ot : older than
```{bash, eval=FALSE}
for i in `ls *.vcf.gz`; do if [ ${i} -nt ${i}.tbi ] ; then echo ${i} NO; else echo ${i} OK; fi; done | grep NO$
```

# Obtain a backuped file on genologin
* find the file
```{bash, eval=FALSE}
ls /work/.snapshots/
```

* then copy from the directory with the date corresponding to the snapshot we are interested in:
```{bash, eval=FALSE}
cp /work/.snapshots/snap_gpfs_2018-09-06-00-00-23/project/cytogen/Alain/seqapipopOnHAV3_AV/callingDiplo/combinedVcf/combineGVCFsHAV3Called_slurm.bash .
```

# Transpose a line into a column with datamash
```{bash, eval=FALSE}
datamash -W transpose < wholeList > wholeList2
```

# One way synchronisation with rsync
# #main options:

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
## typical copy from work to save:
    * directory_or_file will be written in directory, giving /save/path/to/directory/directory_or_file
```{bash, eval = FALSE}
rsync -avzu /work/path/to/directory_or_file /save/path/to/directory
```

## To download fasq files from NG6 by using the links:
    * -L option will transform symlink into referent file/dir
```{bash, eval = FALSE}
nohup sh -c 'for i in `ls /home/gencel/vignal/work/Analysis/Project_ROYALBEE.301/Run*/RawData/*fastq.gz`; do rsync -avLz ${i} /genphyse/cytogen/seqapipop/FastqFromNG6 1>>rsyncLog; done' &
```
# nohup
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

# SED
## Remove blank lines
```{bash, eval=FALSE}
sed -i '/^$/d' file.txt
```
The -i means it will edit the file in-place

# [AWK](../notes/AWK.md)
