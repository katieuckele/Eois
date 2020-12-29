## *Eois* genome 

#### Calculate the proportion of Ns in the genome

##### Calculate # of Ns
```
seqtk comp eois_lepidoptera_12Sep2018_zZ6nZ.fasta  | awk '{x+=$9}END{print x}'
# 10543638
```

##### Calculate total number of bases
```
grep -v ">" eois_lepidoptera_12Sep2018_zZ6nZ.fasta | wc | awk '{print $3-$1}'
# 512314333
```

##### Calculate proportion of Ns in the genome
```
10543638/512314333
# [1] 0.02058041 or 2.06%
```
