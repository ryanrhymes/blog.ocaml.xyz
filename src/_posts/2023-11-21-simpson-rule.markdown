---
layout: post
title:  "Simpson's Rule"
date:   2023-11-21 06:00:00 +0600
categories: algorithm
---

## Introduction  
The Simpson's rule is a numerical method used to approximate the definite integral of a function. It is a closed Newton-Cotes formula that uses quadratic polynomials to approximate the function on each subinterval of the partition. It is named after Thomas Simpson, an 18th-century mathematician who first described the method. The Simpson's rule is widely used in numerical analysis, physics, and engineering to solve problems that involve integration.  
   
## Implementation  
The following implementation is in OCaml:  
   
```ocaml  
let simpson ?(n = 20) ?(eps = 1e-6) f a b =
  let s_new = ref 0. in
  let s_old = ref 0. in
  let o_new = ref 0. in
  let o_old = ref 0. in
  (try
     for i = 1 to n do
       s_new := trapzd f a b i;
       s_old := ((4. *. !s_new) -. !o_new) /. 3.;
       if i > 5
       then (
         let d = abs_float (!s_old -. !o_old) in
         let e = eps *. abs_float !o_old in
         assert (not (d < e || (!s_old = 0. && !o_old = 0.)));
         o_old := !s_old;
         o_new := !s_new)
     done
   with
  | _ -> ());
  !s_new
```  
   
The function `simpson` takes in three arguments: a function `f` that returns a float, and two floats `a` and `b` representing the lower and upper bounds of the integral. It also has two optional parameters: `n` is the number of intervals used in the approximation, and `eps` is the maximum relative error allowed. The default values for `n` and `eps` are 20 and 1e-6, respectively.  
   
## Step-by-step Explanation 
1. The `simpson` function takes in three arguments: a function `f` that returns a float, and two floats `a` and `b` representing the lower and upper bounds of the integral. It also has two optional parameters: `n` is the number of intervals used in the approximation, and `eps` is the maximum relative error allowed. The default values for `n` and `eps` are 20 and 1e-6, respectively.  
   
2. The function initializes four variables: `s_new`, `s_old`, `o_new`, and `o_old`, all of which are set to 0. `s_new` and `s_old` will be used to store the current and previous approximations of the integral, respectively, while `o_new` and `o_old` will be used to store the current and previous approximations of the error, respectively.  
   
3. The function uses a `try`-`with` block to catch any exceptions that might be raised during the execution of the loop. This is done to ensure that the function always returns a value, even if an error occurs.  
   
4. The function then enters a `for` loop that iterates from 1 to `n`. For each iteration of the loop, the function computes the current approximation of the integral over the interval [a,b] using the `trapzd` function, and stores it in the `s_new` variable.  
   
5. The function then uses the current and previous approximations of the integral to compute a new approximation that is more accurate. This is done using the formula:  
   
```  
s_old = (4 * s_new - o_new) / 3  
```  
   
where `s_old` is the new approximation, `s_new` is the current approximation, and `o_new` is the previous approximation.  
   
6. The function then checks if the current iteration is greater than 5. If it is, the function checks if the relative error between the current and previous approximations is less than the maximum relative error allowed (`eps`). If it is, the function returns the current approximation of the integral (`s_new`). If it is not, the function updates the `o_old` and `o_new` variables to store the current and previous approximations of the error, respectively.  
   
7. The function exits the loop and returns the current approximation of the integral (`s_new`).  
   
8. If an exception is raised during the execution of the loop, the function catches it and returns the current approximation of the integral (`s_new`).  
   
Overall, the `simpson` function uses the Simpson's rule to approximate the integral of a function over an interval [a,b]. It does this by dividing the interval into `n` subintervals, approximating the function on each subinterval using a quadratic polynomial, and summing the integrals over each subinterval to obtain an approximation of the integral over the entire interval. The function uses the `trapzd` function to approximate the integral over each subinterval, and uses the current and previous approximations of the integral to compute a new approximation that is more accurate. The function also checks if the relative error between the current and previous approximations is less than the maximum relative error allowed, and returns the current approximation if it is.
  
## Complexity Analysis  
The time complexity of the `simpson` function depends on the number of intervals used in the approximation (`n`) and the maximum relative error allowed (`eps`).  
   
The `trapzd` function, which is used to approximate the integral over each subinterval, has a time complexity of O(n), where n is the number of intervals used in the approximation. This is because the function uses a `for` loop that iterates `n` times to compute the sum of the areas of the trapezoids.  
   
The `simpson` function uses the `trapzd` function `n` times to approximate the integral over each subinterval, and then uses the current and previous approximations of the integral to compute a new approximation that is more accurate. The function also checks if the relative error between the current and previous approximations is less than the maximum relative error allowed, and returns the current approximation if it is.  
   
The time complexity of the `simpson` function can be approximated as follows:  
   
- The `for` loop that iterates from 1 to `n` has a time complexity of O(n).  
- The `trapzd` function is called `n` times, so its time complexity is O(n^2).  
- The time complexity of the computations inside the loop is O(1).  
- The `if` statement that checks if the relative error is less than the maximum relative error allowed has a time complexity of O(1).  
- The `try`-`with` block has a time complexity of O(1).  
- The function returns the current approximation of the integral, which has a time complexity of O(1).  
   
Therefore, the overall time complexity of the `simpson` function is O(n^2). This means that the function takes longer to run as the number of intervals used in the approximation (`n`) increases. The maximum relative error allowed (`eps`) does not affect the time complexity of the function, but it does affect the accuracy of the approximation. A smaller value of `eps` will result in a more accurate approximation, but will also require more iterations of the `for` loop and hence increase the running time of the function.