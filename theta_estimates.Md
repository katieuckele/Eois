## Estimating thetas with angsd

### Install angsd
#### Notes from Trevor: 
```
conda install -c bioconda angsd=0.923 (only this version works)
```
Then you get the error: 
```
libcrypto.so.1.0.0: cannot open shared object file: No such file or directory
```

Looking in your lib directory for the environment you downloaded it as, you will have a libcrypto.so.1.X.X and you need to create a symbolic link so it can find the one you have.

For example:
```
ln -s libcrypto.so.1.1 libcrypto.so.1.0.0
```

### Finding a 'global estimate' of the SFS
```
source activate py36
angsd -bam bam.filelist -doSaf 1 -anc /data/gpfs/assoc/parchmanlab/kuckele/eois/reference/eois_lepidoptera_12Sep2018_zZ6nZ.fasta -GL 1 -P 24 -out out 
realSFS out.saf.idx -P 24 -fold 1 -nSites 50000000 > eois.sfs
```
