# Principal Component Analysis {#pca}



## Method: PCA

Principal component analysis (PCA) is used to analyze one table of quantitative data. PCA mixes the input variables to give new variables, called principal components. The first principal component is the line of best fit. It is the line that maximizes the inertia (similar to variance) of the cloud of data points. Subsequent components are defined as orthogonal to previous components, and maximize the remaining inertia. 

PCA gives one map for the rows (called factor scores), and one map for the columns (called loadings). These 2 maps are related, because they both are described by the same components. However, these 2 maps project different kinds of information onto the components, and so they are *interpreted differently*. Factor scores are the coordinates of the row observations. They are interpreted by the distances between them, and their distance from the origin. Loadings describe the column variables. Loadings are interpreted by the angle between them, and their distance from the origin. 

The distance from the origin is important in both maps, because squared distance from the mean is inertia (variance, information; see sum of squares as in ANOVA/regression). Because of the Pythagorean Theorem, the total information contributed by a data point (its squared distance to the origin) is also equal to the sum of its squared factor scores. 


## The Data Set
Eight sensory panelists evaluated six wheat beers using 42 sensory attributes			
Sensory attributes were evaluated using a 10-points intensity scale (imported from excel).	


```r
df_beers <- import("Wheat beer no alcohol.xlsx")
dm_beers <- data.matrix(df_beers[1:48,3:44])
head(dm_beers) %>% kable()
```


\begin{tabular}{r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r|r}
\hline
Aroma impact & Flavor impact & Sweet & Acid & Bitter & Astringent & Mettalic & Carbonated & Mouthfeel & Viscosity & Yeasty & Malty & Hop & Bready & Grainy & Cereal & Toasted & Burnt & Caramelic & Anisic & Guayacol & Musk & Rhum & Alcoholic & Brandy & Athyl acetate & Acetic acid & Earthy & Banana & Fruity & Ripe & Apple & Catty & Phenyl acetate & Cardboard & Oily & Buttery & Rancid & Fermented & C8 Acid & C10 Acid & Pheolic\\
\hline
6.8 & 7.0 & 0.2 & 0.4 & 5.3 & 4.8 & 0.2 & 1.2 & 2.1 & 2.4 & 1.5 & 3.1 & 6.1 & 0.3 & 0.3 & 1.1 & 0.7 & 1.0 & 0.9 & 1.3 & 3.3 & 2.3 & 2.9 & 6.0 & 0.1 & 1.2 & 0.0 & 0.4 & 0.7 & 1.2 & 1.0 & 1.1 & 1.4 & 2.0 & 0.3 & 0.9 & 0.4 & 0.5 & 3.7 & 0.8 & 0.7 & 1.7\\
\hline
1.0 & 1.0 & 2.0 & 2.1 & 0.9 & 1.0 & 1.0 & 0.5 & 0.9 & 1.0 & 0.5 & 0.9 & 1.6 & 1.4 & 1.0 & 1.0 & 0.4 & 0.0 & 0.6 & 0.0 & 0.0 & 1.0 & 0.0 & 1.0 & 0.0 & 0.6 & 0.0 & 0.0 & 0.0 & 0.0 & 1.0 & 0.0 & 0.0 & 2.1 & 0.9 & 0.0 & 0.0 & 0.0 & 1.0 & 1.9 & 2.1 & 1.8\\
\hline
6.0 & 9.0 & 7.0 & 8.0 & 8.0 & 6.0 & 1.0 & 1.0 & 8.0 & 3.0 & 6.1 & 1.0 & 7.0 & 4.0 & 8.0 & 4.0 & 1.0 & 6.0 & 6.0 & 0.0 & 2.0 & 0.0 & 3.0 & 7.0 & 0.0 & 0.0 & 3.0 & 1.0 & 0.0 & 0.0 & 0.0 & 0.0 & 3.0 & 0.0 & 0.0 & 4.0 & 3.0 & 0.0 & 6.1 & 0.0 & 0.0 & 1.0\\
\hline
7.1 & 8.1 & 4.0 & 3.0 & 5.7 & 5.0 & 7.5 & 6.5 & 8.6 & 4.0 & 5.5 & 2.0 & 3.0 & 4.5 & 3.0 & 4.5 & 3.5 & 2.0 & 4.0 & 3.0 & 1.0 & 3.7 & 3.5 & 5.0 & 6.1 & 3.6 & 3.4 & 5.6 & 6.5 & 5.0 & 5.6 & 6.3 & 9.4 & 3.7 & 4.5 & 5.7 & 6.6 & 6.2 & 6.5 & 5.0 & 8.6 & 2.0\\
\hline
4.5 & 5.0 & 4.1 & 4.2 & 5.6 & 5.4 & 5.7 & 5.5 & 4.6 & 4.2 & 5.0 & 6.1 & 4.9 & 5.4 & 4.1 & 5.2 & 5.7 & 5.7 & 3.4 & 3.7 & 3.9 & 3.9 & 3.9 & 5.2 & 5.5 & 5.1 & 5.1 & 6.0 & 3.5 & 4.9 & 4.3 & 4.8 & 6.3 & 4.8 & 5.6 & 5.5 & 5.1 & 5.0 & 4.1 & 3.8 & 3.9 & 4.4\\
\hline
7.0 & 8.4 & 3.0 & 5.0 & 7.5 & 6.0 & 1.5 & 7.0 & 4.6 & 3.0 & 5.0 & 5.4 & 7.5 & 3.5 & 5.0 & 2.0 & 2.0 & 1.0 & 7.1 & 0.5 & 5.5 & 1.0 & 6.0 & 9.1 & 6.5 & 7.5 & 1.5 & 0.3 & 1.0 & 6.0 & 4.0 & 2.1 & 2.0 & 7.0 & 7.0 & 1.0 & 1.0 & 1.0 & 9.0 & 8.1 & 8.1 & 2.1\\
\hline
\end{tabular}

```r
dm_beers <- t(dm_beers)
dm_beers <- scale(dm_beers, center = TRUE, scale = FALSE) 
#The data was centered based on columns instead of the rows.
dm_beers <- t(dm_beers)
```

## The correlation plot

Here is the correlation plot for the data. Most sensory attributes are negatively correlated with each other, but some are strongly correlated, such as flavor impact and aroma impact, bitter and astringent, sweet and bready/grainy, alcoholic and aroma impact/flavor impact, the acidic attributes with each other and fruity, etc. These positive correlations make sense and show that we've most likely performed the correlation analysis correctly.

```r
cor.res <- cor(dm_beers)
corrplot.mixed(cor.res, lower = 'shade', tl.pos = 'lt', tl.cex = 0.7, tl.col = "black")
```

![](01PCA_files/figure-latex/correlation plot-1.pdf)<!-- --> 

## Analysis

Set the seed so that your analysis is reproducible.

```r
set.seed(42)
```

Run PCA with inference.


```r
res_pcaInf <- epPCA.inference.battery(dm_beers, center = FALSE, scale = "SS1", graphs = FALSE,
                                      test.iters = 999)
```

### Testing the eigenvalues

Here we test the eigenvalues from the permutation test to see if they fall above the 97.5% cutoff. The results from the permutation test are shown in the histogram. We tested the eigenvalues for dimension one and dimension two and both passed the test and surpassed the 97.5% cutoff.

```r
zeDim = 1
pH1 <- prettyHist(
  distribution = res_pcaInf$Inference.Data$components$eigs.perm[,zeDim], 
           observed = res_pcaInf$Fixed.Data$ExPosition.Data$eigs[zeDim], 
           xlim = c(16.5, 24.5), # needs to be set by hand
           breaks = 20,
           border = "white", 
           main = paste0("Permutation Test for Eigenvalue ",zeDim),
           xlab = paste0("Eigenvalue ",zeDim), 
           ylab = "", 
           counts = FALSE, 
           cutoffs = c( 0.975))
```

![](01PCA_files/figure-latex/unnamed-chunk-2-1.pdf)<!-- --> 

```r
zeDim = 2
pH2 <- prettyHist(
  distribution = res_pcaInf$Inference.Data$components$eigs.perm[,zeDim], 
           observed = res_pcaInf$Fixed.Data$ExPosition.Data$eigs[zeDim], 
           xlim = c(2, 9.75), # needs to be set by hand
           breaks = 20,
           border = "white", 
           main = paste0("Permutation Test for Eigenvalue ",zeDim),
           xlab = paste0("Eigenvalue ",zeDim), 
           ylab = "", 
           counts = FALSE, 
           cutoffs = c(0.975))
```

![](01PCA_files/figure-latex/unnamed-chunk-2-2.pdf)<!-- --> 


### Scree Plot

The results from the permutation with Scree plot (significant components colored) are shown here. We see that the first four dimensions have significant p values. Dimension one explains nearly 40% of the variance in our data, and dimension two nearly 15%. We will focus on analyzing the first two dimensions in this analysis.

```r
my.scree.pca <- PlotScree(ev = res_pcaInf$Fixed.Data$ExPosition.Data$eigs,
                      p.ev = res_pcaInf$Inference.Data$components$p.vals)
```

![](01PCA_files/figure-latex/scree plot-1.pdf)<!-- --> 

### Factor scores

The factor scores on components one and two are plotted here. We see that the row factor scores do not exhibit any obvious clusters based on beer type since we centered the rows of the data to eliminate the effects of the rows (otherwise we would have two effects, one from the panelists and one from the beers). We will focus on the column factor scores for this PCA. Centering the columns can also be done in a different PCA in order to examine the effects of the rows, however, the variances will likely conflict from our two independent variables. The factor scores are colored according to their beer type. The blue beers are alcoholic, the red nonalcoholic, and the yellow the regular Mx product.

```r
col4Beers <- c(res_pcaInf$Fixed.Data$Plotting.Data$fi.col[1:16],recode(res_pcaInf$Fixed.Data$Plotting.Data$fi.col[17:32], "#305ABF" = "firebrick3"),res_pcaInf$Fixed.Data$Plotting.Data$fi.col[33:40],recode(res_pcaInf$Fixed.Data$Plotting.Data$fi.col[41:48], "#305ABF" = "#FFA500"))
my.fi.plot <- createFactorMap(res_pcaInf$Fixed.Data$ExPosition.Data$fi, # data
                            title = "Wheat Beer Row Factor Scores", # title of the plot
                            axis1 = 1, axis2 = 2, # which component for x and y axes
                            pch = 19, # the shape of the dots (google `pch`)
                            cex = 2, # the size of the dots
                            text.cex = 2.5, # the size of the text
                            alpha.points = 0.3,
                            col.points = col4Beers, # color of the dots
                            col.labels = col4Beers, display.labels = FALSE # color for labels of dots
                            )

fi.labels <- createxyLabels.gen(1,2,
                             lambda = res_pcaInf$Fixed.Data$ExPosition.Data$eigs,
                             tau = round(res_pcaInf$Fixed.Data$ExPosition.Data$t),
                             axisName = "Component "
                             )
fi.plot <- my.fi.plot$zeMap + fi.labels # you need this line to be able to save them in the end
fi.plot
```

![](01PCA_files/figure-latex/factor_scores-1.pdf)<!-- --> 

Here we get the color for each group:


```r
# get index for the first row of each group
beer_type <- df_beers[,2]
grp.ind <- order(beer_type)[!duplicated(sort(beer_type))]
grp.col <- col4Beers[grp.ind] # get the color
grp.name <- beer_type[grp.ind] # get the corresponding groups
names(grp.col) <- grp.name
```


#### With group means

Get the group means from the PCA.


```r
group.mean <- aggregate(res_pcaInf$Fixed.Data$ExPosition.Data$fi,
                     by = list(beer_type), # must be a list
                     mean)
group.mean

# need to format the results from `aggregate` correctly
rownames(group.mean) <- group.mean[,1] # Use the first column as row names
fi.mean <- group.mean[,-1] # Exclude the first column
fi.mean
```

Plot them!


```r
fi.mean.plot <- createFactorMap(fi.mean,
                                alpha.points = 0.8,
                                col.points = grp.col[rownames(fi.mean)],
                                col.labels = grp.col[rownames(fi.mean)],
                                pch = 17,
                                cex = 3,
                                text.cex = 3)
fi.WithMean <- my.fi.plot$zeMap_background + my.fi.plot$zeMap_dots + 
  fi.mean.plot$zeMap_dots + fi.mean.plot$zeMap_text + fi.labels
fi.WithMean
```

![](01PCA_files/figure-latex/fimeanplot-1.pdf)<!-- --> 

### Tolerance interval

We can plot the tolerance intervals for the beers, we see they are mostly overlapping due to the centering of the rows of the data.

```r
TIplot <- MakeToleranceIntervals(res_pcaInf$Fixed.Data$ExPosition.Data$fi,
                            design = as.factor(beer_type),
                            col = grp.col[rownames(fi.mean)],
                            line.size = .50, 
                            line.type = 3,
                            alpha.ellipse = .2,
                            alpha.line    = .4,
                            p.level       = .95)
# If you get some errors with this function, check the names.of.factors argument in the help.

fi.WithMeanTI <- my.fi.plot$zeMap_background + my.fi.plot$zeMap_dots + 
  fi.mean.plot$zeMap_dots + fi.mean.plot$zeMap_text + TIplot + fi.labels

fi.WithMeanTI
```

![](01PCA_files/figure-latex/unnamed-chunk-4-1.pdf)<!-- --> 


### Bootstrap interval

We can also add the bootstrap interval for the group means to see if these group means are significantly different. The bootstrap intervals are mostly overlapping in this case due to the centering of the data based on rows.

First step: bootstrap the group means

```r
# Depend on the size of your data, this might take a while
fi.boot <- Boot4Mean(res_pcaInf$Fixed.Data$ExPosition.Data$fi,
                     design = beer_type,
                     niter = 1000)
# Check what you have
fi.boot

# What is the cube? Check the first 4 tables. You don't need to include this in 
# your output, because it's a lot of junk text.
fi.boot$BootCube[,,1:4]
```

Second step: plot it!


```r
# Check other parameters you can change for this function
bootCI4mean <- MakeCIEllipses(fi.boot$BootCube[,c(1:2),], # get the first two components
                              col = grp.col[rownames(fi.mean)])

fi.WithMeanCI <- my.fi.plot$zeMap_background + bootCI4mean + 
  my.fi.plot$zeMap_dots + fi.mean.plot$zeMap_dots + 
  fi.mean.plot$zeMap_text + fi.labels

fi.WithMeanCI
```

![](01PCA_files/figure-latex/unnamed-chunk-6-1.pdf)<!-- --> 



### Loadings

Here we show the column factor scores, which is where most of our interpretation of the PCA will take place. Alcoholic, fermented, flavor impact, and aroma impact make strong negative contributions on component one, while Apple/Banana, Buttery/Oily, and Earthy/Musk make strong positive contributions on componenet one. These interpretations are repeated below the graph.

```r
cor.loading <- cor(dm_beers, res_pcaInf$Fixed.Data$ExPosition.Data$fi)
rownames(cor.loading) <- rownames(cor.loading)
col4points <- prettyGraphsColorSelection(n.colors = 42)
loading.plot <- createFactorMap(cor.loading,
                                constraints = list(minx = -1.0, miny = -1.0,
                                                   maxx = 1.0, maxy = 1.0),
                                col.points = col4points, alpha.points = 0, 
                                col.labels = col4points, text.cex = 3)
LoadingMapWithCircles <- loading.plot$zeMap + 
  addArrows(cor.loading, color = col4points, size = 0.5, alpha = 0.6) + 
  addCircleOfCor() + xlab("Component 1") + ylab("Component 2")

LoadingMapWithCircles
```


\includegraphics[width=1.5\linewidth]{01PCA_files/figure-latex/unnamed-chunk-7-1} 

You can also include the variance of each component and plot the factor scores for the columns (i.e., the variables):


```r
my.fj.plot <- createFactorMap(res_pcaInf$Fixed.Data$ExPosition.Data$fj, # data
                            title = "Beers Column Factor Scores", # title of the plot
                            axis1 = 1, axis2 = 2, # which component for x and y axes
                            pch = 19, # the shape of the dots (google `pch`)
                            cex = 3, # the size of the dots
                            text.cex = 3, # the size of the text
                            col.points = col4points, # color of the dots
                            col.labels = col4points, # color for labels of dots
                            )

fj.plot <- my.fj.plot$zeMap + fi.labels # you need this line to be able to save them in the end
fj.plot
```

![](01PCA_files/figure-latex/unnamed-chunk-8-1.pdf)<!-- --> 

* Component 1: Flavor/Aroma impact, Alcoholic, Fermented Vs. Apple/Banana, Buttery/Oily, Earthy/Musk

* Component 2: Burnt,Cereal,Acid.Toasted Vs. C10 acid, Brandy/Rhum, Athyl Acetate


#### Bootstrap Ratio of columns

##### Component 1

Here the boot strap ratio is applied to the contributions for both component 1 and 2. Our interpretations from the column factor scores are confirmed here by the significant bootstrap ratios for the sensory attributes mentioned. Findings are summarized in the next section of this chapter.


```r
signed.ctrJ <- res_pcaInf$Fixed.Data$ExPosition.Data$cj * sign(res_pcaInf$Fixed.Data$ExPosition.Data$fj)

# plot contributions for component 1
ctrJ.1 <- PrettyBarPlot2(signed.ctrJ[,1],
                         threshold = 1 / NROW(signed.ctrJ),
                         font.size = 3,
                         color4bar = gplots::col2hex(col4points), # we need hex code
                         ylab = 'Contributions',
                         ylim = c(1.2*min(signed.ctrJ[,1]), 1.2*max(signed.ctrJ[,1])),
                         horizontal = FALSE, signifOnly = TRUE
) + ggtitle("Contribution barplots", subtitle = 'Component 1: Variable Contributions (Signed)')

# plot contributions for component 2
ctrJ.2 <- PrettyBarPlot2(signed.ctrJ[,2],
                         threshold = 1 / NROW(signed.ctrJ),
                         font.size = 3,
                         color4bar = gplots::col2hex(col4points), # we need hex code
                         ylab = 'Contributions',
                         ylim = c(1.2*min(signed.ctrJ[,2]), 1.2*max(signed.ctrJ[,2])),
                         horizontal = FALSE, signifOnly = TRUE
) + ggtitle("",subtitle = 'Component 2: Variable Contributions (Signed)')


BR <- res_pcaInf$Inference.Data$fj.boots$tests$boot.ratios
laDim = 1

# Plot the bootstrap ratios for Dimension 1
ba001.BR1 <- PrettyBarPlot2(BR[,laDim],
                        threshold = 2,
                        font.size = 3,
                   color4bar = gplots::col2hex(col4points), # we need hex code
                  ylab = 'Bootstrap ratios',
                  horizontal = FALSE, signifOnly = TRUE
                  #ylim = c(1.2*min(BR[,laDim]), 1.2*max(BR[,laDim]))
) + ggtitle("Bootstrap ratios", subtitle = paste0('Component ', laDim))

# Plot the bootstrap ratios for Dimension 2
laDim = 2
ba002.BR2 <- PrettyBarPlot2(BR[,laDim],
                        threshold = 2,
                        font.size = 3,
                   color4bar = gplots::col2hex(col4points), # we need hex code
                  ylab = 'Bootstrap ratios',
                  horizontal = FALSE, signifOnly = TRUE
                  #ylim = c(1.2*min(BR[,laDim]), 1.2*max(BR[,laDim]))
) + ggtitle("",subtitle = paste0('Component ', laDim))
```

We then use the next line of code to put two figures side to side:


```r
  grid.arrange(
    as.grob(ctrJ.1),
    as.grob(ctrJ.2),
    as.grob(ba001.BR1),
    as.grob(ba002.BR2),
    ncol = 2,nrow = 2,
    top = textGrob("Barplots for variables", gp = gpar(fontsize = 18, font = 3))
  )
```

![](01PCA_files/figure-latex/grid_ctrJ-1.pdf)<!-- --> 


## Summary  
When we interpret the factor scores and loadings together, the PCA revealed:

* Component 1: Alcoholic Beers were astringent, bitter, high flavor and aroma impact and Non-Alcoholic Beers were fruity, buttery, oily, earthy, and had high musk

* Component 2: Wheat Beers were fruity, pheolic, high in c10 acid, Rhum, and Brandy content and Non-Wheat Beers had burnt, cereal, high viscosity and high acid attributes.

* Both: Non-alcoholic wheat beers are particularly fruity.
