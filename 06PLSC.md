# Partial Least Squares Correlation {#PLSC}



## Introduction
Partial Least Squares Correlation strives to relate the measurements of two data tables taken on the same set of observations and find the shared information in the tables. To do this, maximal covariance is found between two new variables (one for each table) called latent variables, which are derived as linear combinations of the original observations, which are found by projecting the original matrices to their saliencies.

![](PLSCIntro.png)

## Data and Correlation matrix

In this analysis, Xmat consists of the first half of sensory attributes and Ymat consists of the last half of sensory attributes. Here is shown the correlation matrix and data import. The correlation plots shows mostly positive or zero correlation values between the sensory attributes of Xmat and Ymat.


```r
#data set:
df_beers <- import("Wheat beer no alcohol.xlsx")
dm_beers <- data.matrix(df_beers[1:48,3:44])
Xmat <- dm_beers[,1:21]
Ymat <- dm_beers[,22:42]
```

![](06PLSC_files/figure-latex/unnamed-chunk-2-1.pdf)<!-- --> 

## Scree Plot

The PLSC function call and scree plot (after 1000 iterations permutation testing) are shown. The Scree plot shows how much variance is explained by each dimension. The singular values scree plot is computed by taking the square root of the eigenvalues, and therefore is not additive. The inertia scree plot shows that dimension one explained roughly 95% of the variance, and is the only dimension that falls above the Kaiser Line.

![](06PLSC_files/figure-latex/PLSC scree_with_perm_testing-1.pdf)<!-- --> ![](06PLSC_files/figure-latex/PLSC scree_with_perm_testing-2.pdf)<!-- --> 

## Dimension One

The following analyzes dimension one of the PLSC. The latent variable scores are plotted for dimension one and dimension two, and the confidence intervals for each group (Alcoholic, Non-Alcoholic (NA), and Corona) are plotted on top.

### Latent Variables

The latent variable plot for dimension one is shown here. We see a very slight seperation of alcoholic beers from nonalcoholic beers, with alcoholic beers displaying negative scores on component one.
![](06PLSC_files/figure-latex/PLSC latent_variable_mats-1.pdf)<!-- --> ![](06PLSC_files/figure-latex/PLSC latent_variable_mats-2.pdf)<!-- --> 



### Contribution Plots

For dimension one, all contributions from the sensory attributes were negative, meaning no important differences between the attributes were seen on this dimension.
![](06PLSC_files/figure-latex/PLSC contribution_plots-1.pdf)<!-- --> ![](06PLSC_files/figure-latex/PLSC contribution_plots-2.pdf)<!-- --> 

### Bootstrap Plots

The bootstrap ratio significance level was applied to the contributions, and we see the same findings. Many of the contributions were found significant.
![](06PLSC_files/figure-latex/PLSC bootstrap_plots-1.pdf)<!-- --> ![](06PLSC_files/figure-latex/PLSC bootstrap_plots-2.pdf)<!-- --> 

## Dimension Two

The following analyzes dimension two of the PLSC.

### Latent Variables 2

For the latent variables scores on the second dimension, we don't see any differences of the row means according to beer type, therefore no inferences can be made.  
![](06PLSC_files/figure-latex/PLSC latent_variable_mats2-1.pdf)<!-- --> ![](06PLSC_files/figure-latex/PLSC latent_variable_mats2-2.pdf)<!-- --> 

### Contribution Plots 2

The contribution plot I set shows the differences between mettalic/viscosity/cereal vs. flavor impact/bitter/hop/carmelic sensory attributes. For the J set we see the differences between musky/earthy/buttery/rancid vs. fermented/alcoholic. The J set is most likely attributed to the difference between alcoholic and nonalcoholic beers.
![](06PLSC_files/figure-latex/PLSC contribution_plots2-1.pdf)<!-- --> ![](06PLSC_files/figure-latex/PLSC contribution_plots2-2.pdf)<!-- --> 

### Bootstrap Plots 2

The bootstrapped significance is applied to the contributions, and we see similar results as with the contribution plots.
![](06PLSC_files/figure-latex/PLSC bootstrap_plots2-1.pdf)<!-- --> ![](06PLSC_files/figure-latex/PLSC bootstrap_plots2-2.pdf)<!-- --> 

## Conclusion

Dimension 1:\
Latent Variables: No reliable differences between beer types.\
I-plots: All attributes except acid and burnt significant.\
J-plots: All attributes significant.\
Dimension 2:\
Latent Variables: No obvious differences between beer types.\
I-plots: Flavor-impact, bitter, hop vs. Metallic, mouthfeel, viscosity, cereal.\
J-plots: Fermented vs. Musk, Earthy, Buttery, Rancid\

