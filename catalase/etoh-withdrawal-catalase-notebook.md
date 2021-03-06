Caio Maximino[1]

This is an [R Markdown](http://rmarkdown.rstudio.com) Notebook for the data analysis of the research project "Behavioral and biochemical effects of ethanol withdrawal in zebrafish".

Data is produced by members from Laboratório de Neurociências e Comportamento "Frederico Guilherme Graeff", affiliated to Universidade Federal do Sul e Sudeste do Pará and Universidade do Estado do Pará. The package will include primary data for a behavioral experiment on the effects of ethanol withdrawal on zebrafish anxiety-like behavior, as well as data and scripts for a meta-analysis of behavioral data on zebrafish.

When you execute code within the notebook, the results appear beneath the code.

Biochemical data from the in vitro catalase activity experiment, on brains and head kidneys dissected from animals exposed to the withdrawal protocol. Datasets represent average catalase activities, determined following the Aebi (1984) method, and calculated as a constant *k* = (1/*Δ**t*)(*l**n**A*1/*A*2), where *Δ**t* refers to the time interval used in the experiments (i.e., 30 s between measurements M1 and M2), and A1 and A2 refer to absorbances for measurements M1 and M2. Datasets include analytic validation assays, as well as experimental data on brains and head kidneys.

[![DOI](https://zenodo.org/badge/95811139.svg)](https://zenodo.org/badge/latestdoi/95811139)

Load needed libraries:

``` r
if(!require(ggplot2)){
    install.packages("ggplot2")
    library(ggplot2)
}
```

    ## Loading required package: ggplot2

``` r
if(!require(coin)){
    install.packages("coin")
    library(coin)
}
```

    ## Loading required package: coin

    ## Loading required package: survival

``` r
if(!require(RCurl)){
    install.packages("RCurl")
    library(RCurl)
}
```

    ## Loading required package: RCurl

    ## Loading required package: bitops

Load data from github and inspect it:

``` r
x <- getURL("https://raw.githubusercontent.com/lanec-unifesspa/etoh-withdrawal/master/catalase/teste.csv")
catalase <- read.csv(text = x)
catalase$Group <- as.factor(catalase$Group)
View(catalase)
```

Approximative Two-Sample Fisher-Pitman Permutation Test of the brain data

``` r
oneway_test(Brain ~ Group, data = catalase, distribution="approximate"(B=10000))
```

    ## 
    ##  Approximative Two-Sample Fisher-Pitman Permutation Test
    ## 
    ## data:  Brain by Group (CTRL, WD)
    ## Z = -1.9972, p-value = 0.0419
    ## alternative hypothesis: true mu is not equal to 0

Approximative Two-Sample Fisher-Pitman Permutation Test of the head kidney data

``` r
oneway_test(Head_kidney ~ Group, data = catalase, distribution="approximate"(B=10000))
```

    ## 
    ##  Approximative Two-Sample Fisher-Pitman Permutation Test
    ## 
    ## data:  Head_kidney by Group (CTRL, WD)
    ## Z = -0.46969, p-value = 0.6448
    ## alternative hypothesis: true mu is not equal to 0

Produce figure on ggplot2 (Brain data)

``` r
ggplot(catalase, aes(x=Group, y = Brain)) + geom_dotplot(binaxis='y', stackdir='center', alpha=0.5, dotsize = 0.75) + stat_summary(fun.data=mean_cl_boot, geom="pointrange", color="red") + labs(x = "Group", y = "CAT activity (U per mg protein)")
```

    ## `stat_bindot()` using `bins = 30`. Pick better value with `binwidth`.

![](etoh-withdrawal-catalase-notebook_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-5-1.png)

Produce figure on ggplot2 (Head kidney data)

``` r
ggplot(catalase, aes(x=Group, y = Head_kidney)) + geom_dotplot(binaxis='y', stackdir='center', alpha=0.5, dotsize = 0.75) + stat_summary(fun.data=mean_cl_boot, geom="pointrange", color="red") + labs(x = "Group", y = "CAT activity (U per mg protein)")
```

    ## `stat_bindot()` using `bins = 30`. Pick better value with `binwidth`.

![](etoh-withdrawal-catalase-notebook_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-6-1.png)

[1] Universidade Federal do Sul e Sudeste do Pará

[2] Universidade do Estado do Pará

[3] Universidade Federal do Sul e Sudeste do Pará
