# Statistics for Lunch: Linear Regression
Notes for the talk on linear regression for the Statistics for Lunch series. Click [here](http://abcsFrederick.github.io/LinearRegression/slides.html) to view the slides.

## Overview

This presentation is broken into three major parts:

* Least squares intuition will give a visual explanation of how least squares regression works.
* The assumptions section will cover assumptions we make when performing linear regression and some tools we can use to check these assumptions.
* The most common cause for a violation of assumptions is trying to fit the wrong model to the data. The transformations section will give some examples of different transformations we can use to model our data.

## Least Squares Intuition

### Slide 1

* Our first example data set consists of the size of female sand fleas and the number of eggs she is carrying.
* The regression line here minimizes the error terms - that is, the distance from each point to the regression line.

### Slide 2

* Here we have added a visual representation of the error terms.
* The error terms in a regression are the distance between the predicted value for `y` and the observed value for `y`.
* In this figure, we have colored the error terms based on how far away from the regression line they lie.
    * Those with bright orange coloring are furthest away from the regression line, 
    * those with colored blue are at moderate distances from the regression line,
    * and those with darker/black coloring are closest to the regression line.
* In least squares regression, the line that minimizes the sum of the total squared error terms is the solution.

#### Minimization of error

* Imagine each of these lines is a rubber band, each pulling on the regression line.
* Rubber bands covering a greater distance (i.e. the bright orange error lines) will be tighter and pull on the line with more force.
* The equilibrium point (where the total force applied to the regression line is minimized) is the least squares regression line.

### Slide 3

This illustration shows the difference between the least squares regression line and a sub-optimal line.

* When we perturb the regression line, we see the error terms grow.
* When we let the line go back to it's equilibrium point, we get the best fit line for the data set, regressing y onto x.

#### Model Notation

Here we have an equation representing our best fit line. The number of eggs, `eggs`, is a function of the intercept, β~0~, and the weight of the sand flea in mg. The slope of our line is represented by β~1~, and we also have the error terms represented by ε.

#### Model Output

This model output is typical of what you get from R when you fit a linear regression model. The Estimate column of this table gives us the coefficients for the intercept and the slope of the line for mg, and the last column gives us the p-values for our coefficients. These low p-values tell us that our coefficients are statistically significant - that is, the slope of our line is probably not 0.

We can also use the numbers in the Estimate column to get the equation for our line. The predicted number of eggs is 12.7 plus 1.6 times the weight of the sand flea in mg.

## Assumptions

### Assumption 1: The error terms in our model are normally distributed

The first assumption we discuss here is that our error terns are multivariate normally distributed. We can test this assumption in a couple of different ways.

* Shapiro-Wilks test will give us a quantitative measure in the form of a p-value.
    * The null hypothesis is that the error terms are normally distributed.
    * Thus, we want a high p-value.
    * A low p-value means we have something out of the ordinary and that we have violated our multivariate normality assumption.
    * The Shapiro-Wilks test can be sensitive to outliers.
* The second test, a QQ-plot, is graphical and requires a judgement call on our part (more on this later).

#### Outliers

Because we have a borderline-significant result from our Shapiro-Wilks test, lets look for outliers.

* Using the `car::outlierTest` function, we identify one borderline-significant outlier.

#### Influence and Leverage

This illustration also provides a good way to visualize leverage and influence. Note how the regression line (solid, blue line) changes from the original regression line (dashed, black line) as the red point moves around the figure.

* If this were a seesaw with the fulcrum here in the middle, you would expect the points on the ends to have the greatest leverage.
    * In fact, these points do have a larger influence on the regression line than the data points in the middle.

#### Influence plot

Influence plots display three measures of influence and leverage:

* Studentized residuals (y-axis) show the number of standard deviations separate the predicted value from the observed value. Outliers will have a large studentized residual (either large positive or large negative).
* Hat values (x-axis) give a measure of how much the coefficients change when a specific data point is excluded from the data set. If a single data point strongly affects the slope and/or intercept, it will have a large hat value.
* Cook's distance (radius of each bubble) gives a measure of how much the error terms are affected when a specific data point is excluded from the model.

Point number 16 is the possible outlier from previous slides. It has a large residual (y-axis), but a relatively small hat value (x-axis). This is because it is near the center of the distribution of weights, so it doesn't have much leverage. It does, however, pull the trend line slightly higer, increasing the error terms across the board, so it has a large cooks distance (bubble radius).

Point number 24 is the red colored point from previous slides. It has a smaller resiual because it was closer to the trend line, but it has a much larger hat value because it is on the edge of the distribution. This gives it extra leverage and influence over the slope and intercept of the trend line. Because of this, it also has a large Cooks distance.

#### QQ-Plots

The x-axis of a QQ-plot shows the theretical distribution of the error terms, and the y-axis gives the observed error terms. If the data perfectly fit a normal distribution, then all the data points would lie directly on the solid blue line. The dotted lines mark the region in which we can expect our data to vary if the error terms are from a normal distribution. Note that the 16th data point is right near the edge of this region - indicating it is borderline significant.

Over all, and because the 16th data point has such a small amount of leverage, I would not worry about the normality assumption for this data set.

### Assumption 2: Linear Relationship

We assume that the relationship between the x and y variables is linear, but that does not mean we can't model a quadradic relationship like that shown here. For this assumption we will look at the size of gopher tortoises and clutch size. Very young (i.e. very small) and very old (i.e. very large) tortoises tend to have smaller clutch size, while those in the middle have a larger clutch size.

* The blue line represents the first model. It clearly does not model the relationship between carapace length and clutch size.
* The green line represents the second model with the length^2^ term added in.
    * This is a linear model with respect to the coefficients, since they are all constant values.
    * This model captures the quadratic relationship between carapace length and clutch size very well.
    
#### Model Output

The model output confirms this.

* The p-values for the first model are all large, and the coefficients are small. R can not identify much if any correlation between length and clutch size using this model.
* The p-values for the second model all appear to be statistically significant. This captures the correlation between length and clutch size much better.

#### Component Residual Plots

A Component Residual Plot shows the relationship between each independent variable and the dependent variable after conditioning on all the other dependent variables in the model.

* The blue, dashed line represents the best fit line to the data.
* The magenta, solid line represents a smoothed line, sort of like a running average, of the data.
* Both lines should be pretty similar.

When we look at these two models:

* Model 1 clearly violates the linearity assumption.
* Model 2 looks like it meets the linearity assumption. The magenta lines for length and length^2^ both fall directly over the top of the blue dashed line.

#### Modeling non-linear relationships

* Transform the data to make the relationship linear.
* Add terms to the linear model to capture the relationship (e.g. like we did above).
* Use a non-linear regression model.
    * This can be a powerful method, but lacks some of the statistics available in linear models.

### Assumption 3: No/Little Multicollinearity

The next assumption we address is that there is no/little multicollinearity among the independent variables. Here we have a breast cancer data set with many (30) correlated predictors. We want to use them to predict wether or not a tumor is metastatic.

#### Variance Inflation Factors (VIF)

The `car::vif()` function will compare the variance inflation when including (vs excluding) each variable in the model and give us a measure of how the variance changes when a variable is added to the model. A value greater than 2 is grounds for concern that the variable is correlated with one or more of the other independent variables. We have only included six of our 30 variables in this example.

Observations:

* We would expect the mean radius to be highly correlated with the mean perimeter. The VIF scores confirm this, as they are both very high for these two variables.
* We are probably not surprised to see that the mean area is also correlated with the radius and perimeter.
* Concavity also appears to be correlated with some of the other variables.

#### Options

If we are only concerned with prediction, this assumption isn't as important to meet. If, however, we are interested in measuring the effects of the independent variables, multicollinearity will result in unstable model coefficients. If we have significant amounts of multicollinearity we have the following options.

* Reduce the number of variables in the model
    * Pro: model statistics will behave better
    * Con: might loose some predictive power
* Reduce dimensionality using PCA
    * Pro: can include information from all variables
    * Con: interpretation of model statistics will be hard
* Leave everything in the model
    * Pro: can include all variables
    * Con: model statistics will behave badly
    * Con: too many variables will cause overfitting
* Penalized regression
* Random forests
* There are lots of other methods that we don't list here.

### Assumption 4: No Autocorrelation

The next assumption we make is that there is no autocorrelation. These data are from measles outbreaks in Baltimore, and we can immediately see that there are cycles of outbreaks followed by periods of very low incidence. These cycles introduce non-independence among the predictors.

#### Durbin Watson test

We can test this assumption with `car:durbinWatsonTest()`. There are terms we can add to the model to account for these patterns, but this is beyond the scope of this introduction. If your data are subject to autocorrelation, the variance of your estimates is going to be inflated. Consult a statistician or learn more about time series analysis.

### Assumption 5: Homoscedasticity

The next assumption we make is that our error terms are homoscedastic. That is, they have constant variance as the dependent variable increases in magnitude. In this example, we see that the error terms for `weight` appear to be fairly small for short individuals, but they are much larger among the tallest individuals.

This is probably due to the fact that we have a mixed population including both children and adults.

#### Spread Level Plot

Spread level plots show the precited values along the x-axis and the absolute error terms along the y-axis. We see that the variance of the errors is a lot smaller on the left than on the right. Ideally the smoothed magenta line will fall directly over the top of the blue dashed line, and the blue dashed line will have nearly 0 slope.

Scrolling down, we see the results of a quantitative test, the non-constant variance test. For this model this test gives us a relatively high p-value, indicating we probably don't have a heteroscedasticity problem. Looking back at the original figure, we probably have some issues with our linearity assumption, and perhaps the normailty assumption.

#### Options

If you do have heteroscedastic data, you could try one of the following techniques:

* Transform the data (usually log or power transformation).
* Add terms to the model (e.g. adding age to this model actually helps out quite a bit).
* Use a weighted regression analysis. We won't discuss this here, but look it up if you are interested in learning more.

### Transformations

Many of the problems we encounter in linear regression stem from poor model choice. We will give examples of the two most common transformations that can be used to fix bad assumptions: power transformations and log/exponential transformations.

#### Power transformations

The first of these two slides shows a squared transformation of the `x` variable. In the original data (left figure) we see a non-linear relationship between `x` and `y`. When we use the second model, with `x^2^`, we have a linear relationship.

The second power transformation slide shows a square root transformation of the `x` variable. Similar to the previous figures, changing `x` to `sqrt(x)` fixes our linearity assumption.

#### Log/exponential Transformations

The first of these two slides shows a log transformation of the `x` variable. In the untransformed data (left figure) we see a lot of the data are clustered around 0. The log transformation rescales these such that we have a linear relationship between `log(x)` and `y`.

The second of these two slides shows a log transformation of the `y` variable. In the untransformed data (left figure) we see a lot of the data are clustered around 0 on the y-axis. The log transformation rescales these such that we have a linear relationship between `x` and `log(y)`.

## Assumptions Summary

This table summarizes the assumptions from this presentation. You may note that the cause of violating each assumption is most often due to a poor model.

The consequence of violating assumptions is usually that our statistics are misleading. For example, this could result in a p-value that is too small or a confidence interval that is too tight. This might sound like a good thing (who doesn't like significant results?), but if we stop to consider the implications, we will be less eager. Bad statistics lead to bad inferences, and others will have a hard time replicating your results.

The last column gives the main functions I used in this presentation to test my model assumptions.