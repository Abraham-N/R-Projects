# DiSTATIS {#DiSTATIS}



## Introduction

DiSTATIS is a generalization of multidimensional scaling (MDS); it is used to analyze a set of distance matrices instead of a single distance matrix. Distance matrices are compated by creating a common structure called a compromise, which is a combination of the matrices. The original distance matrices are then projected on the compromise.

Reference: Abdi, H., Valentin, D., Chollet, S., & Chrea, C. (2007). Analyzing assessors and products in sorting tasks: DISTATIS, theory and applications. Food quality and preference, 18(4), 627-640.

## Data Set & Function Call

Here we import the data set and compute the necessary variables for the DiSTATIS analysis. The data is the groupings of 19 judges on 18 (6 red, 6 white, and 6 rose wines) different wines. Groupings scaled from 1 to 9 in this data set. The purpose of the lines of code are commented for your reference.

```r
#import the data frame and pull quantitative data to data matrix
df_wines <- import("WinesAndColors.xlsx")
dm_wines <- data.matrix(df_wines[,4:22])
multiSort <- dm_wines
#Compute the distance matrix
DistanceCube <- DistanceFromSort(multiSort)
# **** Computations ----
## runDistatis--------------------------------
resDistatis <- distatis(DistanceCube, 
                        nfact2keep = 10)
n.active <- dim(DistanceCube)[3]
```



## Heat Maps

The Rv and S heat maps from the DiSTATIS are shown here. We use these heat maps to get an overall idea of how the variables in our data relate. For the Rv map, we see only positive correlations with a mix of slightly stronger positive correlations. Judge 5 across the board had low correlation values as compared to the other judges. For the S map, the wines of the same type correlated with each other, but not with other wines. There was some correlation between rose and white maps, however.
![](07DiSTATIS_files/figure-latex/Heat Maps-1.pdf)<!-- --> ![](07DiSTATIS_files/figure-latex/Heat Maps-2.pdf)<!-- --> 

## Scree Plots

The Scree plots of the Rv and S maps are shown below. For the Rv map, we see that dimension one explains 34% of the variance in the data while dimension two explains 8%. For the S map, dimension one explains 24% of the the variance while dimension two explains 9%.
![](07DiSTATIS_files/figure-latex/Scree Plots-1.pdf)<!-- --> ![](07DiSTATIS_files/figure-latex/Scree Plots-2.pdf)<!-- --> 

## Factor Scores

The RV and S factor score maps are shown below.  We see a strong positive contribution on dimension two on the Rv map from Judges 7, 8, and 12, and strong negative contribution from judges 14, 13, and 10. The judges mostly agreed along dimension one. For the S map, dimension one explains the variance between red wines vs rose and white wines, while dimension two there was no clear seperation of wines based on type and the variance here is likely due to the differences in judges grouping the wines. 
![](07DiSTATIS_files/figure-latex/Factor Scores-1.pdf)<!-- --> 

```
## [1] Bootstrap On Factor Scores. Iterations #: 
## [2] 1000
```

![](07DiSTATIS_files/figure-latex/Factor Scores-2.pdf)<!-- --> 

## Partial Factor Scores

The Global factor scores along with the partial factor scores are computed below. We see that the judges seem to be explaining an equal amount of variance, and the judges are dispersed relatively evenly across the map.
![](07DiSTATIS_files/figure-latex/Partial Factor Scores-1.pdf)<!-- --> ![](07DiSTATIS_files/figure-latex/Partial Factor Scores-2.pdf)<!-- --> 

## Conclusion
The variance dimension one explains is attributed to the difference between red vs. white and rose wines. The judges mostly agreed along dimension one. The second dimension is most likely attributed to the differences in judges 13, 14, and 10 vs. 8, 7, and 12, as there was not any observable separation between white and rose wines along dimension two. 
