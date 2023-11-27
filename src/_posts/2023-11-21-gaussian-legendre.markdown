---
layout: post
title:  "Gauss-Legendre Algorithm"
date:   2023-11-21 12:00:00 +0200
categories: algorithm
---

## Introduction  
The Gauss-Legendre algorithm is an iterative numerical integration technique used for approximating definite integrals. The algorithm is based on the idea of interpolating a function over a given interval using Legendre polynomials. The resulting approximation is then used to compute the integral of the function over the same interval. The Gauss-Legendre algorithm is widely used in numerical analysis and scientific computing.  
   
## Implementation  

```ocaml
let gauss_legendre ?(eps = 3e-11) ?(a = -1.) ?(b = 1.) n =
  let m = (n + 1) / 2 in
  let n' = float_of_int n in
  let x = Array.create_float n in
  let w = Array.create_float n in
  let xm = 0.5 *. (b +. a) in
  let xl = 0.5 *. (b -. a) in
  let p1 = ref infinity in
  let p2 = ref infinity in
  let p3 = ref infinity in
  let pp = ref infinity in
  let z = ref infinity in
  for i = 1 to m do
    let i' = float_of_int i in
    z := cos (Owl_const.pi *. (i' -. 0.25) /. (n' +. 0.5));
    (try
       while true do
         p1 := 1.;
         p2 := 0.;
         for j = 1 to n do
           p3 := !p2;
           p2 := !p1;
           let j' = float_of_int j in
           p1 := ((((2. *. j') -. 1.) *. !z *. !p2) -. ((j' -. 1.) *. !p3)) /. j'
         done;
         pp := n' *. ((!z *. !p1) -. !p2) /. ((!z *. !z) -. 1.);
         let z1 = !z in
         z := z1 -. (!p1 /. !pp);
         assert (abs_float (!z -. z1) > eps)
       done
     with
    | _ -> ());
    x.(i - 1) <- xm -. (xl *. !z);
    x.(n - i) <- xm +. (xl *. !z);
    w.(i - 1) <- 2. *. xl /. ((1. -. (!z *. !z)) *. !pp *. !pp);
    w.(n - i) <- w.(i - 1)
  done;
  x, w


let gauss_legendre_cache = Array.init 50 gauss_legendre

let _gauss_laguerre ?(_eps = 3e-11) _a _b _n = ()

let gaussian_fixed ?(n = 10) f a b =
  let x, w =
    match n < Array.length gauss_legendre_cache with
    | true  -> gauss_legendre_cache.(n)
    | false -> gauss_legendre n
  in
  let xr = 0.5 *. (b -. a) in
  let s = ref 0. in
  for i = 0 to n - 1 do
    let c = (xr *. (x.(i) +. 1.)) +. a in
    s := !s +. (w.(i) *. f c)
  done;
  !s *. xr


let gaussian ?(n = 50) ?(eps = 1e-6) f a b =
  let s_new = ref infinity in
  let s_old = ref infinity in
  (try
     for i = 1 to n do
       s_new := gaussian_fixed ~n:i f a b;
       assert (abs_float (!s_new -. !s_old) > eps);
       s_old := !s_new
     done
   with
  | _ -> ());
  !s_new
```

The Gauss-Legendre algorithm is implemented in OCaml as a function `gaussian` which takes in the following arguments:  
- `n`: the number of iterations to perform (default 50)  
- `eps`: the desired accuracy of the approximation (default 1e-6)  
- `f`: the function to integrate  
- `a`: the lower bound of the integration interval  
- `b`: the upper bound of the integration interval  
   
The implementation is split into two functions: `gaussian_fixed` and `gaussian`. The former performs a single iteration of the algorithm, while the latter performs a specified number of iterations until the desired accuracy is achieved. The `gaussian` function first checks if the desired number of iterations is available in a precomputed cache, otherwise it computes them using the `gauss_legendre` function.  
   
## Step-by-step Explanation  
The Gauss-Legendre algorithm works by approximating the integral of a function `f` over an interval `[a,b]` using Legendre polynomials. The algorithm proceeds as follows:  
1. Compute the Legendre-Gauss nodes and weights for the given interval `[a,b]` and number of nodes `n`.  
2. Transform the integral over `[a,b]` into an integral over `[-1,1]` using the change of variables `x = ((b-a)t + b + a)/2`.  
3. Approximate the integral over `[-1,1]` using the Legendre-Gauss nodes and weights.  
4. Transform the approximation back to the integral over `[a,b]` using the same change of variables as in step 2.  
   
The Legendre-Gauss nodes and weights are computed using the `gauss_legendre` function. This function uses the iterative method described by the algorithm to approximate the roots of the Legendre polynomial of degree `n`. The roots are then used as the nodes for the Gauss-Legendre quadrature, and the corresponding weights are computed using the formula `w_i = 2 / [(1 - z_i^2) * (P_n'(z_i))^2]`, where `P_n` is the Legendre polynomial of degree `n` and `z_i` is the `i`-th root.  
   
The `gaussian_fixed` function approximates the integral over `[-1,1]` using the Legendre-Gauss nodes and weights. It first computes the midpoint `xr` of the interval `[-1,1]` and then evaluates the function `f` at each of the nodes `x_i` transformed to the interval `[a,b]` using the change of variables described above. The approximation is then computed as the sum of the products of the weights `w_i` and the function values `f(x_i)`.  
   
The `gaussian` function iteratively calls `gaussian_fixed` with increasing numbers of nodes until the desired accuracy is achieved. It uses the precomputed cache of Legendre-Gauss nodes and weights if available, otherwise it computes them using the `gauss_legendre` function.  
   
## Complexity Analysis  
The time complexity of the Gauss-Legendre algorithm is dominated by the number of iterations `n` performed by the `gaussian` function. Each iteration of `gaussian` requires evaluating the function `f` at `n` points and computing the Legendre-Gauss nodes and weights. The latter is precomputed and cached for up to 50 nodes, and computed on-the-fly using the `gauss_legendre` function for larger values of `n`.   
  
The time complexity of computing the Legendre-Gauss nodes and weights using `gauss_legendre` is `O(n^2)`, due to the nested loop over `j` and `i`. However, this function is only called once for each unique `n`, so its time complexity is not a bottleneck for the overall algorithm.  
   
The time complexity of evaluating the function `f` at `n` points is dependent on the implementation of `f`. Assuming that `f` has a time complexity of `O(1)` for each evaluation, the time complexity of evaluating `f` at `n` points is `O(n)`.   
  
Therefore, the overall time complexity of the Gauss-Legendre algorithm is `O(n^3)` for large values of `n`. The space complexity is `O(n)` for storing the Legendre-Gauss nodes and weights.