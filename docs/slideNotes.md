# Statistics for Lunch: Linear Regression
Notes for the talk on linear regression for the Statistics for Lunch series. Click [here](http://abcsFrederick.github.io/LinearRegression/slides.html) to view the slides.

## Overview

This presentation is broken into three major parts:

* Least squares intuition will give a visual explanation of how least squares regression works.
* The assumptions section will cover assumptions we make when performing linear regression and some tools we can use to check these assumptions.
* The transformations section will give some examples of different transformations we can use to meet the linearity assumption of linear regression.

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