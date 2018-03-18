---
title: "Test for Automatic Differentiation in R through Julia"
author: "Changcheng Li"
output:
  html_document:
    keep_md: true
---



## Test 1


```julia
using ForwardDiff

f(x::Vector) = sum(sin, x) + sum(sqrt, x)
```

```
## f (generic function with 1 method)
```

```julia
g = x -> ForwardDiff.gradient(f, x)
```

```
## (::#1) (generic function with 1 method)
```

```julia
h = x -> ForwardDiff.hessian(f, x)
```

```
## (::#3) (generic function with 1 method)
```

```julia

g([1.,1.,1.,1.,1.])
```

```
## 5-element Array{Float64,1}:
##  1.0403
##  1.0403
##  1.0403
##  1.0403
##  1.0403
```

```julia
h([1.,1.,1.,1.,1.])
```

```
## 5×5 Array{Float64,2}:
##  -1.09147   0.0       0.0       0.0       0.0    
##   0.0      -1.09147   0.0       0.0       0.0    
##   0.0       0.0      -1.09147   0.0       0.0    
##   0.0       0.0       0.0      -1.09147   0.0    
##   0.0       0.0       0.0       0.0      -1.09147
```

## Test 2


```r
library(JuliaCall)
julia_setup()
julia_library("ForwardDiff")

f <- function(x) {sum(sin(x)) + sum(sqrt(x))}
g <- function(x) julia_call("ForwardDiff.gradient", f, x)
h <- function(x) julia_call("ForwardDiff.hessian", f, x)

g(rep(1, 5))
```

```
## [1] 1.040302 1.040302 1.040302 1.040302 1.040302
```

```r
h(rep(1, 5))
```

```
##           [,1]      [,2]      [,3]      [,4]      [,5]
## [1,] -1.091471  0.000000  0.000000  0.000000  0.000000
## [2,]  0.000000 -1.091471  0.000000  0.000000  0.000000
## [3,]  0.000000  0.000000 -1.091471  0.000000  0.000000
## [4,]  0.000000  0.000000  0.000000 -1.091471  0.000000
## [5,]  0.000000  0.000000  0.000000  0.000000 -1.091471
```

## Test 3

```r
julia_command("function julia_type(x) string(typeof(x)) end")
```

```
## julia_type (generic function with 1 method)
```

```cpp
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
String julia_type(Environment x) {
    if (!x.inherits("JuliaObject")) stop("Input must be a JuliaObject.");
    Environment julia("package:JuliaCall"); Function jcall = julia["julia_call"];
    return jcall("julia_type", x);
}
```


```r
julia_type(JuliaObject(1L))
```

```
## [1] "Int64"
```

```r
julia_type(JuliaObject(1))
```

```
## [1] "Float64"
```

```r
julia_type(JuliaObject(1:10))
```

```
## [1] "Array{Int64,1}"
```

```r
julia_type(JuliaObject(rnorm(10)))
```

```
## [1] "Array{Float64,1}"
```

## Some Additional Results

In this additional test, I expand the result in the above test to make a simple `Rcpp` class as a wrapper for `JuliaObject`, I define some conversion function following the vignette of ["Rcpp Extending"](https://cran.r-project.org/web/packages/Rcpp/vignettes/Rcpp-extending.pdf).
And I create an overloaded function `sqrt` for `JuliaObject` in `Rcpp`.
The `Rcpp` function calls the `R` function to accomplish the overloading currently.
A function `fn` is defined based on the overloaded `sqrt`.


```cpp
#include <RcppCommon.h>

class JuliaObject{
public:
    SEXP x;
    JuliaObject();
    JuliaObject(SEXP);
    operator SEXP();
};

#include <Rcpp.h>
using namespace Rcpp;

JuliaObject::JuliaObject(SEXP x1){
    if (!Environment(x1).inherits("JuliaObject")) stop("Input must be a JuliaObject.");
    x = x1;
};

SEXP wrap(JuliaObject j){
    return SEXP(j.x);
}

// [[Rcpp::export]]
String julia_type(JuliaObject x) {
    Environment julia("package:JuliaCall"); Function jcall = julia["julia_call"];
    return jcall("julia_type", x);
}

JuliaObject sqrt(JuliaObject j){
    Environment julia("package:JuliaCall"); Function jcall = julia["julia_call"];
    return jcall("JuliaCall.JuliaObject", jcall("sqrt", j));
}

NumericVector _fn(NumericVector x){
    return sqrt(x);
}

JuliaObject _fn(JuliaObject x){
    return sqrt(x);
}

// [[Rcpp::export]]
SEXP fn(SEXP x){
    if (TYPEOF(x) == ENVSXP){
        JuliaObject tmp = JuliaObject(x);
        return wrap(_fn(tmp));
    }
    else if(TYPEOF(x) == REALSXP){
            NumericVector tmp = NumericVector(x);
            return wrap(_fn(tmp));
            }
    else{
        stop("Input is not supported.");
    }
}
```


```r
fn(42)
```

```
## [1] 6.480741
```

```r
fn(JuliaObject(42))
```

```
## Julia Object of type Float64.
```
