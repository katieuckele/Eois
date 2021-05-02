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
