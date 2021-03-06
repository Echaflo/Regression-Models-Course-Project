---
title: "Regression Models Course Project"
author: "echf"
date: "3/9/2021"
output:
  pdf_document: default
  html_document:
    keep_md: yes
---

## Executive Summary

##### You work for Motor Trend, a magazine about the automobile industry. Looking at a data set of a collection of cars, they are interested in exploring the relationship between a set of variables and miles per gallon (MPG) (outcome). They are particularly interested in the following two questions:

* Is an automatic or manual transmission better for MPG
* Quantify the MPG difference between automatic and manual transmissions  



## Exploring the dataset

```{r}
echo = TRUE
options(width=80)

library(ggplot2) #for plots
```

```{r}
head(mtcars)
data(mtcars)
```

```{r}
summary(mtcars)
```



```{r}

# Transform certain variables into factors
mtcars$cyl  <- factor(mtcars$cyl)
mtcars$vs   <- factor(mtcars$vs)
mtcars$gear <- factor(mtcars$gear)
mtcars$carb <- factor(mtcars$carb)
mtcars$am   <- factor(mtcars$am,labels=c("Automatic","Manual"))
```


```{r fig.align="center"}
## Histgram of MPG
hist(mtcars$mpg, breaks=12, xlab="Miles Per Gallon (MPG)", main="MPG Distribution",
     col="green")
```



##  Regression Analysis

#### We’ve visually seen that automatic is better for MPG, but we will now quantify his difference.

```{r}
aggregate(mpg~am, data = mtcars, mean)

```
```{r fig.align="center"}
boxplot(mpg ~ am, data = mtcars, xlab = "Transmission type")
```

#### We will use mpg as the dependent variable and am as the independent variable to fit a linear regression, where Beta1 is the group mean for automatic and Beta0 is the intercept.

```{r}
fit_simple <- lm(mpg ~ factor(am), data=mtcars)
summary(fit_simple)
```

#### It shows that on average, a car has 17.147 mpg with automatic transmission, and if it is manual transmission, 7.245 mpg is increased. This model has the Residual standard error as 4.902 on 30 degrees of freedom. And the Adjusted R-squared value is 0.3385, which means that the model can explain about 34% of the variance of the MPG variable. The low Adjusted R-squared value also indicates that other variables should be added to the model.


## Anova test and Residuals

#### Finally, the final model is selected.


```{r}

init <- lm(mpg ~ am, data = mtcars)
betterFit <- lm(mpg~am + cyl + disp + hp + wt, data = mtcars)
anova(init, betterFit)
```
#### This results in a p-value of 8.637e-08, and we can claim the betterFit model is significantly better than our init simple model. We double-check the residuals for non-normality  and can see they are all normally distributed and homoskedastic.



## Residual Analysis and Diagnostics

#### According to the residual plots, the following underlying assumptions can be varified: 
     1. The Residuals vs. Fitted plot shows no consistent pattern, supporting the accuracy 
         of the independence assumption. 
     2. The Normal Q-Q plot indicates that the residuals are normally distributed 
         because the points lie closely to the line.
     3. The Scale-Location plot confirms the constant variance assumption, 
         as the points are randomly distributed.
     4. The Residuals vs. Leverage argues that no outliers are present,
         as all values fall well within the 0.5 bands.


```{r fig.align="center"}
par(mfrow = c(2,2))
plot(betterFit)
```

```{r}
```










