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
#### Calculate the proportion of As, Cs, Gs, and Ts in the genome

##### Calculate # of As
```
seqtk comp eois_lepidoptera_12Sep2018_zZ6nZ.fasta  | awk '{x+=$3}END{print x}'
# 161038793
```

##### Calculate proportion of As
```
161038793/512314333
# [1] 0.3143359 or 31.43%
```

##### Calculate # of Cs
```
seqtk comp eois_lepidoptera_12Sep2018_zZ6nZ.fasta  | awk '{x+=$4}END{print x}'
# 89877151
```

##### Calculate proportion of Cs
```
89877151/512314333
# [1] 0.1754336 or 17.54%
```

##### Calculate # of Gs
```
seqtk comp eois_lepidoptera_12Sep2018_zZ6nZ.fasta  | awk '{x+=$5}END{print x}'
# 89866551
```

##### Calculate proportion of Gs
```
89866551/512314333
# [1] 0.1754129 or 17.54%
```

##### Calculate # of Ts
```
seqtk comp eois_lepidoptera_12Sep2018_zZ6nZ.fasta  | awk '{x+=$6}END{print x}'
# 160988200
```

##### Calculate proportion of Ts
```
160988200/512314333
# [1] 0.3142372 or 31.42%
```
