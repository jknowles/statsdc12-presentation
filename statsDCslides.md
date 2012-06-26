% Using R and Longitudinal Data System Records to Answer Policy Questions
% Jared Knowles


# Overview
- Why R?
- Examples of R Analyses?
- Share R code across states
- Develop joint methods
- Produce reports



# Why R?
- R is free
- R is open source
- R is **best in class** and state of the art
- R is free

# R
<p align="center"><img src="http://dl.dropbox.com/u/1811289/workspacescreen.png" height="700" width="800"></p>

# Google Scholar Hits
R has recently passed Stata on Google Scholar hits and it is catching up to the two major players SPSS and SAS

<p align="center"><img src="http://dl.dropbox.com/u/1811289/googlescholar.png" height="600" width="800"></p>

# R Has an Active Web Presence
R is linked to from more and more sites 

<p align="center"><img src="http://dl.dropbox.com/u/1811289/sitelinks.png" height="650" width="800"></p>

# R Extensions
These links come from the explosion of add-on packages to R

<p align="center"><img src="http://dl.dropbox.com/u/1811289/addons.png" height="600" width="800"></p>

# R Has an Active Community 
Usage of the R listserv for help has really exploded recently

<p align="center"><img src="http://dl.dropbox.com/u/1811289/rlistserv.png" height="600" width="800"></p>


# R Examples
Read in Data



```r
studat <- read.csv("data/smalldata.csv")
str(studat[5:18, ])
```



```
## 'data.frame':	14 obs. of  32 variables:
##  $ X          : int  274 276 478 574 613 620 717 772 1004 1056 ...
##  $ school     : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ stuid      : int  142705 14995 120205 103495 55705 28495 37705 52705 41995 10705 ...
##  $ grade      : int  3 3 3 3 3 3 3 3 3 3 ...
##  $ schid      : int  205 495 205 495 205 495 205 205 495 205 ...
##  $ dist       : int  75 105 15 45 75 45 75 75 105 75 ...
##  $ white      : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ black      : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ hisp       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ indian     : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ asian      : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ econ       : int  1 1 1 1 1 0 1 0 1 1 ...
##  $ female     : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ ell        : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ disab      : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ sch_fay    : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ dist_fay   : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ luck       : int  0 0 1 0 0 0 0 1 0 0 ...
##  $ ability    : num  81.9 101.9 87.3 96.6 98.4 ...
##  $ measerr    : num  52.98 22.6 4.67 -9.35 -7.7 ...
##  $ teachq     : num  56.68 71.62 66.88 75.21 4.95 ...
##  $ year       : int  2000 2000 2000 2000 2000 2000 2000 2000 2000 2000 ...
##  $ attday     : int  156 157 169 180 170 152 162 180 152 165 ...
##  $ schoolscore: num  56 56 56 56 56 ...
##  $ district   : int  3 3 3 3 3 3 3 3 3 3 ...
##  $ schoolhigh : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ schoolavg  : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ schoollow  : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ readSS     : num  373 437 418 454 310 ...
##  $ mathSS     : num  441 463 436 434 284 ...
##  $ proflvl    : Factor w/ 4 levels "advanced","basic",..: 2 4 4 4 3 2 4 4 2 3 ...
##  $ race       : Factor w/ 5 levels "A","B","H","I",..: 2 2 2 2 2 2 2 2 2 2 ...
```



```r
source("data/simulate_data.R")
```




# Simple Diagnostics



```r
source("ggplot2themes.R")
library(ggplot2)
qplot(readSS, mathSS, data = studat, alpha = I(0.2)) + geom_smooth(aes(group = ell, 
    color = factor(ell))) + theme_dpi()
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-1.svg) 


# Advanced Diagnostics



```r
samp <- sample(studat$stuid, 24)
plotsub <- subset(studat, stuid %in% samp)
qplot(grade, readSS, data = plotsub) + facet_wrap(~stuid, nrow = 4, 
    ncol = 6) + theme_dpi() + geom_line() + geom_smooth(method = "lm", se = FALSE)
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2.svg) 


# More Advanced 
<p align="center"><img src="http://dl.dropbox.com/u/1811289/TESTstuplot.gif" height="800" width="900"></p>

# What we do?
- We fit statistical models to all the students in Wisconsin modeling their future test score as best as possible through each strategy described above
- We also include an indicator of whether the student was eligible for BLBC instruction or not
- We compare to see if the average student receiving BLBC did better than the average student receiving other ESL services, all else equal
- We report the range of uncertainty around this difference between student groups and compare them to see if any meaningful differences emerge
- Due to our large sample size we expect our results to be biased in favor of finding statistically significant results, so we also examine the magnitude of findings to see if they are **substantively significant**; i.e. is the difference big enough to matter in the lives of students?


# Evaluations of Policy 
- Results are presented in effect sizes, or standard deviation units of change in test scores.
- **0.1** is small, 0.2 to 0.4 is reasonable and is about a *year* of education in most cases. Bigger than 0.4 is huge.
![plot of chunk modelspec](figure/modelspec.svg) 

- The bars represent the 95% confidence interval around the estimate. The VAM model is consistently statistically significant, not overlapping 0, and negative.
- The length of the bars represent the uncertainty about the estimate.
- But the mean effect size is quite small, less than 0.1 standard deviations in most cases. 
- This represents a year-to-year change in a student's score between BLBC and non-BLBC instruction.

# Results
- Language is different. Wisconsin has a large sample of both Hmong and Spanish speakers and they have different results when analyzed separately
![plot of chunk modelbracketlang](figure/modelbracketlang.svg) 

- BLBC has no effect for Spanish speakers on math, but a large negative effect for Hmong speakers
- BLBC may be slightly negative for reading among SPanish speakers, but not for Hmong speakers
- BLBC may be slightly positive for English proficiency for Hmong speakers and not for Spanish speakers
- More precision is needed

# Conclusions and Next Steps
**Conclusions**

- In Wisconsin BLBC does not have the **negative** effects found in recent research on California, though a slight negative effect may exist in some cases
- Unlike other states where BLBC has been studied, Wisconsin has substantially different results between language groups on mathematics and possibly on English proficiency achievement
- There is still a lot of imprecision in the estimates used here and more precision would be helpful, but effects are not substantively large in terms of relative student performance, even in the upper and lower bounds

**Next Steps**

- Get more data over more years and use a more precise estimation technique to reduce uncertainty about effects
- Explore the *variation* across BLBC programs in addition to the mean effect
- Learn more about the non-cognitive non-academic outcomes for BLBC in order to understand the costs and benefits of BLBC programs more fully
- Estimate a "treatement-on-the-treated"" parameter to more directly compare to prior research
- Survey teachers and merge teacher/program practice data with student outcomes to begin exploring the effective components of BLBC and ESL programs


# Inference Trees
Outline of the conditional inference tree structure. 


```r
library(partykit)
mypar <- ctree_control(testtype = "Bonferroni", mincriterion = 0.99)
mytree <- ctree(mathSS ~ race + econ + ell + disab + sch_fay + dist_fay + 
    attday + readSS, data = subset(studat, grade == 3))
plot(mytree)
```

![plot of chunk parttree](figure/parttree.svg) 





