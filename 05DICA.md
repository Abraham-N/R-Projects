# Discriminant Correspondence Analysis {#DICA}



## Introduction
Introduction: Discriminant Correspondence Analysis (DiCA) is an extension of Discriminant Analysis (DA) and Correspondence Analysis (CA). In DiCA, observations are categorized into groups (as in DA). With DiCA, each group is represented by the sum of it's observations and a CA is performed on the data matrix. Afterward the original observations are assigned to the closest group after projecting them as supplementary elements. This chapter will go through and example where DiCA is applied to the "Wheat Beer Sensory Profile" data set, the same data frame used in the previous chapters.


References: 
Abdi, H. (2007). Discriminant correspondence analysis. Encyclopedia of measurement and statistics, 2007,     1-10.

## Data and Binning
Here is the importing and binning of the data. We are using the same data set: the alcoholic vs. non-alcoholic sensory profile.

```r
df_beers <- import("Wheat beer no alcohol.xlsx")
dm_beers <- data.matrix(df_beers[1:48,3:44])
head(dm_beers)
XYmat <- data.frame(
               row.names = rownames(dm_beers))
irec = c(1:42)
for(val in irec)
{
  XYmat[,colnames(dm_beers)[irec]] <- BinQuant(
             dm_beers[,irec], nClass = 3, stem = '')
}
XYmat <- as.matrix(XYmat)
XYmat <- apply(XYmat,2,as.numeric)
```

## Heat Map

Here is the heatmap for the discriminant correspondence analysis. We see that most strongly positive correlations occur with the later sensory attributes with each other.
![](05DICA_files/figure-latex/DICA heatmap-1.pdf)<!-- --> 

## DiCA Code and Scree
Here I've applied the DiCA and inference battery functions to the binned data. The Scree plot is also shown here. We see the first dimension explains roughly 30% of the data, while the second dimension explains roughly 25% of the data. We will focus on the first two dimensions in this analysis.

```
## [1] "It is estimated that your iterations will take 0.15 minutes."
## [1] "R is not in interactive() mode. Resample-based tests will be conducted. Please take note of the progress bar."
## ================================================================================
```

![](05DICA_files/figure-latex/DICA scree_and_inference-1.pdf)<!-- --> 

## Plots


### Row factor scores

Here is the code to plot the row factor scores with group means. The separation of row factor scores between beer type was quite apparent, as a separation of non alcoholic beers was witnessed from alcoholic beers on component 1. Component 2 is most likely related to the effects of the panelists grading the beers (for the rows).
![](05DICA_files/figure-latex/DICA row_factor_scores with means-1.pdf)<!-- --> 

Here I've ploted the row factor scores with confidence intervals, we see some overlap of the confidence intervals but there is still a clear seperation between alcoholic and non-alcoholic beers. 
![](05DICA_files/figure-latex/DICA row_factor_scores with CI-1.pdf)<!-- --> 

Here is I've plotted the same row factor scores with tolerance interval hulls: 
![](05DICA_files/figure-latex/row_factor_scores with Hull-1.pdf)<!-- --> 


### Col factor scores
Here is the code producing the column factor scores. We see that component one shows the difference brandy/rhum/fruity vs. flavor impact/aroma impact/oily sensory attributes. Component two shows the differences between mettalic/pheolic vs. buttery/malty/acetic acid sensory attributes.
![](05DICA_files/figure-latex/col_factor_scores-1.pdf)<!-- --> 


### Contribution Plots

Here is the code producing the Contribution Plots. Alcoholic sensory attributes were important on component 1 and component 2 was some wheat related sensory attributes, meaning the panelists may have interpreted these sensory attributes differently. 
![](05DICA_files/figure-latex/DICA contribution plots-1.pdf)<!-- --> ![](05DICA_files/figure-latex/DICA contribution plots-2.pdf)<!-- --> 

### Bootstrap plots

The following generates the bootstrap ratio barplots. Although the contribution plots showed many important contributions, The bootstrap ratios show that most of these contributions did not reach threshold significance levels.
![](05DICA_files/figure-latex/DICA bootstrap ratio plots-1.pdf)<!-- --> ![](05DICA_files/figure-latex/DICA bootstrap ratio plots-2.pdf)<!-- --> 

## Confusion Matrices

Here are the fixed and random effect confusion matrices, along with the prediction accuracies. Similar to BADA, we see a steep drop in prediction accuracy between fixed and random data, however, the fixed accuracy in this case is larger than BADA, therefore DiCA was more accurate in predicting the classifications of our observations than.

\begin{tabular}{l|r|r|r|r|r|r}
\hline
  & .Proposal 1 & .Proposal 2 & .NA 1 & .NA 2 & .Blue moon & .Corona\\
\hline
.Proposal 1 & 5 & 1 & 0 & 0 & 0 & 1\\
\hline
.Proposal 2 & 0 & 5 & 0 & 1 & 0 & 0\\
\hline
.NA 1 & 1 & 0 & 6 & 1 & 1 & 0\\
\hline
.NA 2 & 1 & 1 & 1 & 6 & 1 & 1\\
\hline
.Blue moon & 1 & 1 & 0 & 0 & 6 & 2\\
\hline
.Corona & 0 & 0 & 1 & 0 & 0 & 4\\
\hline
\end{tabular}

```
## [1] 0.6666667
```


\begin{tabular}{l|r|r|r|r|r|r}
\hline
  & .Proposal 1.actual & .Proposal 2.actual & .NA 1.actual & .NA 2.actual & .Blue moon.actual & .Corona.actual\\
\hline
.Proposal 1.predicted & 0 & 3 & 2 & 2 & 1 & 1\\
\hline
.Proposal 2.predicted & 3 & 0 & 1 & 1 & 1 & 0\\
\hline
.NA 1.predicted & 1 & 1 & 1 & 2 & 2 & 0\\
\hline
.NA 2.predicted & 1 & 1 & 2 & 1 & 1 & 2\\
\hline
.Blue moon.predicted & 2 & 3 & 0 & 0 & 1 & 4\\
\hline
.Corona.predicted & 1 & 0 & 2 & 2 & 2 & 1\\
\hline
\end{tabular}

```
## [1] 0.08333333
```

## Conclusion
The separation of row factor scores between beer type was quite apparent, as a separation of non alcoholic beers was witnessed from alcoholic beers on component 1. Component 2 is most likely related to the effects of the panelists grading the beers (for the rows). The contribution plots also tell the same story, as alcoholic sensory attributes were important on component 1 and component 2 was some wheat related sensory attributes, meaning the panelists may have interpreted these sensory attributes differently. 
