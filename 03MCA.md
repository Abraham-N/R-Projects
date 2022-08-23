# Multiple Correspondence Analysis {#mca}



## Introduction

Multiple correspondence analysis (MCA) extends correspondence analysis (CA). It allows for the analysis of the pattern of relationships of more than one categorical dependent variable. It can also be seen as a generalization of principal component analysis when when the variables analyzed are categorical instead of quantitative. MCA is obtained by applying correspondence analysis to an indicator matrix.\

source: Abdi, H., & Valentin, D. (2007). Multiple correspondence analysis. Encyclopedia of measurement and statistics, 2(4), 651-657.\

MCA is applied in this chapter to a set of quantitative data, transformed into categorical by recoding each variable into bins (a range of scores goes into different bins; low, medium, or high scores).


## The Data Set
Data set to be analyzed by MCA is the same as in the PCA chapter. The first 6 rows represent panelists 1 - 6 grading of beer "Proposal 1."
There are 8 panelists grading 6 different beers according to the sensory attributes below. 2 of the beers were Non-alcoholic and 4 alcoholic. 5 of the beers were wheat based and one alcoholic beer was non-wheat based.


## Binning the Data
Here are the histograms of the first 4 sensory attributes. Based on their roughly normal distributions and the number of observations (48 for each sensory attribute), I decided to cut the data into 3 bins.
![](03MCA_files/figure-latex/hist-1.pdf)<!-- --> ![](03MCA_files/figure-latex/hist-2.pdf)<!-- --> ![](03MCA_files/figure-latex/hist-3.pdf)<!-- --> ![](03MCA_files/figure-latex/hist-4.pdf)<!-- --> 

A for loop was used to iterate through each of 42 quantitative variables and bin them accordingly:

```r
mesBeerRecoded <- data.frame(
               row.names = rownames(dm_beers))
irec = c(1:42)
for(val in irec)
{
  mesBeerRecoded[,colnames(dm_beers)[irec]] <- BinQuant(
             dm_beers[,irec], nClass = 3, stem = '')
}
```

## Phi Correlation

The heat map for the phi correlation is shown here. This was achieved by taking the square root of the phi squared. We see very similar correlations as compared to the PCA run in chapter 2.
![](03MCA_files/figure-latex/phi correlation-1.pdf)<!-- --> 

## MCA Code

Multiple Correspondence Analysis is run here using the function epMCA in package ExPosition version 2.8.23. The design was manually recoded later in this chapter.

```r
dm_beers_cat <- as.factor(df_beers[,2])
resMCA <- epMCA(DATA = mesBeerRecoded,
             graphs = FALSE )
#Inference battery is also run here:
resMCA.inf <- InPosition::epMCA.inference.battery(
                 DATA = mesBeerRecoded,
                 graphs =  FALSE)
```

```
## [1] "It is estimated that your iterations will take 0.08 minutes."
## [1] "R is not in interactive() mode. Resample-based tests will be conducted. Please take note of the progress bar."
## ================================================================================
```

## Plots

The following is the scree plot for the eigenvalues of the MCA. We see that component one explains roughly 72% of the variance in the data followed by roughly 17% and 2.5% for components 2 and 3 respectively.
![](03MCA_files/figure-latex/MCA Scree-1.pdf)<!-- --> 

The factor scores for the rows of the data are created here. Not much can be interpreted by the eye from these factor scores since no obvious clusters are exhibited. The yellow dots represent the regular Mx non-wheat beer product, the blue dots represent alcoholic beers, and red non-alcoholic beers.
![](03MCA_files/figure-latex/MCA row factor scores-1.pdf)<!-- --> ![](03MCA_files/figure-latex/MCA row factor scores-2.pdf)<!-- --> ![](03MCA_files/figure-latex/MCA row factor scores-3.pdf)<!-- --> ![](03MCA_files/figure-latex/MCA row factor scores-4.pdf)<!-- --> 

The factor scores for the columns of the data (sensory attributes) are created here. We see large contributions for alcoholic related sensory attributes on component 1 and wheat-beer related attributes on component 2.
![](03MCA_files/figure-latex/MCA column factor scores-1.pdf)<!-- --> 

The signed contribution plots are created here. The first contribution barplot is for component 1 and the second for component 2. Significant positive contributions on component 1 were shown in red, and negative shown in yellow.
![](03MCA_files/figure-latex/MCA Contribution and Bootstrap Bars-1.pdf)<!-- --> ![](03MCA_files/figure-latex/MCA Contribution and Bootstrap Bars-2.pdf)<!-- --> 

## Conclusion
Conclusion:
The variance of the data explained by component 1 is due to the differences between non alcoholic and alcoholic beers. Alcoholic sensory attributes showed strong negative contributions on component 1, and non-alcoholic sensory attributes (such as sweet,fruity, bready, and grainy)
The variance of the data explained by component 2 is due to the differences between wheat beers and non-wheat beers, with wheat beers showing positive contributions on component 2 and non-wheat beers negative.
