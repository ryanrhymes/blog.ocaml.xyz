---
layout: post
title:  "Polynomial Interpolation"
date:   2023-11-21 05:00:00 +0200
categories: algorithm
---

## Introduction  
   
Polynomial interpolation is a mathematical technique used to construct a polynomial function that passes through a given set of points. Given a set of n points (x_0, y_0), (x_1, y_1), ..., (x_n-1, y_n-1), polynomial interpolation finds a unique polynomial of degree n-1 or less that passes through these points. This technique is widely used in various fields such as computer graphics, numerical analysis, and signal processing.  
   
## Implementation  
   
The following code is an implementation of the polynomial interpolation algorithm in OCaml from Owl library. The function `polint` takes three arguments - `xs`, `ys`, and `x`. The arrays `xs` and `ys` represent the x-coordinates and y-coordinates of the n points, respectively. The variable `x` represents the point at which we want to evaluate the polynomial. The function returns a tuple of two values - the interpolated value of the polynomial at `x` and the error estimate.  

```ocaml
let polint xs ys x =
  let n = Array.length xs in
  let m = Array.length ys in
  let error () =
    let s =
      Printf.sprintf
        "polint requires that xs and ys have the same length, but xs length is %i \
         whereas ys length is %i"
        n
        m
    in
    Owl_exception.INVALID_ARGUMENT s
  in
  Owl_exception.verify (m = n) error;
  let c = Array.copy ys in
  let d = Array.copy ys in
  let ns = ref 0 in
  let dy = ref 0. in
  let dif = ref (abs_float (x -. xs.(0))) in
  for i = 0 to n - 1 do
    let dift = abs_float (x -. xs.(i)) in
    if dift < !dif
    then (
      ns := i;
      dif := dift)
  done;
  let y = ref ys.(!ns) in
  ns := !ns - 1;
  for m = 0 to n - 2 do
    for i = 0 to n - m - 2 do
      let ho = xs.(i) -. x in
      let hp = xs.(i + m + 1) -. x in
      let w = c.(i + 1) -. d.(i) in
      let den = ho -. hp in
      assert (den <> 0.);
      let den = w /. den in
      c.(i) <- ho *. den;
      d.(i) <- hp *. den
    done;
    if (2 * !ns) + 2 < n - m - 1
    then dy := c.(!ns + 1)
    else (
      dy := d.(!ns);
      ns := !ns - 1);
    y := !y +. !dy
  done;
  !y, !dy
```

## Step-by-step Explanation  
   
1. The function first checks if the lengths of the arrays `xs` and `ys` are the same. If they are not, an exception is raised.  
   
2. Two arrays `c` and `d` are created, which are initialized to the values in the array `ys`.  
   
3. The variable `ns` is initialized to 0, which will be used to keep track of the index of the closest point to `x`.  
   
4. The variable `dif` is initialized to the absolute difference between `x` and the first point in the array `xs`.  
   
5. A loop is executed over all the points in the array `xs`. For each point, the absolute difference between `x` and the point is computed. If this difference is less than the current value of `dif`, then the index of the point is stored in the variable `ns`, and the value of `dif` is updated to the new difference.  
   
6. The value of `y` is initialized to the y-coordinate of the point in the array `ys` that is closest to `x`.  
   
7. The variable `ns` is decremented by 1.  
   
8. A loop is executed n-1 times. In each iteration, two nested loops are executed over the remaining points in the array `xs`. The variable `m` is used to keep track of the number of iterations.  
   
9. In the inner loop, the variable `ho` is initialized to the difference between the current point and `x`, and the variable `hp` is initialized to the difference between the point m positions ahead and `x`.  
   
10. The variable `w` is initialized to the difference between the y-coordinates of the point m positions ahead and the current point.  
   
11. The variable `den` is initialized to the difference between `ho` and `hp`. If `den` is 0, an exception is raised.  
   
12. The variable `den` is updated to `w/den`.  
   
13. The value of `c` at the current index is updated to `ho*den`, and the value of `d` at the current index is updated to `hp*den`.  
   
14. If the index of the closest point to `x` is less than `n-m-1` multiplied by 2, then the value of `dy` is updated to `c[ns+1]`. Otherwise, the value of `dy` is updated to `d[ns]`, and the index of the closest point to `x` is decremented by 1.  
   
15. The value of `y` is updated to `y+dy`.  
   
16. The function returns the tuple `(y, dy)`.  
   
## Complexity Analysis  
   
The polynomial interpolation algorithm has a time complexity of O(n^2), where n is the number of points. This is because the algorithm involves a nested loop over all the points in the input arrays. In the outer loop, the algorithm iterates n-1 times, and in the inner loop, it iterates over the remaining n-m-1 points. Therefore, the total number of iterations is given by the sum of the first n-1 positive integers, which is n(n-1)/2. Thus, the time complexity of the algorithm is O(n^2).  
   
In terms of space complexity, the algorithm uses two arrays of size n to store the y-coordinates and two additional variables to store the interpolated value and the error estimate. Therefore, the space complexity of the algorithm is O(n).