# Assignment: Statistical Inference Course Project 2 Part 2
John Allen  
Thursday, January 28, 2016  

This project will analyze the reported effects of Vitamin C on tooth growth in Guinea pigs.

####Load libraries

```r
library(datasets)
library(ggplot2)
```

####Initial observations of ToothGrowth dataset

```r
str(ToothGrowth)
```

```
## 'data.frame':	60 obs. of  3 variables:
##  $ len : num  4.2 11.5 7.3 5.8 6.4 10 11.2 11.2 5.2 7 ...
##  $ supp: Factor w/ 2 levels "OJ","VC": 2 2 2 2 2 2 2 2 2 2 ...
##  $ dose: num  0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 ...
```

```r
head(ToothGrowth)
```

```
##    len supp dose
## 1  4.2   VC  0.5
## 2 11.5   VC  0.5
## 3  7.3   VC  0.5
## 4  5.8   VC  0.5
## 5  6.4   VC  0.5
## 6 10.0   VC  0.5
```

```r
summary(ToothGrowth)
```

```
##       len        supp         dose      
##  Min.   : 4.20   OJ:30   Min.   :0.500  
##  1st Qu.:13.07   VC:30   1st Qu.:0.500  
##  Median :19.25           Median :1.000  
##  Mean   :18.81           Mean   :1.167  
##  3rd Qu.:25.27           3rd Qu.:2.000  
##  Max.   :33.90           Max.   :2.000
```

####Quick look show that the ToothGrowth dataset consists of 60 observations with 3 columns . "len" which shows the length of tooth growth during the test. "supp" which classifies the treatments to the teeth of either OJ (orange juice) or VC (Asorbic Acid).  And "dose" which reports the dosage of each treatment.

###Exploratory Analysis
#####First we will look at a couple of box plot of the dosages and tooth growth for the treatments
1. Here is the seperate dosages and tooth measurement per treatment


```r
library(ggplot2)
t = ToothGrowth
levels(t$supp) <- c("OJ - Orange Juice", "VC - Ascorbic Acid")
ggplot(t, aes(x=factor(dose), y=len)) + 
  facet_grid(.~supp) +
  geom_boxplot(aes(fill = supp), show.legend = FALSE) +
  labs(title="Guinea pig tooth length by dosage for each type of supplement", 
    x="Dose (mg/day)",
    y="Tooth Length")
```

![](Statistical_Inference_Course_Project_2_Part_2_files/figure-html/unnamed-chunk-3-1.png) 

2. Next we will look at tooth growth as a function of treatment

```r
ggplot(aes(x=supp, y=len), data=ToothGrowth) + 
  geom_boxplot(aes(fill=supp))
```

![](Statistical_Inference_Course_Project_2_Part_2_files/figure-html/unnamed-chunk-4-1.png) 

####A first observation appears to show a link OJ (orange juice) and increase tooth growth versus VC (asorbic acid) at the dosage levels of .5 and 1.0. It is then shown to outperform VC on a total tooth growth measurement for the experiment in the second analysis. It does need to be noted however the appearance of a correlation to dosage of treatment and tooth growth between both.

##Use confidence intervals and hypothesis tests to compare tooth growth by supp and dose.


###1.  For the dosage of 0.5 mg/day, the two supplements deliver the same tooth growth.

```r
Hypothesis1 <- t.test(len ~ supp, data = subset(t, dose == 0.5))
Hypothesis1$conf.int
```

```
## [1] 1.719057 8.780943
## attr(,"conf.level")
## [1] 0.95
```

```r
Hypothesis1$p.value
```

```
## [1] 0.006358607
```
The confidence interval does not include 0 and the p-value is below the 0.05 threshold. The null hypothesis can be rejected. The alternative hypothesis that 0.5 mg/day dosage of orange juice delivers more tooth growth than ascorbic acid is accepted.

###2.  For the dosage of 1 mg/day, the two supplements deliver the same tooth growth

```r
Hypothesis2 <- t.test(len ~ supp, data = subset(t, dose == 1))
Hypothesis2$conf.int
```

```
## [1] 2.802148 9.057852
## attr(,"conf.level")
## [1] 0.95
```

```r
Hypothesis2$p.value
```

```
## [1] 0.001038376
```
The confidence interval does not include 0 and the p-value is smaller than the 0.05 threshold. The null hypothesis can be rejected. The alternative hypothesis that 1 mg/day dosage of orange juice delivers more tooth growth than ascorbic acid is accepted.

###3. For the dosage of 2 mg/day, the two supplements deliver the same tooth growth

```r
Hypothesis3 <- t.test(len ~ supp, data = subset(t, dose == 2))
Hypothesis3$conf.int
```

```
## [1] -3.79807  3.63807
## attr(,"conf.level")
## [1] 0.95
```

```r
Hypothesis3$p.value
```

```
## [1] 0.9638516
```
The confidence interval does include 0 and the p-value is larger than the 0.05 threshold. The null hypothesis cannot be rejected.

##SUMMARY

####Orange juice delivers more tooth growth than ascorbic acid for dosages 0.5 & 1.0. Orange juice and ascorbic acid deliver the same amount of tooth growth for dose amount 2.0 mg/day. For the entire data set we cannot conclude orange juice is more effective that ascorbic acid. 
