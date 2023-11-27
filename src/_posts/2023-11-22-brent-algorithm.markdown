---
layout: post
title:  "Brent'ss Algorithm"
date:   2023-11-22 01:00:00 +0200
categories: algorithm
---

## Introduction  
Brent's algorithm, also known as Brent's method or the Brent–dekker method, is a root-finding algorithm that combines the bisection method, the secant method, and inverse quadratic interpolation. It is an iterative algorithm that can find the root of a continuous function in a given interval. The algorithm is widely used in numerical analysis, optimization, and computer science.  
   
## Implementation  
The following is an implementation of Brent's algorithm in OCaml. The function takes a function `f` and two endpoints `a` and `b` of an interval `[a, b]` as input, and returns an approximate root of `f` in the interval. The optional parameters `max_iter` and `xtol` are the maximum number of iterations and the tolerance for convergence, respectively.  
   
```ocaml  
let brent ?(max_iter = 1000) ?(xtol = 1e-6) f a b =
  let fa = f a in
  let fb = f b in
  let error () =
    let s = Printf.sprintf "f(a) *. f(b) = %g *. %g should be negative." fa fb in
    Owl_exception.INVALID_ARGUMENT s
  in
  Owl_exception.verify (fa *. fb < 0.) error;
  if fa = 0.
  then a
  else if fb = 0.
  then b
  else (
    let xa = ref a in
    let xb = ref b in
    let xc = ref b in
    let fc = ref fb in
    let fa = ref fa in
    let fb = ref fb in
    let d = ref infinity in
    let e = ref infinity in
    let p = ref infinity in
    let q = ref infinity in
    let r = ref infinity in
    let eps = 3e-16 in
    try
      for _i = 1 to max_iter do
        if (!fb > 0. && !fc > 0.) || (!fb < 0. && !fc < 0.)
        then (
          xc := !xa;
          fc := !fa;
          d := !xb -. !xa;
          e := !d);
        if abs_float !fc < abs_float !fb
        then (
          xa := !xb;
          xb := !xc;
          xc := !xa;
          fa := !fb;
          fb := !fc;
          fc := !fa);
        let tol = (2. *. eps *. abs_float !xb) +. (0.5 *. xtol) in
        let xm = 0.5 *. (!xc -. !xb) in
        assert (abs_float xm >= tol && !fb != 0.);
        (* 1st strategy: inverse quadratic interpolation *)
        if abs_float !e >= tol && abs_float !fa > abs_float !fb
        then (
          let s = !fb /. !fa in
          if !xa = !xc
          then (
            p := 2. *. xm *. s;
            q := 1. -. s)
          else (
            q := !fa /. !fc;
            r := !fb /. !fc;
            p := s *. ((2. *. xm *. !q *. (!q -. !r)) -. ((!xb -. !xa) *. (!r -. 1.)));
            q := (!q -. 1.) *. (!r -. 1.) *. (s -. 1.));
          if !p > 0. then q := -. !q;
          p := abs_float !p;
          let min1 = (3. *. xm *. !q) -. abs_float (tol *. !q) in
          let min2 = abs_float (!e *. !q) in
          if 2. *. !p < min min1 min2
          then (
            e := !d;
            d := !p /. !q)
          else (
            d := xm;
            e := !d)
          (* 2nd strategy: bisection method *))
        else (
          d := xm;
          e := !d);
        (* adjust the position *)
        xa := !xb;
        fa := !fb;
        if abs_float !d > tol
        then xb := !xb +. !d
        else xb := !xb +. if tol > 0. then xm else -.xm;
        fb := f !xb
      done;
      !xb
    with
    | _ -> !xb) 
```  
 
Here is an example of using the function to find the root of the function `f(x) = x^3 - 2x - 5` in the interval `[2, 3]`.  
 
```ocaml  
let f x = x ** 3. -. 2. *. x -. 5.  
 
let root = brent f 2. 3.  
 
let () = Printf.printf "The root of f(x) = x^3 - 2x - 5 in [2, 3] is %g.\n" root  
```  
 
The output should be:  
 
```  
The root of f(x) = x^3 - 2x - 5 in [2, 3] is 2.09455.  
```  
 
## Step-by-step Explanation  
Brent's algorithm is an iterative algorithm that combines the bisection method, the secant method, and inverse quadratic interpolation. The algorithm starts with two endpoints `a` and `b` of an interval `[a, b]` that contains a root of a continuous function `f(x)`. The algorithm then iteratively refines the estimate of the root until it converges to a desired level of accuracy.  
 
1. Initialize variables  

 Let `fa` and `fb` be the values of `f` at `a` and `b`, respectively. If `fa` or `fb` is zero, return the corresponding endpoint as the root. Otherwise, initialize the following variables:  

 - `xa`: the previous value of `xb`  
 - `xb`: the current estimate of the root  
 - `xc`: the previous value of `xa`  
 - `fa`: the value of `f` at `a`  
 - `fb`: the value of `f` at `b`  
 - `fc`: the value of `f` at `c`  
 - `d`: the step size  
 - `e`: the previous value of `d`  
 - `p`, `q`, `r`: variables used in inverse quadratic interpolation  
 - `eps`: a small number used for floating-point comparison  
 
2. Check for sign change  

 Verify that `fa` and `fb` have opposite signs. If not, raise an error.  
 
3. Iterate until convergence  

 Repeat the following steps until convergence or the maximum number of iterations is reached:  

 1. Check for convergence  

    Compute the tolerance `tol` for convergence. If `|f(b)|` is small enough or the step size `d` is smaller than `tol`, return `b` as the root.  

 2. Compute the step size  

    Compute the midpoint `xm` of the interval `[b, c]`, where `c` is the previous value of `a` or `b`. If `|e| >= tol` and `|f(a)| > |f(b)|`, use inverse quadratic interpolation to compute a new step size `d`. Otherwise, use bisection method to compute `d`.  

 3. Update the estimate of the root  

    Compute the new estimate of the root by adding `d` to `xb`. If `d` is too small, add or subtract `tol` instead.  
  
   4. Evaluate the function  
  
      Compute the value of `f` at the new estimate `xb`.  
  
   5. Update variables  
  
      Update the variables `xa`, `xb`, `xc`, `fa`, `fb`, `fc`, `d`, and `e` with their new values.  
  
   6. Check for convergence  
  
      If `|f(b)|` is small enough or the step size `d` is smaller than `tol`, return `b` as the root.  
  
   7. Check for oscillation  
  
      If the function value at the new estimate `xb` has the same sign as the function value at the previous estimate `xc`, replace `xc` and `fc` with `xa` and `fa`, and set `d` and `e` to their initial values.  
  
   8. Check for convergence  
  
      If `|f(b)|` is small enough or the step size `d` is smaller than `tol`, return `b` as the root.  
  
   9. Choose the interpolation method  
  
      If `|f(b)| < |f(a)|`, use secant method to compute a new step size `d`. Otherwise, use inverse quadratic interpolation.  
  
   10. Update the estimate of the root  
  
       Compute the new estimate of the root by adding `d` to `xb`. If `d` is too small, add or subtract `tol` instead.  
  
   11. Evaluate the function  
  
       Compute the value of `f` at the new estimate `xb`.  
  
   12. Update variables  
  
       Update the variables `xa`, `xb`, `xc`, `fa`, `fb`, `fc`, `d`, and `e` with their new values.  
   
4. Return the root  
  
   If the algorithm does not converge within the maximum number of iterations, return the last estimate of the root.  
   
## Complexity Analysis  
The time complexity of Brent's algorithm is O(log(tol/eps) * f_evals), where tol is the tolerance for convergence, eps is a small number used for floating-point comparison, and f_evals is the number of evaluations of the function `f` required by the algorithm. The algorithm is guaranteed to converge if the function `f` is continuous and has a root in the initial interval `[a, b]`. The algorithm is also robust to some types of singularities, such as poles and branch cuts. However, the algorithm may fail to converge if the function has multiple roots or if the initial interval is too large. In practice, Brent's algorithm is often more efficient than other root-finding algorithms, such as the bisection method and the secant method, especially for functions that are smooth but not necessarily analytic.