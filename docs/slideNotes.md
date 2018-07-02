# Statistics for Lunch: Linear Regression
Notes for the talk on linear regression for the Statistics for Lunch series. Click [here](http://abcsFrederick.github.io/LinearRegression/slides.html) to view the slides.

## Overview

This presentation is broken into three major parts:

* Least squares intuition will give a visual explanation of how least squares regression works.
* The assumptions section will cover assumptions we make when performing linear regression and some tools we can use to check these assumptions.
* The most common cause for a violation of assumptions is trying to fit the wrong model to the data. The transformations section will give some examples of different transformations we can use to model our data.

## Least Squares Intuition

### Slide 1

* This is a data set I made up to show how least squares regression works.
* The regression line here minimizes the error terms - that is, the distance from each point to the regression line.

### Slide 2

* Here we have added a visual representation of the error terms.
* The error terms in a regression are the distance between the predicted value for `y` and the observed value for `y`.
* In this figure, we have colored the error terms based on how far away from the regression line they lie.
    * Those with bright orange coloring are further away from the regression line, 
    * and those with darker/black coloring are closer to the regression line.
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

### Component Residual Plots

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
