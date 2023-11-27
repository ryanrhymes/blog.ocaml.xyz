---
layout: post
title:  "Rational Interpolation"
date:   2023-11-21 05:00:00 +0200
categories: algorithm
---

## Introduction  
Rational interpolation is a numerical method used to approximate a function using a rational function. The algorithm is used in various fields including engineering, physics, and finance.  

## Implementation  
The `ratint` algorithm is implemented in OCaml as shown below. The function takes three arguments: `xs`, `ys`, and `x`. `xs` is an array of length `n` representing the x-coordinates of the function, `ys` is an array of length `n` representing the y-coordinates of the function, and `x` is the point at which the function is to be approximated. The function returns a tuple containing the approximation of the function at `x` and the derivative of the approximation at `x`.  
   
```ocaml  
let ratint xs ys x =  
  let n = Array.length xs in  
  let m = Array.length ys in  
  let error () =  
    let s =  
      Printf.sprintf  
        "ratint requires that xs and ys have the same length, but xs length is %i \  
         whereas ys length is %i"  
        n  
        m  
    in  
    Owl_exception.INVALID_ARGUMENT s  
  in  
  Owl_exception.verify (m = n) error;  
  let c = Array.copy ys in  
  let d = Array.copy ys in  
  let hh = ref (abs_float (x -. xs.(0))) in  
  let y = ref 0. in  
  let dy = ref 0. in  
  let ns = ref 0 in  
  let eps = 1e-25 in  
  try  
    for i = 0 to n do  
      let h = abs_float (x -. xs.(i)) in  
      if h = 0.  
      then (  
        y := ys.(i);  
        dy := 0.;  
        raise Owl_exception.FOUND)  
      else if h < !hh  
      then (  
        ns := i;  
        hh := h;  
        c.(i) <- ys.(i);  
        d.(i) <- ys.(i) +. eps)  
    done;  
    y := ys.(!ns);  
    ns := !ns - 1;  
    for m = 1 to n - 1 do  
      for i = 1 to n - m do  
        let w = c.(i) -. d.(i - 1) in  
        let h = xs.(i + m - 1) -. x in  
        let t = (xs.(i - 1) -. x) *. d.(i - 1) /. h in  
        let dd = t -. c.(i) in  
        if dd = 0. then failwith "Has a pole";  
        let dd = w /. dd in  
        d.(i - 1) <- c.(i) *. dd;  
        c.(i - 1) <- t *. dd  
      done  
    done;  
    !y, !dy  
  with  
  | Owl_exception.FOUND -> !y, !dy  
  | e                   -> raise e  
```  
   
1. The `ratint` function takes three arguments: `xs`, `ys`, and `x`.  
   
2. The length of the input arrays `xs` and `ys` are obtained and verified to be equal. If they are not equal, an error is thrown.  
   
3. Two arrays `c` and `d` are created as copies of the `ys` array.  
   
4. A variable `hh` is initialized to the absolute difference between `x` and the first element of `xs`. This will be used to keep track of the smallest distance between `x` and an element of `xs`.  
   
5. Three variables `y`, `dy`, and `ns` are initialized to 0. `y` will be used to store the approximation of the function at `x`, `dy` will be used to store the derivative of the approximation at `x`, and `ns` will be used to keep track of the index of the closest element of `xs` to `x`.  
   
6. A variable `eps` is initialized to a small number. This will be used to prevent division by zero.  
   
7. A for loop is used to iterate over the `xs` array.  
   
8. For each `xs[i]`, the absolute difference between `x` and `xs[i]` is computed and stored in a variable `h`.  
   
9. If `h` is equal to 0, then `ys[i]` is the exact value of the function at `x`, so `y` is set to `ys[i]`, `dy` is set to 0, and an exception is raised to exit the loop.  
   
10. If `h` is less than `hh`, then `ns` is set to `i`, `hh` is set to `h`, and the `c[i]` and `d[i]` arrays are updated to `ys[i]`. This keeps track of the closest element of `xs` to `x`.  
   
11. After the loop, `y` is set to `ys[ns]`, and `ns` is decremented by 1.  
   
12. Two nested for loops are used to iterate over the `c` and `d` arrays.  
   
13. For each iteration, `m` represents the current iteration of the outer loop, and `i` represents the current iteration of the inner loop.  
   
14. A variable `w` is computed as the difference between `c[i]` and `d[i-1]`.  
   
15. A variable `h` is computed as the difference between `xs[i+m-1]` and `x`.  
   
16. A variable `t` is computed as `(xs[i-1] - x) * d[i-1] / h`.  
   
17. A variable `dd` is computed as `t - c[i]`.  
   
18. If `dd` is equal to 0, then the function has a pole, and an exception is thrown.  
   
19. A variable `dd` is computed as `w / dd`.  
   
20. The `d[i-1]` array is updated to `c[i] * dd`, and the `c[i-1]` array is updated to `t * dd`.  
   
21. After the loops, `y` and `dy` are returned as the approximation and derivative of the function at `x`.  
   
22. If any exceptions are raised during the computation, they are rethrown.   
  
## Complexity Analysis  
The `ratint` algorithm has a time complexity of O(n^2), where n is the length of the input arrays `xs` and `ys`.   
  
This is because the algorithm has two nested loops, each iterating over the `c` and `d` arrays. The outer loop iterates `n-1` times, and the inner loop iterates `n-m` times, where `m` is the current iteration of the outer loop. This gives a total of `(n-1) + (n-2) + ... + 1` iterations for the outer loop, which is equal to `n(n-1)/2`. The inner loop has a similar number of iterations, giving a total of `n(n-1)^2/2` iterations for both loops combined.  
   
Therefore, the time complexity of the algorithm is O(n^2). This means that as the length of the input arrays `xs` and `ys` increases, the time taken by the algorithm to compute the approximation of the function at `x` and its derivative also increases quadratically.