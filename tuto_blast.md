# Command line BLAST+
* Use BLAST+, rather than the old blast, which is removed from the official NCBI FTP
## Module load
```bash
module load bioinfo/NCBI_Blast+/2.15.0+
```
## Changes vs BLAST
* blastall -p blastn changed to blastn
* blastall -p blastp changed to blastp
* etc.
## Available programs
* Database creation: makeblastdb
* Search in databases: blastn, blastp, blastx, tblastn, tblastx
* Complete list of executables (the elements in the same folder as blastn): 
```bash
ls $(dirname $(which blastn))
```
## Help
```bash
# compact
blastn -h
# extended
blastn -help
```
## Make a BLAST database
```bash
# create directory 
mkdir /home/ikoyo/genomes
mkdir /home/ikoyo/genomes/HAv3_1
cd /home/ikoyo/genomes/HAv3_1
# copy genome sequence locally (could have been downloaded from NCBI)
cp /home/vignal/save/Genomes/Abeille/HAv3_1_indexes/GCF_003254395.2_Amel_HAv3.1_genomic.fna .
# Build database
makeblastdb -in /home/ikoyo/genomes/HAv3_1/GCF_003254395.2_Amel_HAv3.1_genomic.fna -dbtype ‘nucl’
```
## Blast sequences
* For instance in a directory /home/ikoyo/Blast_results (create Blast_results with mkdir)
```bash
# general
blastn -query /path/to/fasta/file -db /path/to/blast_database -outfmt 6 -out /path/to/out_file
# For instance:
blastn -query /home/ikoyo/cox_aja_sah.fasta -db /home/ikoyo/genomes/HAv3_1/GCF_003254395.2_Amel_HAv3.1_genomic.fna  -outfmt 6 -out /home/ikoyo/Blast_results/blast_table.txt
``` 
* -outfmt 6 => tabular format without headers => can do analyses in R, pandas or other
* -outfmt 7 => headers as comments with the column names, but not useable directly for import!
* See blastn -help for all other options