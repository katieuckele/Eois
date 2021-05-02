# Inferring dated *Eois* phylogeny with RevBayes

## Determining the prior node distributions

From Strutzenberger et al. 2017

"Neotropical Eois were dated to an age of 24.2Ma (20.3–28.4Ma 95%HPD)."

## Determining the sampling proportion

From Strutzenberger and Fiedler, 2011: 

"Hence, the overall species count of Eois is likely to be considerably higher than 250, and a rough estimate yields total species numbers for Neotropical Eois somewhere from 733 to 1710 species (G. Brehm, F. Bodner, P. Strutzenberger, F. Hunefeld & K. Fiedler,
submitted). 

From Brehm et al., 2011: 

"Simple richness estimations based on the ratio of described versus undescribed species lead to values between 733 and 1,710 Neotropical Eois species in total (Table 2)."

To calculate the number of GBS samples in the Jahner et al. 2017 dataset, 

first, I read in the Supplementary table 1 from Jahner et al. 2017
```
s1 <- read.csv("~/Google_Drive/Uckele/eois_olivacea/RevBayes/Jahner2017_TableS1.csv")
```
next, I count the number of rows of data for which GBS = 1
```
> sum(s1$GBS == 1)
[1] 137
```
To calculate our proportion of *Eois* sampled, I divide the number of GBS samples by the larger of the two estimates of *Eois* species richness
```
> 137/1710
[1] 0.08011696
```

## Ipyrad Param File
FASTQ files for this analysis are located in the following repository on Pronghorn:
```
/data/gpfs/assoc/denovo/kuckele/eois_phylo/fastq/RevBayes
```
The ipyrad params file (params-EoisRevBayes.txt): 
```
------- ipyrad params file (v.0.9.62)-------------------------------------------
EoisRevBayes                   ## [0] [assembly_name]: Assembly name. Used to name output directories for assembly ste$
/data/gpfs/assoc/denovo/kuckele/eois_phylo/ipyrad ## [1] [project_dir]: Project dir (made in curdir if not present)
                               ## [2] [raw_fastq_path]: Location of raw non-demultiplexed fastq files
                               ## [3] [barcodes_path]: Location of barcodes file
/data/gpfs/assoc/denovo/kuckele/eois_phylo/fastq/RevBayes/*fastq.gz          ## [4] [sorted_fastq$
reference                         ## [5] [assembly_method]: Assembly method (denovo, reference)
/data/gpfs/assoc/denovo/kuckele/eois_genome/fasta/eois_lepidoptera_12Sep2018_zZ6nZ.fasta                               ## [6] [reference_sequence]: Location of reference sequence file
ddrad                            ## [7] [datatype]: Datatype (see docs): rad, gbs, ddrad, etc.
                         ## [8] [restriction_overhang]: Restriction overhang (cut1,) or (cut1, cut2)
5                              ## [9] [max_low_qual_bases]: Max low quality base calls (Q<20) in a read
33                             ## [10] [phred_Qscore_offset]: phred Q score offset (33 is default and very standard)
6                              ## [11] [mindepth_statistical]: Min depth for statistical base calling
6                              ## [12] [mindepth_majrule]: Min depth for majority-rule base calling
10000                          ## [13] [maxdepth]: Max cluster depth within samples
0.9                           ## [14] [clust_threshold]: Clustering threshold for de novo assembly
0                              ## [15] [max_barcode_mismatch]: Max number of allowable mismatches in barcodes
2                              ## [16] [filter_adapters]: Filter for adapters/primers (1 or 2=stricter)
35                             ## [17] [filter_min_trim_len]: Min length of reads after adapter trim
2                              ## [18] [max_alleles_consens]: Max alleles per site in consensus sequences
0.05                           ## [19] [max_Ns_consens]: Max N's (uncalled bases) in consensus
0.05                           ## [20] [max_Hs_consens]: Max Hs (heterozygotes) in consensus
70                              ## [21] [min_samples_locus]: Min # samples per locus for output
0.2                            ## [22] [max_SNPs_locus]: Max # SNPs per locus
8                              ## [23] [max_Indels_locus]: Max # of indels per locus
0.5                            ## [24] [max_shared_Hs_locus]: Max # heterozygous sites per locus
0, 0, 0, 0                     ## [25] [trim_reads]: Trim raw read edges (R1>, <R1, R2>, <R2) (see docs)
0, 0, 0, 0                     ## [26] [trim_loci]: Trim locus edges (see docs) (R1>, <R1, R2>, <R2)
p, s, l, n                        ## [27] [output_formats]: Output formats (see docs)
```
Sbatch file (sbatch_eoisRevBayes.sh): 
```
#!/usr/bin/env bash
#SBATCH --account=cpu-s1-bionres-0
#SBATCH --partition=cpu-s1-bionres-0
#SBATCH --cpus-per-task=32
#SBATCH --job-name=ipyrad
#SBATCH --output=ipyrad_eoisRevBayes.txt
#SBATCH --mem-per-cpu=3500M
#SBATCH --time=7-00:00:00

source activate py36

## change into the directory where your params file resides
cd /data/gpfs/assoc/denovo/kuckele/eois_phylo/ipyrad/

## call ipyrad on your params file
ipyrad -f -p params-EoisRevBayes.txt -s 1234567 -c 32
```

