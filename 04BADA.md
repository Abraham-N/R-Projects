# Barycentric Discriminant Analysis {#BADA}



## Introduction
Barycentric Discriminant Analysis (BADA) generalizes discriminant analysis (DA). As in DA, BADA combines measurements of observations and assigns them to categories. BADA can be used when DA cannot, like when there are more variables than observations or when the measurements are categorical. BADA relies on the principle that each each category is represented by the barycenter (weighted average) of its observations.

References: 
Abdi, H., & Williams, L. J. (2010). Barycentric discriminant analysis (BADIA). Encyclopedia of research    design, 64-75.

The data set to be analyzed is the same as in previous chapters: The alcoholic vs non-alcoholic wheat beer sensory profile.


## Heatmap
Here is the heatmap of averages for the BADA. We see mostly positive correlations accross the board, however, we see acetic acid and mettalic are mostly negatively correlated with the other sensory attributes.
![](04BADA_files/figure-latex/BADA heatmap_of_averages-1.pdf)<!-- --> 

## BADA Code and Scree

Here is the code used to run the BADA, as well as the scree plot. We see that dimension one here explains roughly 40% of the variance in our data and dimension two explains roughly 20%. We will focus on the first two dimensions in this analysis.

```r
# Computations ----
# Run BADA  ----
resBADA <- tepBADA(XYmat, DESIGN = df_beers$Product,
                   graphs = FALSE)
# Inferences ----
#set.seed(70301) # we had a problem 
# with the inference part
# it is addressed iin the Fix from Luke's github
nIter = 1000
resBADA.inf <- tepBADA.inference.battery(XYmat, 
                  DESIGN = df_beers$Product,
                  test.iters = nIter,
                  graphs = FALSE)
```

```
## [1] "It is estimated that your iterations will take 0.56 minutes."
## [1] "R is not in interactive() mode. Resample-based tests will be conducted. Please take note of the progress bar."
## ================================================================================
```

![](04BADA_files/figure-latex/unnamed-chunk-2-1.pdf)<!-- --> 

## Plots

### Row Factor Scores
Here are the row factor scores with observations and group means. We see blue moon and proposal two beer means (both of which are alcoholic) have negative contributions, while the rest of the beer means are positive. However, there is not a clear separation of beer type in this case.
![](04BADA_files/figure-latex/BADA row_factor_scores_group_means-1.pdf)<!-- --> 

Here are the row factor scores with confidence intervals:
![](04BADA_files/figure-latex/BADA row_factor_scores_CI-1.pdf)<!-- --> 

Here are the row factor scores with tolerance intervals:
![](04BADA_files/figure-latex/BADA row_factor_scores_TI-1.pdf)<!-- --> 

### Col Factor Scores

Here are the column factor scores. For component one, we see a distinction between Fruity/Hop vs. Acetic Acid/Mettalic/Astringent. For component two, we see the differences between carbonated, acid, and earthy vs. phenyl acetate/oily/cardboard.
![](04BADA_files/figure-latex/BADA col factor scores-1.pdf)<!-- --> 

### Contribution Plots

Here are the contribution plots for the BADA. alcoholic sensory attributes were important on component 1 and component 2 was some wheat related sensory attributes, meaning the panelists may have interpreted these attributes differently. 

![](04BADA_files/figure-latex/BADA contribution_plots-1.pdf)<!-- --> ![](04BADA_files/figure-latex/BADA contribution_plots-2.pdf)<!-- --> 

### Bootstrap plots
Here are the bootstrap ratio barplots for the variables. Although the contribution plots showed many important contributions, The bootstrap ratios show that most of these contributions did not reach threshold significance levels.
![](04BADA_files/figure-latex/BADA bootstrap_plots-1.pdf)<!-- --> ![](04BADA_files/figure-latex/BADA bootstrap_plots-2.pdf)<!-- --> 

## Confusion Matrices

Here are the fixed and random effect confusion matrices, along with the prediction accuracy. There was a large drop in the fixed accuracy vs. the random accuracy, however, the fixed accuracy is still quite low at 42% meaning BADA did a poor job of classifying our observations.

```r
#fixed confusion matrix
fixedCM <- resBADA.inf$Inference.Data$loo.data$fixed.confuse
head(fixedCM) %>% kable()
```


\begin{tabular}{l|r|r|r|r|r|r}
\hline
  & .Proposal 1 & .Proposal 2 & .NA 1 & .NA 2 & .Blue moon & .Corona\\
\hline
.Proposal 1 & 4 & 3 & 0 & 1 & 3 & 1\\
\hline
.Proposal 2 & 1 & 4 & 2 & 3 & 0 & 1\\
\hline
.NA 1 & 1 & 0 & 2 & 1 & 1 & 1\\
\hline
.NA 2 & 1 & 0 & 2 & 2 & 0 & 0\\
\hline
.Blue moon & 0 & 0 & 1 & 0 & 3 & 0\\
\hline
.Corona & 1 & 1 & 1 & 1 & 1 & 5\\
\hline
\end{tabular}

```r
#fixed accuracy
resBADA.inf$Inference.Data$loo.data$fixed.acc
```

```
## [1] 0.4166667
```

```r
#random confusion matrix
randomCM <- resBADA.inf$Inference.Data$loo.data$loo.confuse
head(randomCM) %>% kable()
```


\begin{tabular}{l|r|r|r|r|r|r}
\hline
  & .Proposal 1.actual & .Proposal 2.actual & .NA 1.actual & .NA 2.actual & .Blue moon.actual & .Corona.actual\\
\hline
.Proposal 1.predicted & 0 & 4 & 1 & 3 & 3 & 3\\
\hline
.Proposal 2.predicted & 1 & 1 & 2 & 2 & 2 & 2\\
\hline
.NA 1.predicted & 1 & 0 & 0 & 1 & 1 & 2\\
\hline
.NA 2.predicted & 4 & 0 & 3 & 1 & 0 & 1\\
\hline
.Blue moon.predicted & 0 & 2 & 1 & 0 & 0 & 0\\
\hline
.Corona.predicted & 2 & 1 & 1 & 1 & 2 & 0\\
\hline
\end{tabular}

```r
#random accuracy
resBADA.inf$Inference.Data$loo.data$loo.acc
```

```
## [1] 0.04166667
```
## Conclusion
Conclusion: There was some separation of row factor scores between beer type, as a separation of non\ alcoholic beers was witnessed from alcoholic beers on component 1, and alcohol related contributions were\ important on component 1. Component 2 is most likely related to the effects of the panelists\ grading the beers (for the rows). The contribution plots also tell the same story, as alcoholic sensory\ attributes were important on component 1 and component 2 was some wheat related sensory attributes,\ meaning the panelists may have interpreted these attributes differently. The bootstrap ratios, however,\ show that these contributions did not reach threshold significance levels.\
