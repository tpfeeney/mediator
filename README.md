
# mediator <img src="man/figures/hex.png" align="right" height="139" />

The goal of `mediator` is to conduct causal mediation analysis under the
counterfactual framework, allowing interation between the exposure and
mediator \[Valeri 2013\]. Currently, `mediator` estimates the controlled
direct effect (CDE), natural direct effect (NDE), natural indirect
effect (NIE), total effect (TE) and proportion mediated (PM) and their
95% confidence intervals.

## Installation

You can install `mediator` from github with:

``` r
# install.packages("devtools")
devtools::install_github("gerkelab/mediator")
```

## Usage

`mediator` currently implements mediation analysis for dichotomous and
count mediators and outcomes, as well as censored time-to-event
outcomes. Estimate validity assumes proper modeling on the part of the
user.

The following example uses `example250` from within `mediator`, which
was randomly generated, and assumes `x` (binary) is our independent
variable/treatment, `y` (continuous) as our mediator, `cens` (binary) as
our outcome and `c` (continuous) as a confounder that needs to be
adjusted for.

![](man/figures/mediator-dag.png)

In the above DAG the path for the NIE is shown in blue while the path
for the NDE is in purple. The TE is the combined effect of both the NIE
and NDE.

The simplest use case of `mediator` would be as follows:

``` r
mediator::mediator(data = example250,
                   out.model = glm(cens ~ x + y + c + x*y, 
                                   family = "binomial",
                                   data = example250),
                   med.model = lm(y ~ x + c, 
                                  data = example250),
                   treat = "x", mediator = "y",
                   out.reg = "logistic", med.reg = "linear")
##                Effect Estimate Lower 95% CI Upper 95% CI
## 1                 CDE  0.42792      0.14331      1.27772
## 2                 NDE  0.71508      0.20414      2.50483
## 3                 NIE  1.06904      0.82487      1.38549
## 4        Total Effect  0.76445      0.25065      2.33153
## 5 Proportion Mediated -0.20959         <NA>         <NA>
```

A data frame (printed to the console if not assigned to an object) is
returned containing the point estimates and 95% confidence intervals.

## Additional resources

For an in-depth explanation of mediation analysis or complementary tools
for SAS or SPSS users, please check out Linda Valeri and Tyler
VanderWeele’s paper and macros, which are available on VanderWeele’s
[website](https://www.hsph.harvard.edu/tyler-vanderweele/tools-and-tutorials/).

The parametric model-based approach of `mediator` differs from another R
package,
[`mediation`](https://cran.r-project.org/web/packages/mediation/vignettes/mediation.pdf),
which conducts mediation analysis under a non-parametric framework.

-----

### References

Valeri, L., and T. J. Vanderweele. 2013. “Mediation analysis allowing
for exposure-mediator interactions and causal interpretation:
theoretical assumptions and implementation with SAS and SPSS macros.”
Psychol Methods 18 (2): 137–50.
