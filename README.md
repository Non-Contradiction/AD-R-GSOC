# Automatic Differentiation in R through Julia

## Background

Automatic differentiation (AD) is a set of techniques to calculate the derivatives automatically.
It generally outperform non-AD methods like symbolic differentiation and numerical approximation
in speed or/and accuracy.
It has important applications in many fields, like optimization, machine learning,
Bayesian statistics and differential equations.
[`Julia`](https://julialang.org/) is a high-level, high-performance dynamic
programming language for numerical computing.
While there is a lack of automatic differentiation package in R,
`Julia` has mature automatic differentiation packages,
like [`ForwardDiff.jl`](https://github.com/JuliaDiff/ForwardDiff.jl)
for forward mode AD and
[`ReverseDiff.jl`](https://github.com/JuliaDiff/ReverseDiff.jl)
for reverse mode AD.
The aim of this project is to develop an R wrapper for the `Julia` AD packages
`ForwardDiff.jl` and `ReverseDiff.jl` by the use of R packages which embedds `Julia`
in R like [`JuliaCall`](https://github.com/Non-Contradiction/JuliaCall).

## Related work

* R package [`Deriv`](https://cran.r-project.org/web/packages/numDeriv/index.html)
  for symbolic differentiation.
* R package [`numDeriv`](https://cran.r-project.org/web/packages/numDeriv/index.html)
  for numeric differentiation.
  It provides numerical gradients, Jacobians, and Hessians, computed by finite differences,
  Richardson extrapolation, and other numerical approximation methods.
* R package [`radx`](https://github.com/quantumelixir/radx/) for automatic differentiation,
  but it imposes restrictions on the functions that can be dealt with,
  and the functionality is not complete.

## Details of the coding project

TODO

## Expected impact

The project is expected to lead to an R wrapper for `Julia`'s AD packages
 `ForwardDiff.jl` and `ReverseDiff.jl`. It should be able to do both forward mode and
 backward mode AD for

* native R functions with little or no modification,
* `Julia` functions in R through the `JuliaCall` package,
* "typical" `Rcpp` functions with little modification,
* mixing of the three kinds of functions.

## Mentors

Students, please contact mentors below after completing at least one of the tests below.

MENTORS: fill in this part. each project needs 2 mentors. One should be an expert R programmer with previous package
development experience, and the other can be a domain expert in some other field or application area
(optimization, bioinformatics, machine learning, data viz, etc).
Ideally one of the two mentors should have previous experience with GSOC (either as a student or mentor).
Please provide contact info for each mentor, along with qualifications. Example:

* Toby Hocking <toby.hocking@r-project.org> is the author of R packages X and Y.
* Other Dev <other.dev@gmail.com> is an expert in machine learning,
  and has previous GSOC experience with NAME_OF_OPEN_SOURCE_ORGANIZATION in 2015-2016.

## Tests

Students, please do one or more of the following tests before contacting the mentors above.

* Use `Julia` packages `ForwardDiff` to get gradient and hessian
  functions for a simple `Julia` function `f(x::Vector) = sum(sin, x) + sum(sqrt, x);`,
  and evaluate the gradient and hessian functions on the vector `[1.0, 1.0, 1.0, 1.0, 1.0]`.
* Use R package `JuliaCall` to get the gradient and hessian functions for an R function
  `f <- function(x) {sum(sin(x)) + sum(sqrt(x))}` through `ForwardDiff`,
  and evaluate the gradient and hessian functions on the vector `rep(1, 5)`.
  This should be quite straightforward given `JuliaCall` already
  overloads R functions like `sum`, `sin` and `sqrt`.
* This project will also involve programming in `Rcpp`.
  You should be able to deal with `JuliaObject` from `JuliaCall`, which is an
  R6 object in `Rcpp`.
  For this test, write an `Rcpp` function to return the type string of a `JuliaObject`.

# Solutions of tests

Students, please post a link to your test results here.

Name: Changcheng Li
Email: cxl508@psu.edu, lch34677@gmail.com
Solution: https://github.com/Non-Contradiction/AD-R-GSOC/blob/master/Test.md
