[Home](../index.md)

# MAFFT
 
```bash
module load bioinfo/MAFFT/7.505
```
## Documentation
* https://mafft.cbrc.jp/alignment/software/manual/manual.html?utm_source=chatgpt.com
* https://mafft.cbrc.jp/alignment/software/algorithms/algorithms.html?utm_source=chatgpt.com
* https://mafft.cbrc.jp/alignment/software/tips0.html
* In fact, the best is the paper itself :
    * Katoh, K., & Standley, D. M. (2013). MAFFT Multiple sequence alignment software version 7: Improvements in performance and usability. Molecular Biology and Evolution, 30(4), 772–780. https://doi.org/10.1093/molbev/mst010


```bash
mafft --genafpair  --adjustdirection --maxiterate 16 --reorder test.mfa > test_mafft.mfa
```
* --genafpair for short sequences and high precision
* --maxiterate 16 : I found some suggestions with --maxiterate 1000, including in mafft -h