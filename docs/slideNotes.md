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

#### Influence and Leverage

This illustration also provides a good way to visualize leverage and influence.

* If this were a seesaw with the fulcrum here in the middle, you would expect the points on the ends to have the greatest leverage.
    * In fact, these points do have a larger influence on the regression line than the data points in the middle.
    * We will talk more about evaluating influence quantitatively later, but this will give us some good intuition as to why some points have more influence over the regression terms than other points.
    
## Assumptions

### Linear Relationship

The first assumption we discuss here is that there is a linear relationship between the dependent and independent variables.

In this data set, the relationship between `x1` and `y` is linear, but the relationship between `x2` and `y` is quadratic.

<<<<<<< Updated upstream
#### Component Residual Plots
=======
### Component Residual Plots
>>>>>>> Stashed changes

A Component Residual Plot shows the relationship between each independent variable and the dependent variable after conditioning on all the other dependent variables in the model.

* The red, dashed line represents the best fit line to the data.
* The green, solid line represents a smoothed line, sort of like a running average, of the data.
* Both lines should be pretty similar.

When we look at these two models:

* Model 1 (top two figures) clearly violates the linearity assumption.
* Model 2 (bottom two figures) looks like it meets the linearity assumption.

### Multivariate Normality

The next assumption we will discuss is the multivariate normality (MVN) assumption: we assume that the error terms (i.e. how far away our observed data points are from the predicted values) are normally distributed.

* The error terms about `y1` are normally distributed.
    * The cloud of points on the left look pretty typical.
* The error terms about `y2`, however, are t~3~-distributed.
    * The cloud of points on the right are much more spread out.
    * This is not just a function of the variance being greater - there appear to be at least four outliers, perhaps more.

#### Diagnostics (good)

This QQ-plot shows us the observed error terms (on the y-axis) and the expected distribution of the error terms (on the x-axis).

* The points on this plot should fall along this solid, red line.
* Some variation is to be expected, but they should fall inside of the dashed, red lines.

A Shapiro-Wilk test of the residuals will give us a quantitative measure of wether we have violated the MVN assumption.

* Null hypothesis: the residuals are normally distributed.
* p-value > 0.05: we fail to reject the null hypothesis. There is no evidence indicating we have violated the MVN assumption.

#### Diagnostics (bad)

Again, we have the expected distribution of the error terms (on the x-axis) plotted against the observed error terms (on the y-axis).

* Here, however, we have four data points outside of the range of expected values.
* Also, the Shapiro-Wilk p-value < 0.00005: we reject the null hypothesis and conclude that the MVN assumption is violated.

#### Model comparison (2 slides)

| Model    | Estimate | SE   | R^2^  | 
|:---------|:---------|:-----|:------|
| Expected | 1.00     | 0.10 | 0.500 |
| 1 (MVN)  | 0.91     | 0.09 | 0.504 |
| 2 (t~3~) | 0.80     | 0.18 | 0.166 |
<<<<<<< Updated upstream

### No/Little Multicollinearity

The next assumption we address is that there is no/little multicollinearity among the independent variables. Here we have a data set in which `x2` is correlated with `x1`. We will compare three different models using this data set:

- 1 includes all three independent variables,
- 2 does not include `x2`, and
- 3 does not include `x1`

#### Multicollinearity: Variance Inflation Factors (VIF)

The `car::vif()` function will compare the variance inflation when including (vs excluding) each variable in the model and give us a measure of how the variance changes when a variable is added to the model. A value greater than 2 is grounds for concern that the variable is correlated with one or more of the other independent variables.

- 1 we see that both `x1` and `x2` have high VIF scores. This is an indicator of multicollinearity.
- 2/3 when excluding `x1` or `x2` the VIF scores for the remaining indepenednt variables look much better.

#### Multicollinearity: Model comparison

When we compare these models with eachother, we see that the coefficients for `x1` and `x2` change quite a bit depending on what variables are included in the model. These coefficients are going to be difficult to estimate for the individual effects of `x1` and `x2`, but note that the predited values are a little better when including them both (as measured by R^2^).

If we are only concerned with prediction, this assumption isn't as important to meet. If, however, we are interested in measuring the effects of the independent variables, multicollinearity will result in unstable model coefficients.

### No Autocorrelation (3 slides)

The next assumption we make is that there is no autocorrelation. This figure might look OK at first glance until we add a loess smothed line to the data. Doing so highlights this periodic pattern in the data.

We can test this assumption with `car:durbinWatsonTest()`. There are terms we can add to the model to account for these patterns, but this is beyond the scope of this introduction. If your data are subject to autocorrelation, the variance of your estimates is going to be inflated. Consult a statistician.

### Homoscedasticity

The next assumption we make is that our error terms are homoscedastic. That is, they have constant variance as the dependent variable increases in magnitude. In this example, we see that the error terms for `y1` appear to be fairly constant, but the error terms for `y2` do not. There is an increase in the spread around the trend line as `y2` increases in the second figure.

#### Homoscedasticity: Contrast and Compare (4 slides)

The first model, predicting the value of `y1`, has a flat trend line when plotting the residuals against the fitted values. That indicates that the variance of the error terms is constant as `y1` increases. The `car:ncvTest()` results below the figure give us a quantitative measure confirming our intuition from the `car:spreadLevelPlot()` (p-value well above 0.05).

The second model, predicting the value of `y2`, has a positive slope when plotting the residuals against the fitted values. This indicates that the variance of the error terms increases as the value of `y2` increases. The `car:ncvTest()` results below the figure give us a quantitative measure confirming our intuition from the `car:spreadLevelPlot()` (extremely small p-value - we reject the null hypothesis that the variance is constant).

The next two slides show the model output from each of these models. Note the increased standard error in the second model. This is going to result in incorrect p-values and confidence intervals. The residuals of the second model look fairly semetric in the interquartile range, but there appear to be some outliers on the upper end.

## Influential Observations

Probably the most frequent reason for violations of our model assumptions is that we have picked the wrong model, but it is also common that our problems stem from a small number of influential observations.

In this illustration, we see that how the regression line changes as the red point moves around the figure. The line changes very little when the red point is in the center of the distribution of `x`, but as it moves to the edges of the range of `x` it has more leverage.

This operates under the same principle as a seesaw. If you load up a seesaw with a bunch of kids, adding an adult to the middle of the seesaw will not unbalance the seesaw (higher weight in this case correlates with a higher error term in our regression model). Adding the adult to either end of the seesaw, however, will have a much greater effect on the balance of the seesaw. Likewise, data points with large error terms will affect the slope of the regression line much more if they are at either end of the distribution of `x` than if they are in the middle of the distribution.

### Influential Observations: good example

Here we have the results of `car:outlierTest()` and `car:influencePlot()` for a typical dataset without any overly influential data points. The one potential outlier isn't statistically significantly extreme, as measured by the Bonferroni p-value, and the 

* Studentized Residuals (y-axis): a standardized measure of the residuals. The 20th data point is about 3.4 standard deviations away from the predicted value.
* Hat values (x-axis): gives a measure of how much the regression coefficients would change if the data point in question were removed from the model.
* Cook's distance (radius of the bubble): give a measure of how much the predicted values from the regression model would change if the data point in question were removed from the model.
=======
>>>>>>> Stashed changes
