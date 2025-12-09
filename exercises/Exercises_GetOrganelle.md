# Exercises on GetOrganelle analyses

Once analyses are done using the script Assemble_mitoch.bash and sarray, the scaffold.fasta files are each in a folder having the sample name, with a path such as:

~/sample/extended_spades/scaffold.fasta

## Exercise 1

Obtain the sequence identifiers of all scaffolds for all samples. Will give something such as:

```bash
UK1A_GTCCGC_L003/extended_spades/scaffolds.fasta:>NODE_145_length_287_cov_1.988372
UK1A_GTCCGC_L003/extended_spades/scaffolds.fasta:>NODE_146_length_286_cov_4.035088
UK1A_GTCCGC_L003/extended_spades/scaffolds.fasta:>NODE_147_length_286_cov_3.549708
UK1A_GTCCGC_L003/extended_spades/scaffolds.fasta:>NODE_148_length_282_cov_3.712575
UK1A_GTCCGC_L003/extended_spades/scaffolds.fasta:>NODE_149_length_282_cov_3.275449
UK1A_GTCCGC_L003/extended_spades/scaffolds.fasta:>NODE_150_length_281_cov_2.951807
UK1A_GTCCGC_L003/extended_spades/scaffolds.fasta:>NODE_151_length_280_cov_2.969697
```
* This can be done using grep.

## Exercise 2

Make a table with columns separated by tabulations, that can be imported into R, having:
* Sample_name as column 1
* Node number as column 2
* Scaffold length as column 3
* Coverage (depth) as comumn 4

Such as:
```
UK19A	1	16850	118.390977
UK19A	2	2550	11.027105
UK19A	3	373	4.624031
UK1A	1	16889	122.443544
UK1A	2	6336	104.620479
```
* This will use the previous grep command, piped into awk
* Field separators can be specified in the awk BEGIN{FS="_";OFS="\t"}
* "\t" is for tabulation

## Exercise 3

It is quite possible that NODE_1 is always the one we want. Check this by making the same table with only the nodes 1.

## Exercise 4

If after inspection, the nodes 1 are the correct ones, what needs to be done, is extracting the first sequence of the scaffold.fasta file for each sample into a file named after the sample. You can use seqtk for this or I just found seqkit, which seems to be better.

Example use of seqkit:
```bash
module load bioinfo/SeqKit/2.9.0
seqkit head -n 1 in.fa > out.fa # will extract the first sequence of a multifasta file. -n 2 will extract the two first, etc.
```

And the name of the sequence can be replaced using sed:

```bash
sed -i "s/^>.*/>$SEQNAME/" ${OUTDIR}/${SEQNAME}.fa
```
Try using a loop like:

```bash
for i in ${INDIR}/*/extended_spades/scaffolds.fasta
```



