---
layout: post
title:  "Ridder's Algorithm"
date:   2023-11-22 02:00:00 +0200
categories: algorithm
---

## Introduction  
   
Ridder's algorithm is a root-finding algorithm that helps to find the root of a given function. This algorithm is an improvement over the bisection method and provides a faster convergence rate. The algorithm is named after Derek Ridder, who introduced it in 1978. It is widely used in scientific and engineering applications.  
   
## Implementation  
   
The algorithm is implemented in OCaml as follows:  
   
```ocaml
let ridder ?(max_iter = 1000) ?(xtol = 1e-6) f a b =
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
    let fa = ref fa in
    let fb = ref fb in
    let x = ref infinity in
    try
      for _i = 1 to max_iter do
        let dm = 0.5 *. (!xb -. !xa) in
        let xm = !xa +. dm in
        let fm = f xm in
        let s = sqrt ((fm *. fm) -. (!fa *. !fb)) in
        assert (s <> 0.);
        let sgn = if !fa < !fb then -1. else 1. in
        x := xm +. (sgn *. dm *. fm /. s);
        let fn = f !x in
        if Owl_base_maths.same_sign fn fm = false
        then (
          xa := !x;
          xb := xm;
          fa := fn;
          fb := fm)
        else if Owl_base_maths.same_sign fn !fa = false
        then (
          xb := !x;
          fb := fn)
        else (
          xa := !x;
          fa := fn);
        assert (abs_float (!xb -. !xa) >= xtol && fn != 0.)
      done;
      !x
    with
    | _ -> !x)
```  
   
The function takes three arguments: the function `f` whose root needs to be found, and the interval `[a, b]` within which to search for the root. The function returns the estimated value of the root.  
   
## Step-by-step Explanation 
1. The function `ridder` takes three input parameters: the function `f` whose root needs to be found, and the interval `[a, b]` within which to search for the root. It also has two optional parameters: `max_iter` (maximum number of iterations) and `xtol` (tolerance level). The function returns the estimated value of the root.  
   
2. The function computes the value of `f` at the two endpoints `a` and `b`, and stores them in the variables `fa` and `fb`, respectively.  
   
3. The function checks if the product of `fa` and `fb` is negative. If it is not, it raises an error since the algorithm only works if the function changes sign over the interval `[a, b]`.  
   
4. The function checks if either `fa` or `fb` is zero. If either of them is zero, the function returns the corresponding value of `a` or `b` since the root is already found.  
   
5. The function initializes the variables `xa`, `xb`, `fa`, and `fb` with the values of `a`, `b`, `fa`, and `fb`, respectively. It also initializes the variable `x` with a value of infinity.  
   
6. The function enters a loop that runs for a maximum of `max_iter` iterations. This is to prevent the function from running indefinitely in case the algorithm fails to converge.  
   
7. Within the loop, the function computes the midpoint `xm` of the interval `[xa, xb]`.  
   
8. The function computes the value of `f` at `xm` and stores it in the variable `fm`.  
   
9. The function computes the value of `s`, which is the square root of `(fm^2 - fa*fb)`. This value is used to determine the new estimate for the root.  
   
10. The function checks if `s` is zero. If it is, the algorithm cannot continue since it would require division by zero. In this case, the function raises an error.  
   
11. The function computes the sign of `fa` and `fb` and stores it in the variable `sgn`. If `fa < fb`, `sgn` is set to `-1`, otherwise it is set to `1`.  
   
12. The function computes the new estimate for the root using the formula `x = xm + (sgn * dm * fm / s)`, where `dm` is half the distance between `xa` and `xb`.  
   
13. The function computes the value of `f` at the new estimate `x` and stores it in the variable `fn`.  
   
14. The function checks if `fn` and `fm` have opposite signs. If they do, the root is between `xm` and `x`, so the interval is updated to `[xa, x]` and `fa` and `fb` are updated accordingly. If they don't, the root is between `xa` and `x`, so the interval is updated to `[x, xb]` and `fa` and `fb` are also updated accordingly.  
   
15. The function checks if `fn` and `fa` have opposite signs. If they do, the root is between `xa` and `x`, so the interval is updated to `[xa, x]` and `fa` is updated to `fn`. If they don't, the root is between `x` and `xb`, so the interval is updated to `[x, xb]` and `fb` is updated to `fn`.  
   
16. The function checks if the absolute difference between `xa` and `xb` is less than `xtol`. If it is, the algorithm has converged and the function returns the estimated value of the root.  
   
17. If the loop has completed `max_iter` iterations without converging, the function raises an error.  
   
18. If an error occurs during the loop, the function returns the current estimate of the root.  
   
Overall, the algorithm works by iteratively refining the interval `[xa, xb]` until the root is found within a certain tolerance level. At each iteration, the algorithm computes the midpoint `xm` of the interval and the value of `f` at `xm`. It then uses these values to compute a new estimate for the root using the Ridder's formula. The algorithm then updates the interval and the values of `fa` and `fb` based on whether the new estimate is closer to `xa` or `xb`. The algorithm repeats this process until the root is found or the maximum number of iterations is reached.  
   
## Complexity Analysis  
   
The time complexity of the Ridder's algorithm is `O(log2(1/ε))`, where `ε` is the tolerance level. This means that the algorithm converges exponentially fast as the tolerance level is decreased. The algorithm also has a space complexity of `O(1)` since it only uses a fixed number of variables regardless of the size of the input.  
   
However, it is important to note that the algorithm may not converge if the function is not well-behaved or if the interval `[a, b]` is not chosen properly. In such cases, the algorithm may require more iterations or fail to converge altogether. Therefore, it is important to carefully choose the interval and verify that the function changes sign over the interval before using the algorithm.