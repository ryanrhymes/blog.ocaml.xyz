---
layout: post
title:  "Bisection Algorithm"
date:   2023-11-21 04:00:00 +0200
categories: algorithm
---

## Introduction  
   
The bisection algorithm is a root-finding algorithm that repeatedly bisects an interval and then selects a subinterval in which a root must lie for further processing. It is a simple and robust method that is guaranteed to converge to a root if the function is continuous and changes sign exactly once over the interval.  
   
## Implementation  
   
The implementation below is in OCaml. It takes a function f, and two points a and b, such that f(a) and f(b) have opposite signs, indicating that there is a root in between the two points. The algorithm then bisects the interval and checks the sign of the function at the midpoint. If the sign is negative, the root must be in the left subinterval, and if the sign is positive, the root must be in the right subinterval. The algorithm repeats this process until it converges to a root.  
   
```ocaml  
let bisec ?(max_iter = 1000) ?(xtol = 1e-6) f a b =
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
    let x, d =
      match fa < 0. with
      | true  -> ref a, ref (b -. a)
      | false -> ref b, ref (a -. b)
    in
    try
      for _i = 1 to max_iter do
        d := !d *. 0.5;
        let c = !x +. !d in
        let fc = f c in
        if fc <= 0. then x := c;
        assert (abs_float !d >= xtol && fc != 0.)
      done;
      !x
    with
    | _ -> !x)
```  
   
## Step-by-step Explanation  
   
1. The function takes a function f, and two points a and b, such that f(a) and f(b) have opposite signs, indicating that there is a root in between the two points.  
   
2. The function checks that f(a) and f(b) have opposite signs. If they do not, the function raises an error.  
   
3. If f(a) is 0, the function returns a, since a is already a root.  
   
4. If f(b) is 0, the function returns b, since b is already a root.  
   
5. The function initializes x to a or b, depending on whether f(a) is negative or positive, and initializes d to the distance between a and b.  
   
6. The function enters a loop that runs for a maximum of max_iter iterations.  
   
7. In each iteration, the function bisects the interval by setting d to d/2.  
   
8. The function computes the midpoint c of the interval by adding d to x.  
   
9. The function evaluates the function at c.  
   
10. If the sign of the function at c is negative, the function sets x to c, indicating that the root must be in the left subinterval.  
   
11. The function checks that the distance between the endpoints of the interval is greater than xtol, and that the function value at c is not 0. If either of these conditions is false, the function exits the loop.  
   
12. If the loop completes without finding a root, the function returns the last value of x.  
   
## Complexity Analysis  
   
The bisection algorithm has a guaranteed convergence rate of O(log2((b-a)/xtol)), where a and b are the endpoints of the interval and xtol is the desired tolerance. This means that the number of iterations required to find a root decreases by a factor of 2 with each doubling of the number of correct digits.   
  
The algorithm's time complexity is O(max_iter), where max_iter is the maximum number of iterations allowed. In practice, the algorithm usually converges much faster than this worst-case bound, and the number of iterations required depends on the function being evaluated and the desired accuracy.   
  
The space complexity of the algorithm is O(1), as it only requires a constant amount of memory to store the variables used in the algorithm.   
  
Overall, the bisection algorithm is a simple and robust method for finding roots of continuous functions that is guaranteed to converge to a root if the function changes sign exactly once over the interval. Its time complexity depends on the maximum number of iterations allowed and the convergence rate, while its space complexity is constant.