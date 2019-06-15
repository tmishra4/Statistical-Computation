Chi-squared Test
================
Tapas Mishra
15/06/2019

### Introduction

Chi-squared test is useful in understanding if two categorical variables
have a significant correlation between them or not. These two variables
should come from the same population. For example, if we want to test if
the gender of a patient correlates, with the presence of a disease in
patients, considering gender is a categorical variable, with levels Male
/ Female.

This test is also useful , as a goodness of fit test - to compare if the
observed distribution is an expected distribution in population.

In this tutorial, we will understand -

1)  Chi-squared test for goodness of fit.
2)  Chi-squared test for independence.
3)  Chi-squared test as a test for homogeneity.

One thing to understand is, that first two tests are performed in a
quite similar fashion, but just interpreted in a different fashion. We
will also do some examples to understand the concept.

This test is useful in the predictive model building when we are trying
to identify important features, which has significant relationships with
the response.

### Chi-squared test as goodness of fit.

In this case - Null hypothesis
![\\H\_0](https://latex.codecogs.com/png.latex?%5CH_0 "\\H_0"): There is
no significant difference between the observed and the expected value.
Alternative hypothesis
![\\H\_a](https://latex.codecogs.com/png.latex?%5CH_a "\\H_a"): There is
a significant difference between the observed and the expected value.

#### Test Statistic

![\\chi^2= \\sum\_{i=1}^{n} \\frac{( E\_i -
O\_i)^2}{E\_i}](https://latex.codecogs.com/png.latex?%5Cchi%5E2%3D%20%5Csum_%7Bi%3D1%7D%5E%7Bn%7D%20%5Cfrac%7B%28%20E_i%20-%20O_i%29%5E2%7D%7BE_i%7D
"\\chi^2= \\sum_{i=1}^{n} \\frac{( E_i - O_i)^2}{E_i}")

where ![\\O\_i](https://latex.codecogs.com/png.latex?%5CO_i "\\O_i") is
observed values and ![\\E\_](https://latex.codecogs.com/png.latex?%5CE_
"\\E_") is Expected values. Under null hypothesis (and expected counts
at least 5) this has a Chi-squared distribution with df=k−1. Always 1
tailed–reject ![\\H\_0](https://latex.codecogs.com/png.latex?%5CH_0
"\\H_0") when ![\\chi^2](https://latex.codecogs.com/png.latex?%5Cchi%5E2
"\\chi^2") is large.

##### Simple Example

At a hospital , Number of Cardiac events in a week from Sun to Sat is
given as 24,36,27,26,32,26,29
![\\H\_0](https://latex.codecogs.com/png.latex?%5CH_0 "\\H_0") : There
is no significant difference between the observed and the expected
value. Equal probability of a cardiac event is expected.
![\\H\_a](https://latex.codecogs.com/png.latex?%5CH_a "\\H_a") : There
is a significant difference between the observed and the expected value.

Let us understand the working below -

``` r
x = c(24,36,27,26,32,26,29)
cat("Observed Values : " , x)
```

    ## Observed Values :  24 36 27 26 32 26 29

``` r
cat("\n","Expected values : ", round(rep(mean(x), length(x)),2) )
```

    ## 
    ##  Expected values :  28.57 28.57 28.57 28.57 28.57 28.57 28.57

``` r
cat("\n","O - E : ", x - round(rep(mean(x), length(x)),2) )
```

    ## 
    ##  O - E :  -4.57 7.43 -1.57 -2.57 3.43 -2.57 0.43

``` r
cat("\n","(O - E)^2 / E : ", round((x - mean(x))^2 / mean(x),2) )
```

    ## 
    ##  (O - E)^2 / E :  0.73 1.93 0.09 0.23 0.41 0.23 0.01

``` r
cat("\n","Chi - Square: ",  sum(round((x - mean(x))^2 / mean(x),2)))
```

    ## 
    ##  Chi - Square:  3.63

``` r
cat("\n","95th percentile Critical Value : ",  qchisq(.95 , length(x)-1))
```

    ## 
    ##  95th percentile Critical Value :  12.59159

So the ![\\chi^2](https://latex.codecogs.com/png.latex?%5Cchi%5E2
"\\chi^2") is much less than the critical value, so the H0 prevails.

A quick way to solve using R function. Default null is that categories
have equal probability.

``` r
x<-c(24 , 36 ,27,26, 32, 26,29)
chisq.test(x)
```

    ## 
    ##  Chi-squared test for given probabilities
    ## 
    ## data:  x
    ## X-squared = 3.63, df = 6, p-value = 0.7266

``` r
# To extract expected values:
round(chisq.test(x)$expected,2)
```

    ## [1] 28.57 28.57 28.57 28.57 28.57 28.57 28.57

### Chi-squared test as Test of Independence.

Two ways to think of independence of events A and B<br/> 1.P(A∩B)
=P(A)P(B).<br/> Multiplication rule for independentevents.<br/> 2.P(A|B)
=P(A|BC) =P(A). <br/> Conditioning provides no info. Chi-squared test of
independence uses <br/> (1) to test H0: A and B independent vs. HA: A
and B not
independent

##### Example

![](E:\\Projects\\Statistical%20Computation\\Statistical-Computation\\Example2.png)<!-- -->

If P(Remain) is 120/200 <br/> and P(1-5 years of service) is 45/200<br/>
P(Remain and 1-5 years of service)=120/200×45/200=0.135<br/> Therefore
expected count is 0.135×200=27<br/>

Same as before, where k enumerates all r×c cells.

![\\chi^2= \\sum\_{i=1}^{k} \\frac{( E\_i -
O\_i)^2}{E\_i}](https://latex.codecogs.com/png.latex?%5Cchi%5E2%3D%20%5Csum_%7Bi%3D1%7D%5E%7Bk%7D%20%5Cfrac%7B%28%20E_i%20-%20O_i%29%5E2%7D%7BE_i%7D
"\\chi^2= \\sum_{i=1}^{k} \\frac{( E_i - O_i)^2}{E_i}")

Degress of freedom is different:(r−1)×(c−1) , where r and c are total
rows and total columns respectively.

``` r
x<-matrix(c(10,30,5,75, 25, 15,10,30), ncol=4, byrow=TRUE)
x
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]   10   30    5   75
    ## [2,]   25   15   10   30

``` r
chisq.test(x)
```

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  x
    ## X-squared = 25.397, df = 3, p-value = 1.275e-05

``` r
cat("\n","95th percentile Critical Value : ", qchisq(.95 , df = (4-1)*(2-1)))
```

    ## 
    ##  95th percentile Critical Value :  7.814728

![\\chi^2](https://latex.codecogs.com/png.latex?%5Cchi%5E2 "\\chi^2") =
25.4 and cf 7.81, 0.95 quantile of Chi-sq distribution with df=
(4−1)×(2−1) = 3 <br/> pvalue\<0.0001; reject H0, Loyalty and length
of service are
notindependent.

##### Example 1

![](E:\\Projects\\Statistical%20Computation\\Statistical-Computation\\Example1.png)<!-- -->

1.  Write down appropriate null and alternative hypotheses.
2.  Compute the expected value for one cell, by hand. Compute the table
    of expected values in R and confirm that they match.
3.  Find the Chi-squared test statistic in your output. What
    distribution do we compare it to (including any parameters)? What is
    the critical value?
4.  Give the p-value and state your conclusion in context.

##### Sol 1

data = the expected votes of each party<br/> prob = the actual
proportion each party got votes in election<br/>

Null hypothesis = that expected proportion of votes are same to actual
proportion of votes in election alternate hypothesis = that expected
proportion of votes significantly different to actual proportion of
votes in election.

``` r
data = c(241,14,187,54,46)
prob = c(.38,.05,.38,.14,.05)
data
```

    ## [1] 241  14 187  54  46

``` r
prob
```

    ## [1] 0.38 0.05 0.38 0.14 0.05

``` r
chisq.test(data,p = prob)
```

    ## 
    ##  Chi-squared test for given probabilities
    ## 
    ## data:  data
    ## X-squared = 33.53, df = 4, p-value = 9.305e-07

``` r
E = round(chisq.test(data,p = prob)$expected,2)
cat("Actual Value","\n")
```

    ## Actual Value

``` r
data
```

    ## [1] 241  14 187  54  46

``` r
cat("Expected  Value","\n")
```

    ## Expected  Value

``` r
E
```

    ## [1] 205.96  27.10 205.96  75.88  27.10

``` r
cat("Expected  Value of one cell is 0.38*546 = ",0.38*546,"\n")
```

    ## Expected  Value of one cell is 0.38*546 =  207.48

``` r
cat("O-E","\n")
```

    ## O-E

``` r
data - E
```

    ## [1]  35.04 -13.10 -18.96 -21.88  18.90

``` r
cat("(O-E)^2 / E","\n")
```

    ## (O-E)^2 / E

``` r
(data - E)^2 / E
```

    ## [1]  5.961359  6.332472  1.745395  6.309099 13.181181

``` r
cat("Critical Value","\n")
```

    ## Critical Value

``` r
qchisq(0.95 , length(data)-1)
```

    ## [1] 9.487729

So the ![\\chi^2](https://latex.codecogs.com/png.latex?%5Cchi%5E2
"\\chi^2") = 33.53 , which is much larger than the critical value, also
the p-val is too small , so rejecting the null hypothesis. So expected
proportion of votes significantly different to actual proportion of
votes in election

##### Example 2

During my study of building a prediction model to predict participants
on high risk of heart disease using some of the health parameters as
predictors. I tested if presence of heart disease has any correlation
with population’s gender. Below is snippet. num is
response.

``` r
heart.data <- read.csv("https://archive.ics.uci.edu/ml/machine-learning-databases/heart-disease/processed.cleveland.data",header=FALSE,sep=",",na.strings = '?')
names(heart.data) <- c( "age", "sex", "cp", "trestbps", "chol","fbs", "restecg",
                   "thalach","exang", "oldpeak","slope", "ca", "thal", "num")

#converting sex and num into categorical variables

heart.data$sex <- factor(heart.data$sex,
                    levels = c(0,1),
                    labels = c("Female", "Male"))

heart.data$Heart.Disease.Indicator <- factor(heart.data$num,
                    levels = c(0,1,2,3,4))

#Display gender vs Heart disease counts
table(heart.data$sex , heart.data$Heart.Disease.Indicator )
```

    ##         
    ##           0  1  2  3  4
    ##   Female 72  9  7  7  2
    ##   Male   92 46 29 28 11

Lets look at the bar graph to visualize.

``` r
library(tidyverse)
```

    ## Registered S3 methods overwritten by 'ggplot2':
    ##   method         from 
    ##   [.quosures     rlang
    ##   c.quosures     rlang
    ##   print.quosures rlang

    ## -- Attaching packages ------------------------------------------------------------------------ tidyverse 1.2.1 --

    ## v ggplot2 3.1.1     v purrr   0.3.2
    ## v tibble  2.1.1     v dplyr   0.8.1
    ## v tidyr   0.8.3     v stringr 1.4.0
    ## v readr   1.3.1     v forcats 0.4.0

    ## -- Conflicts --------------------------------------------------------------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
ggplot(data = heart.data)+
  geom_bar(position = position_dodge(),
           width = 0.6,
           size = 0.5,
           color = "black",
           mapping = aes(x = sex , fill = Heart.Disease.Indicator ))+
  labs(x= "Heart Disease Levels By Gender", y = "Number of Patients", title = "Number of Patients based on Heart Disease Levels By Gender")
```

![](ChisqTest_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

We do see that males are more prone to heart disease as compared to
females. To verify, chi-square test is performed, for that null
hypothesis H0 is proposed. Null hypothesis states that disease has no
correlation with population’s gender, if this hypothesis is rejected by
chi-square test then, hypothesis HA is established which states that
disease has some correlation with population’s gender.

``` r
x = as.matrix(table(heart.data$sex , heart.data$num ))
chisq.test(x)
```

    ## Warning in chisq.test(x): Chi-squared approximation may be incorrect

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  x
    ## X-squared = 23.425, df = 4, p-value = 0.0001041

Therefore, the null hypothesis is rejected. Hence, it can be stated with
95% confidence that disease has some correlation with population gender.

Hope you Enjoyed \! Cheers.
