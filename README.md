# Automatic Differentiation in R through Julia

## Background

Automatic differentiation (AD) is a set of techniques to calculate derivatives automatically.
It generally outperforms non-AD methods like symbolic differentiation and numerical approximation
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
`ForwardDiff.jl` and `ReverseDiff.jl` by the use of R packages which embeds `Julia`
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
* R package [`nlsr`](https://cran.r-project.org/web/packages/nlsr/index.html),
  which is a solver for nonlinear least squares problems with some symbolic derivative features.
* From Perry de Valpine of Berkeley, we have learned that there is development in the
  [Nimble project](https://r-nimble.org/) to incorporate AD via C++. Similar capability
  was in the AD Model Builder software, as described by Ben Bolker in
  <https://cran.r-project.org/web/packages/R2admb/vignettes/R2admb.pdf>.

## Details of the coding project
The project is going to wrap the [differentiation api of `ForwardDiff.jl`](http://www.juliadiff.org/ForwardDiff.jl/stable/user/api.html)
and the [api of `ReverseDiff.jl`](http://www.juliadiff.org/ReverseDiff.jl/api/) to do AD for native `R` functions and some `Rcpp` functions in an `R` package `ADjlr`, which means automatic differentiation in `R` through `Julia`.

The package `ADjlr` should provide an easy-to-use and well-documented user interface.
A good example is the package `numDeriv`, which is already widely used in optimization to provide derivative information like gradient by `numDeriv::grad`, hessian by `numDeriv::hessian` and jacobian by `numDeriv::jacobian`.
The usage of these three functions in `numDeriv` are consistent with each other,
so users can use the functions easily.
Another good example is the package [`optextras`]( https://CRAN.R-project.org/package=optextras), which provides a consistent and easy-to-use interface to apply and check derivatives for optimization.
The package `ADjlr` should provide an interface similar to other well-established packages to allow for easier usage, comparison and improvement of the tools.

### Testing
Since one of the most important goal of this project is to ensure the correctness of
automatic differentiation, so a comprehensive test set covering lots of possible functions
for automatic differentiation will be written for the project,
including native `R` functions, `Julia` functions
through `JuliaCall`, `Rcpp` functions.
In the tests, the automatic differentiation results for a (mostly) same function
in `Julia`, `R`, and `Rcpp`
will be checked against each other,
and will also be checked against the numerical
and symbolic differentiation results by packages like `numDeriv` and `Deriv` for correctness.

There should also be some benchmark tests, which checks the performance of the code
before/after a certain commit or pull request,
and compares the performance and with the numerical
and symbolic differentiation results by packages like `numDeriv` and `Deriv`.

Every important commit or pull request should pass the tests.
So CI systems will be configured, both on travis CI for mac os and linux, and appveyor for windows.

## Expected impact

The project is expected to lead to an R wrapper for `Julia`'s AD packages
 `ForwardDiff.jl` and `ReverseDiff.jl`. It should be able to do both forward mode and
 backward mode AD for

* native R functions with little or no modification,
* `Julia` functions in R through the `JuliaCall` package,
* "typical" `Rcpp` functions with little modification,
* mixing of the three kinds of functions.

## Mentors

John Nash is retired Professor of Management in the Telfer School of Management, University of Ottawa. He has worked for approximately half a century in numerical methods for linear algebra and function minimization, in particular with R. See 'optim()' and packages such as optimx, Rvmmin, Rtnmin, Rcgmin, lbfgsb3 and nlsr. However, he also has been active in trying to simplify and unify access to the tools R makes available, that is, the navigation of the package space.

[Hans Werner Borchers](https://github.com/hwborchers) has experience with `Julia` and `JuliaCall`.
There is also some notes on using `Julia` and `JuliaCall` 
on his [github page](https://hwborchers.github.io/).

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

## Solutions of tests

Students, please post a link to your test results here.

Name: Changcheng Li

Email: cxl508@psu.edu, lch34677@gmail.com

Solution: <https://github.com/Non-Contradiction/AD-R-GSOC/blob/master/Test.md>
