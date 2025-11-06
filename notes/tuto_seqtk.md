[Home](../index.md)

# seqtk

```bash
module load bioinfo/Seqtk/1.3
```
## Extract several sequences from a multifasta file
```bash
# Make a list of the sequences in the file:
grep '>' multifasta_file.fa | awk 'BEGIN{FS=">"}{print $2}' > list.tsv
# Edit the list with text editor, such as nano, by keeping the lines for the sequences to keep, then:
seqtk subseq  multifasta_file.fa list.tsv > multifasta_file_subset.fa
```
## Extract several regions (which could be gene coordinates)
* These can be retrieved from the gtf annotation file with grep and awk !
```bash
# Make list of regions TAB delimited file (tsv)
# Can also be prepared and modified with the nano text editor
cat > regions.list
NC_001566.1 1000    1300
NC_001566.1 1670    1700
NC_001566.1 2578    2788

seqtk subseq /home/vignal/save/Genomes/Abeille/HAv3_1_indexes/GCF_003254395.2_Amel_HAv3.1_genomic.fna regions.list > regions.fa
```
* You get a multifasta file

## Extract one region from a chromosome
* Positions 1001 to 2000 of HAv3.1 chromosome 1
```bash
echo -e "NC_037638.1\t1000\t2000" | seqtk subseq /home/vignal/save/Genomes/Abeille/HAv3_1_indexes/GCF_003254395.2_Amel_HAv3.1_genomic.fna -
# equivalent to:
seqtk subseq /home/vignal/save/Genomes/Abeille/HAv3_1_indexes/GCF_003254395.2_Amel_HAv3.1_genomic.fna <(echo -e "NC_037638.1\t1000\t2000")
```
* All the mitochondrial genome
```bash
echo -e "NC_001566.1" | seqtk subseq /home/vignal/save/Genomes/Abeille/HAv3_1_indexes/GCF_003254395.2_Amel_HAv3.1_genomic.fna - > HAv3_1_mitoch.fa
```
## Bonus : prepare the file for extracting the genes
* download the gff annotation file from NCBI
```bash
awk 'BEGIN{OFS="\t"}$1 == "NC_001566.1" && $3 == "gene" {print $1, $4, $5, $9}' genomic.gff | grep -e protein -e rRNA | awk 'BEGIN{OFS="\t"}{print $1, $2, $3}'
```
