Statistics for Lunch: Linear Regression
=======================================

Welcome to the repository for the linear regression materials for
Statistics for Lunch! There are two main branches in this repository:

Master
------

The master branch is where the exercises for the hands-on session are
located as well as the code used to generate the slides for the lecture.

-   Files located in the `docs/` directory were used to generate the
    lecture slides. The slides are located there as well, but click
    [here](http://abcsFrederick.github.io/LinearRegression/slides.html)
    to view the slides.

-   More detailed lecture notes can be found
    [here](https://github.com/abcsFrederick/LinearRegression/blob/master/docs/slideNotes.md)
    if you missed the lecture portion or wish to review the
    presentation.

Solutions
---------

The solutions branch is where solutions to the hands-on practice
problems will be deposited.

Linear Regression Lab
=====================

This week we will practice testing and correcting model assumptions for
linear models. Each of the data sets we will practice with have
different problems that you will need to sort out.

Example
-------

In this toy example, we have a variable with a non-linear relationship,
as well as a couple of variables that are more or less strongly
correlated. Note how the tests of our assumptions are affected by these
violations.

    require(tidyverse)

    # the data
    n <- 500

    set.seed(23497)
    dat <- data_frame(x1 = rnorm(100),
                      x2 = rnorm(100),
                      x3 = x1 + rnorm(100),
                      x4 = x3 + rnorm(100),
                      y = x1 + 2*x2 + x2^2 + x3 + x4 + rnorm(100))

    # first model
    model0 <- lm(y ~ x1 + x2 + x3 + x4, data = dat)

    # test for linearity
    crPlots(model0)

![](lab_files/figure-markdown_strict/example-1.png)

    # test for multivariate normality
    require(broom)
    with(augment(model0), shapiro.test(.std.resid))

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  .std.resid
    ## W = 0.96648, p-value = 0.01196

    qqPlot(model0)

![](lab_files/figure-markdown_strict/example-2.png)

    # test for multicollinearity
    vif(model0)

    ##       x1       x2       x3       x4 
    ## 1.774338 1.063819 3.665218 2.828375

    # test for autocorrelation
    durbinWatsonTest(model0)

    ##  lag Autocorrelation D-W Statistic p-value
    ##    1      0.07011517      1.857625     0.5
    ##  Alternative hypothesis: rho != 0

    # test for homoscedasticity
    ncvTest(model0)

    ## Non-constant Variance Score Test 
    ## Variance formula: ~ fitted.values 
    ## Chisquare = 0.2710789    Df = 1     p = 0.602609

    spreadLevelPlot(model0)

    ## Warning in spreadLevelPlot.lm(model0): 41 negative fitted values removed

![](lab_files/figure-markdown_strict/example-3.png)

    ## 
    ## Suggested power transformation:  0.7937753

This model looks nicer:

    model1 <- lm(y ~ x1 + identity(x2^2) + x4, data = dat)

    # a couple of tests
    crPlots(model1)

![](lab_files/figure-markdown_strict/example%20fixed-1.png)

    with(augment(model0), shapiro.test(.std.resid))

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  .std.resid
    ## W = 0.96648, p-value = 0.01196

    vif(model1)

    ##             x1 identity(x2^2)             x4 
    ##       1.381205       1.004667       1.385173

    # final model results
    summary(model1)

    ## 
    ## Call:
    ## lm(formula = y ~ x1 + identity(x2^2) + x4, data = dat)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -5.8918 -1.0031  0.3043  1.5051  4.3659 
    ## 
    ## Coefficients:
    ##                Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)     -0.4035     0.3023  -1.335    0.185    
    ## x1               1.2586     0.2842   4.428 2.52e-05 ***
    ## identity(x2^2)   1.6148     0.1981   8.150 1.36e-12 ***
    ## x4               1.8210     0.1794  10.151  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2.364 on 96 degrees of freedom
    ## Multiple R-squared:  0.7685, Adjusted R-squared:  0.7613 
    ## F-statistic: 106.2 on 3 and 96 DF,  p-value: < 2.2e-16

Height and weight
-----------------

Here we look at some data for the [!Kung
San](https://en.wikipedia.org/wiki/%C7%83Kung_people) people, referenced
in [McElreath](http://xcelab.net/rm/statistical-rethinking/).

-   Create a statistical model describing your expected relationship
    between weight and the other variables,
-   Check your model assumptions and revise accordingly.

<!-- -->

    howell1 <- read_delim('https://raw.githubusercontent.com/rmcelreath/rethinking/master/data/Howell1.csv', ';') %>%
        mutate(sex = ifelse(male, 'male', 'female'))

    ## Parsed with column specification:
    ## cols(
    ##   height = col_double(),
    ##   weight = col_double(),
    ##   age = col_double(),
    ##   male = col_integer()
    ## )

Simulated glomerular filtration rate
------------------------------------

In this simulated data set, we are going to explore glomerular
filtration rate as a function of age, gender and blood pressure. The
gist sourced below will create a data set called `dat`.

-   Create a statistical model describing your expected relationship
    between glomerular filtration rate (gfr) and the other variables,
-   Check your model assumptions and revise accordingly.

<!-- -->

    # this will create a variable called `dat`
    source('https://tinyurl.com/y78ws8gk')
