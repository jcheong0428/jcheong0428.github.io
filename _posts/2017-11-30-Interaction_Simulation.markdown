---
layout: post
title:  "Interpreting coefficients in regressions with continuous and categorical variable interactions"
date:   2017-11-30 11:24:14 -0400
categories: jekyll update
comments: true
tags: statistics regression r simulation
---

<div style="text-align:center">
<img src="/assets/post20171130/main.jpg" width="750">  
</div><br>

The ability to understand and interpret the results of regressions is fundamental for effective data analysis. Often however, it is difficult to fully understand what happens behind the scenes when we specify and estimate a model in softwares such as R. Understanding how each term was represented in estimating the model is critical to interpret the model results accurately.

Once common mistake is interpreting the coefficient of a continuous variable as the average main effect when you have a categorical variable that interacts with the continuous variable. Here I provide the R code to demonstrate and explain why you cannot simply interpret the coefficient as the main effect unless you have specified a contrast.

TLDR: You should only interpret the coefficient of a continuous variable interacting with a categorical variable as the average main effect when you have specified your categorical variables to be a contrast. You cannot interpret it as the main effect if the categorical variables are dummy coded.

To illustrate, I am going to create a fake dataset with variables Income, Age, and Gender. My specification is that for Males, Income and Age have a correlation of r = .80, while for Females, Income and Age have a correlation of r = .30.

From this specification, **the average effect of Age on Income, controlling for Gender should be .55 (= (.80 + .30) / 2 ).**

As for mean group differences, let's say Males earn on average $2, while Females earn on average $3.  
Data for each gender is generated separately then concatenated to create a combined dataframe: `data`. Data is generated in `R` using `mvrnorm` from package `MASS`:

``` r
require(MASS)
# Generage fake data for male
data_male <- data.frame(mvrnorm(n=1000,mu=c(2,0),Sigma=rbind(c(1,.8),c(.8,1)),empirical=TRUE ) )
colnames(data_male)<-c('Income','Age')
data_male$Gender = 'Male'
# Generate fake data for female
data_female <- data.frame(mvrnorm(n=1000,mu=c(3,0),Sigma=rbind(c(1,.3),c(.3,1)),empirical=TRUE ))
colnames(data_female) <- c('Income','Age')
data_female$Gender = 'Female'
# Combine data
data <-rbind(data_female,data_male)
data$Gender<-as.factor(data$Gender)
```

We can check if the data we created has the correlation and average we specified:

``` r
cat('Correlation between Income & Age for Male:',cor(data_male$Income, data_male$Age))
cat('Correlation between Income & Age for Female:',cor(data_female$Income, data_female$Age))
cat('Mean Income for Male:',mean(data_male$Income))
cat('Mean Income for Female:',mean(data_female$Income))
```

    ## Correlation between Income & Age for Male: 0.8
    ## Correlation between Income & Age for Female: 0.3
    ## Mean Income for Male: 2
    ## Mean Income for Female: 3

Now, we naively run a linear model predicting Income from Age, Gender, and their interaction.

``` r
m <- lm('Income~Age*Gender',data=data)
summary(m)
```

    ##
    ## Call:
    ## lm(formula = "Income~Age*Gender", data = data)
    ##
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max
    ## -3.4916 -0.4905 -0.0051  0.5044  3.2038
    ##
    ## Coefficients:
    ##                Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)     3.00000    0.02521  118.99   <2e-16 ***
    ## Age             0.30000    0.02522   11.89   <2e-16 ***
    ## GenderMale     -1.00000    0.03565  -28.05   <2e-16 ***
    ## Age:GenderMale  0.50000    0.03567   14.02   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.7973 on 1996 degrees of freedom
    ## Multiple R-squared:  0.4921, Adjusted R-squared:  0.4913
    ## F-statistic: 644.6 on 3 and 1996 DF,  p-value: < 2.2e-16

The model summary prints coefficients for the Intercept, Age, GenderMale, Age:GenderMale. The effect of Age is .30 which is **NOT the average effect controlling for gender but the effect for the Female group**. The effect of GenderMale is $-1 which is how much Male earn less than Female which is the Intercept $3. Lastly, the interaction Age:GenderMale is how much more Income correlates with Age for Male in addition to Female.

Importantly, this is the default R behavior with categorical variables that it **alphabetically sets the first variable as the reference level* (i.e., the intercept). So in our case Female has been set as our reference level. Then, our categorical variables are dummy coded (a.k.a., treatment contrast) so that Females are 0s, and Males are 1s, which can be verified by using `contrasts`.

``` r
contrasts(data$Gender)
```

    ##        Male
    ## Female    0
    ## Male      1

So, what do we need to do to get the AVERAGE effect of Age on Income controlling for Gender while keeping the interaction?   
The answer is: **specify a contrast so that Females are coded as -.50 and males coded as .50.**

``` r
contrasts(data$Gender) <- c(-.5,.5)
m <- lm('Income~Age*Gender',data=data)
summary(m)
```

    ##
    ## Call:
    ## lm(formula = "Income~Age*Gender", data = data)
    ##
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max
    ## -3.4916 -0.4905 -0.0051  0.5044  3.2038
    ##
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  2.50000    0.01783  140.23   <2e-16 ***
    ## Age          0.55000    0.01784   30.84   <2e-16 ***
    ## Gender1     -1.00000    0.03565  -28.05   <2e-16 ***
    ## Age:Gender1  0.50000    0.03567   14.02   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.7973 on 1996 degrees of freedom
    ## Multiple R-squared:  0.4921, Adjusted R-squared:  0.4913
    ## F-statistic: 644.6 on 3 and 1996 DF,  p-value: < 2.2e-16

We get four terms again but they are specified as Intercept, Age, Gender1, and Age:Gender1.   
The Age effect is **0.55 which is exactly the average effect across gender** as we specified ( (0.8+0.3) / 2).    
The Age:Gender1 interaction is 0.5 which is the **difference** between the age effects between gender (0.8 - 0.3).  
Effect of Gender1 is $-1 which is representing the average difference between the two genders ($2-$3), as specified by our contrast.  
The reference Intercept is $2.5 which is the average income across gender ( ($2+$3) / 2 ). The point is now our Gender1 coefficient represents the difference of effect between the gender.

Once again, we can verify what our contrast was.

``` r
contrasts(data$Gender)
```

    ##        [,1]
    ## Female -0.5
    ## Male    0.5

### Conclusion

I hope this example makes it clear that when you run linear models with interactions between continuous and categorical variables, you need be careful in how they are represented (dummy coded or contrasts) as this will change how you interpret the coefficients.

You should only interpret the coefficient of a continuous variable interacting with a categorical variable as the average main effect when you have specified your categorical variables to be a contrast. You cannot interpret it as the main effect if the categorical variables are dummy coded as they are the effect for the reference level.

This question has also been asked online such as in this post on [stackexchange](https://stats.stackexchange.com/questions/41129/interpreting-coefficients-of-an-interaction-between-categorical-and-continuous-v) and this post on [r-bloggers](https://www.r-bloggers.com/interpreting-interaction-coefficient-in-r-part1-lm/)

Personally, I first learned this thanks to [Professor George Wolford](http://pbs.dartmouth.edu/people/george-wolford) in his graduate statistics course at Dartmouth College.


### Additional info...

If you want your categorical variables to be treated as dummy codes, you can set it as a treatment contrast. Which replicate the default result provided by R.

``` r
contrasts(data$Gender) <- contr.treatment(2)
m <- lm('Income~Age*Gender',data=data)
summary(m)
```

    ##
    ## Call:
    ## lm(formula = "Income~Age*Gender", data = data)
    ##
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max
    ## -3.4916 -0.4905 -0.0051  0.5044  3.2038
    ##
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  3.00000    0.02521  118.99   <2e-16 ***
    ## Age          0.30000    0.02522   11.89   <2e-16 ***
    ## Gender2     -1.00000    0.03565  -28.05   <2e-16 ***
    ## Age:Gender2  0.50000    0.03567   14.02   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.7973 on 1996 degrees of freedom
    ## Multiple R-squared:  0.4921, Adjusted R-squared:  0.4913
    ## F-statistic: 644.6 on 3 and 1996 DF,  p-value: < 2.2e-16

If you run the model *without* the interaction, then even if your categorical variables are dummy coded, the main effect of Age is the **average effect controlling for Gender** as you would expect.

``` r
m <- lm('Income~Age+Gender',data=data)
summary(m)
```

    ##
    ## Call:
    ## lm(formula = "Income~Age+Gender", data = data)
    ##
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max
    ## -3.5525 -0.5102 -0.0055  0.5424  3.2461
    ##
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  3.00000    0.02642  113.56   <2e-16 ***
    ## Age          0.55000    0.01869   29.43   <2e-16 ***
    ## Gender2     -1.00000    0.03736  -26.77   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.8354 on 1997 degrees of freedom
    ## Multiple R-squared:  0.4421, Adjusted R-squared:  0.4416
    ## F-statistic: 791.3 on 2 and 1997 DF,  p-value: < 2.2e-16
