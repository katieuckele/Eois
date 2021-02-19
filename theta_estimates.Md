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

### 1. Estimate the site allele frequency likelihood

	-bam <INPUT> = input of bam names for population
	-doSaf <INPUT> = option 1: calculate the site allele frequency likelihood based on individual genotype likelihoods assuming HWE
	-anc <INPUT> = ancestral fasta file (i.e. the genome)
	-GL <INPUT> = genotype likelihood model (1 = SAMtools; 2 = GATK; 3 = SOAPsnp; 4 = SYK)
	-P <INPUT> = number of cores used
	-out <INPUT> = outfile prefix
```	
angsd -bam group1bams.txt -doSaf 1 -anc /data/gpfs/assoc/parchmanlab/kuckele/eois/reference/eois_lepidoptera_12Sep2018_zZ6nZ.fasta -GL 1 -P 2 -out grp1_drkblue &
angsd -bam group2bams.txt -doSaf 1 -anc /data/gpfs/assoc/parchmanlab/kuckele/eois/reference/eois_lepidoptera_12Sep2018_zZ6nZ.fasta -GL 1 -P 2 -out grp2_cream &
angsd -bam group3bams.txt -doSaf 1 -anc /data/gpfs/assoc/parchmanlab/kuckele/eois/reference/eois_lepidoptera_12Sep2018_zZ6nZ.fasta -GL 1 -P 2 -out grp3_ltblue &
angsd -bam group4bams.txt -doSaf 1 -anc /data/gpfs/assoc/parchmanlab/kuckele/eois/reference/eois_lepidoptera_12Sep2018_zZ6nZ.fasta -GL 1 -P 2 -out grp4_mustard &
```

### 2. Obtain the maximum likelihood estimate of the folded SFS

	-P <INPUT> = number of cores used
```
realSFS grp1_drkblue.saf.idx -P 2 -fold 1 > grp1_drkblue.sfs 
realSFS grp2_cream.saf.idx -P 2 -fold 1 > grp2_cream.sfs 
realSFS grp3_ltblue.saf.idx -P 2 -fold 1 > grp3_ltblue.sfs 
realSFS grp4_mustard.saf.idx -P 2 -fold 1 > grp4_mustard.sfs 
```

### 3. Calculate the thetas for each site
```
realSFS saf2theta grp1_drkblue.saf.idx -sfs grp1_drkblue.sfs -outname grp1_drkblue
realSFS saf2theta grp2_cream.saf.idx -sfs grp2_cream.sfs  -outname grp2_cream
realSFS saf2theta grp3_ltblue.saf.idx -sfs grp3_ltblue.sfs -outname grp3_ltblue
realSFS saf2theta grp4_mustard.saf.idx -sfs grp4_mustard.sfs -outname grp4_mustard

thetaStat print grp1_drkblue.thetas.idx 2>/dev/null > grp1_drkblue_persite_thetas.txt
thetaStat print grp2_cream.thetas.idx 2>/dev/null > grp2_cream_persite_thetas.txt
thetaStat print grp3_ltblue.thetas.idx 2>/dev/null > grp3_ltblue_persite_thetas.txt
thetaStat print grp4_mustard.thetas.idx 2>/dev/null > grp4_mustard_persite_thetas.txt
```

| Pop          | Pi         | Pi_low     | Pi_high    | W          | W_low      | W_high     |
|--------------|------------|------------|------------|------------|------------|------------|
| grp1_drkblue | 0.00368015 | 0.00367607 | 0.00368423 | 0.0056467  | 0.00564272 | 0.00565068 |
| grp2_cream   | 0.00488115 | 0.00487622 | 0.00488608 | 0.00577686 | 0.0057728  | 0.00578092 |
| grp3_ltblue  | 0.00520844 | 0.00520399 | 0.00521289 | 0.00663696 | 0.0066339  | 0.00664001 |
| grp4_mustard | 0.00389922 | 0.00389206 | 0.00390638 | 0.00430013 | 0.00429368 | 0.00430658 |
