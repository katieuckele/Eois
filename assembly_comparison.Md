## Comparing reference and denovo-based assemblies 

### Note: I ended up going with approach 4. Reference-based assembly with bwa mem

### 1. Reference-based assembly

After aligning to the reference genome, we called variants in a subset of 133 individuals (out of total 198) based on the counts of assembled reads for each individual: 

```
samtools mpileup -P ILLUMINA --BCF --max-depth 1000 --adjust-MQ 50 --min-BQ 20 --min-MQ 20 --skip-indels --output-tags DP,AD --fasta-ref /data/gpfs/assoc/parchmanlab/kuckele/eois/reference/eois_lepidoptera_12Sep2018_zZ6nZ.fasta aln*sorted.bam | \
bcftools call -m --variants-only --format-fields GQ --skip-variants indels | \
bcftools filter --set-GTs . -i 'QUAL > 19 && FMT/GQ >9' | \
bcftools view -m 2 -M 2 -v snps --apply-filter "PASS" --output-type v --output-file eois_variants_rawfiltered_5Jan2021.vcf 
```

The rawfiltered vcf contained 633,815 sites: 

```
$ vcftools --vcf eois_variants_rawfiltered_5Jan2021.vcf

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf eois_variants_rawfiltered_5Jan2021.vcf

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 133 out of 133 Individuals
After filtering, kept 633815 out of a possible 633815 Sites
Run Time = 4.00 seconds
```
After filtering missing data at ```--max-missing=0.8```, thinning by 100, and setting the minor allele frequency to 0.05, we were left with 4,683 out of a possible 633,815 sites: 

```
$ vcftools --vcf eois_variants_rawfiltered_5Jan2021.vcf  --out eois_variants_miss20_14APRIL20 --remove-filtered-all --maf 0.05 --max-missing 0.8 --thin 100 

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf eois_variants_rawfiltered_5Jan2021.vcf
	--maf 0.05
	--thin 100
	--max-missing 0.8
	--out eois_variants_miss20_14APRIL20
	--remove-filtered-all

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 133 out of 133 Individuals
After filtering, kept 4683 out of a possible 633815 Sites
Run Time = 13.00 seconds
```

Removing the thinning parameter produces a larger number of SNPs (10,185/633,815):
```
$ vcftools --vcf eois_variants_rawfiltered_5Jan2021.vcf  --out eois_variants_miss20_14APRIL20 --remove-filtered-all --maf 0.05 --max-missing 0.8

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf eois_variants_rawfiltered_5Jan2021.vcf
	--maf 0.05
	--max-missing 0.8
	--out eois_variants_miss20_14APRIL20
	--remove-filtered-all

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 133 out of 133 Individuals
After filtering, kept 10185 out of a possible 633815 Sites
Run Time = 13.00 seconds
```

### 2. *De novo* assembly

After aligning to the *de novo* genome, we called variants in the same subset of 133 individuals as above, based on the counts of assembled reads for each individual (code same as above): 

```
samtools mpileup -P ILLUMINA --BCF --max-depth 1000 --adjust-MQ 50 --min-BQ 20 --min-MQ 20 --skip-indels --output-tags DP,AD --fasta-ref /data/gpfs/assoc/parchmanlab/kuckele/eois/reference/EO_denovo.fasta aln*sorted.bam | \
bcftools call -m --variants-only --format-fields GQ --skip-variants indels | \
bcftools filter --set-GTs . -i 'QUAL > 19 && FMT/GQ >9' | \
bcftools view -m 2 -M 2 -v snps --apply-filter "PASS" --output-type v --output-file eois_variants_rawfiltered_12APRIL2020.vcf 
```

The rawfiltered vcf contained 456,041 sites: 

```
$ vcftools --vcf eois_variants_rawfiltered_12APRIL2020.vcf 

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf eois_variants_rawfiltered_12APRIL2020.vcf

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 133 out of 133 Individuals
After filtering, kept 456041 out of a possible 456041 Sites
Run Time = 3.00 seconds
```

After filtering missing data at ```--max-missing=0.8```, thinning by 100, and setting the minor allele frequency to 0.05, we were left with 4,939 out of a possible 456,041 sites: 

```
vcftools --vcf eois_variants_rawfiltered_12APRIL2020.vcf --out eois_variants_miss20_14APRIL20 --remove-filtered-all --maf 0.05 --max-missing 0.8 --thin 100 

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf eois_variants_rawfiltered_12APRIL2020.vcf
	--maf 0.05
	--thin 100
	--max-missing 0.8
	--out eois_variants_miss20_14APRIL20
	--remove-filtered-all

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 133 out of 133 Individuals
After filtering, kept 4939 out of a possible 456041 Sites
Run Time = 11.00 seconds
```

### 3. Consensus-based assembly

After aligning to the consensus genome, we called variants in the same subset of 133 individuals as above, based on the counts of assembled reads for each individual (code same as above): 

```
samtools mpileup -P ILLUMINA --BCF --max-depth 1000 --adjust-MQ 50 --min-BQ 20 --min-MQ 20 --skip-indels --output-tags DP,AD --fasta-ref /data/gpfs/assoc/parchmanlab/kuckele/eois/reference/consensus_1DEC20.fa aln*sorted.bam | \
bcftools call -m --variants-only --format-fields GQ --skip-variants indels | \
bcftools filter --set-GTs . -i 'QUAL > 19 && FMT/GQ >9' | \
bcftools view -m 2 -M 2 -v snps --apply-filter "PASS" --output-type v --output-file eois_variants_rawfiltered_11Jan2021.vcf 
```

The rawfiltered vcf contained 438,240 sites: 

```
$ vcftools --vcf eois_variants_rawfiltered_11Jan2021.vcf

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf eois_variants_rawfiltered_11Jan2021.vcf

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 133 out of 133 Individuals
After filtering, kept 438240 out of a possible 438240 Sites
Run Time = 3.00 seconds
```

After filtering missing data at ```--max-missing=0.8```, thinning by 100, and setting the minor allele frequency to 0.05, we were left with 853 out of a possible 438,240 sites: 

```
$ vcftools --vcf eois_variants_rawfiltered_11Jan2021.vcf --out eois_variants_miss20 --remove-filtered-all --maf 0.05 --max-missing 0.8 --thin 100 

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf eois_variants_rawfiltered_11Jan2021.vcf
	--maf 0.05
	--thin 100
	--max-missing 0.8
	--out eois_variants_miss20
	--remove-filtered-all

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 133 out of 133 Individuals
After filtering, kept 853 out of a possible 438240 Sites
Run Time = 9.00 seconds
```

### 4. Reference-based assembly with bwa mem

After aligning to the reference genome with ```bwa mem```, we called variants on the same subset of 133 individuals (out of total 198) based on the counts of assembled reads for each individual: 

```
samtools mpileup -P ILLUMINA --BCF --max-depth 1000 --adjust-MQ 50 --min-BQ 20 --min-MQ 20 --skip-indels --output-tags DP,AD --fasta-ref /data/gpfs/assoc/parchmanlab/kuckele/eois/reference/eois_lepidoptera_12Sep2018_zZ6nZ.fasta aln*sorted.bam | \
bcftools call -m --variants-only --format-fields GQ --skip-variants indels | \
bcftools filter --set-GTs . -i 'QUAL > 19 && FMT/GQ >9' | \
bcftools view -m 2 -M 2 -v snps --apply-filter "PASS" --output-type v --output-file eois_variants_rawfiltered_12Jan2021.vcf 
```
The rawfiltered vcf contained 724,163 sites: 
```
$ vcftools --vcf eois_variants_rawfiltered_12Jan2021.vcf

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf eois_variants_rawfiltered_12Jan2021.vcf

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 133 out of 133 Individuals
After filtering, kept 724163 out of a possible 724163 Sites
Run Time = 6.00 seconds
```
After filtering missing data at ```--max-missing=0.8```, thinning by 100, and setting the minor allele frequency to 0.05, we were left with 5,150 out of a possible 724,163 sites: 
```
$ vcftools --vcf eois_variants_rawfiltered_12Jan2021.vcf  --out eois_variants_miss20 --remove-filtered-all --maf 0.05 --max-missing 0.8 --thin 100

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf eois_variants_rawfiltered_12Jan2021.vcf
	--maf 0.05
	--thin 100
	--max-missing 0.8
	--out eois_variants_miss20
	--remove-filtered-all

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 133 out of 133 Individuals
After filtering, kept 5150 out of a possible 724163 Sites
Run Time = 15.00 seconds
```
### 5. Consensus-based assembly with bwa mem

After aligning to the reference genome with ```bwa mem```, we called variants on the same subset of 133 individuals (out of total 198) based on the counts of assembled reads for each individual: 
```
samtools mpileup -P ILLUMINA --BCF --max-depth 1000 --adjust-MQ 50 --min-BQ 20 --min-MQ 20 --skip-indels --output-tags DP,AD --fasta-ref /data/gpfs/assoc/parchmanlab/kuckele/eois/reference/consensus_1DEC20.fa aln*sorted.bam | \
bcftools call -m --variants-only --format-fields GQ --skip-variants indels | \
bcftools filter --set-GTs . -i 'QUAL > 19 && FMT/GQ >9' | \
bcftools view -m 2 -M 2 -v snps --apply-filter "PASS" --output-type v --output-file eois_variants_rawfiltered_15Jan2021.vcf 
```
The rawfiltered vcf contained 570,923 sites: 
```
$ vcftools --vcf eois_variants_rawfiltered_15Jan2021.vcf 

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf eois_variants_rawfiltered_15Jan2021.vcf

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 133 out of 133 Individuals
After filtering, kept 570923 out of a possible 570923 Sites
Run Time = 3.00 seconds
```
After filtering missing data at ```--max-missing=0.8```, thinning by 100, and setting the minor allele frequency to 0.05, we were left with 943 out of a possible 570,923 sites: 
```
$ vcftools --vcf eois_variants_rawfiltered_15Jan2021.vcf  --out eois_variants_miss20 --remove-filtered-all --maf 0.05 --max-missing 0.8 --thin 100

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf eois_variants_rawfiltered_15Jan2021.vcf
	--maf 0.05
	--thin 100
	--max-missing 0.8
	--out eois_variants_miss20
	--remove-filtered-all

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 133 out of 133 Individuals
After filtering, kept 943 out of a possible 570923 Sites
Run Time = 11.00 seconds
```








