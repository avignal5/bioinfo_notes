[Home](../index.md)

# seqkit
* To be compared eventually with [seqtk](tuto_seqtk.md), having simiular functions, which is also on the cluster

```bash
module load bioinfo/SeqKit/2.9.0
```

## Length of sequences
* fx2tab converts fasta/q to tabular format
* -l : sequence length
* -n : prints names only (otherwise, you get also the sequence)
* -i : prints only the identifier (otherwise you get the description)
```bash
seqkit fx2tab -lni seq.fa
```