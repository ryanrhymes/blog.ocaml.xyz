---
layout: post
title:  "Tridiagonal Matrix Algorithm"
date:   2023-11-22 05:00:00 +0200
categories: algorithm
---

## Introduction  
The tridiagonal matrix algorithm is a numerical method used to solve a system of linear equations where the matrix is tridiagonal. A tridiagonal matrix is a matrix where all the elements are zero except for those on the main diagonal, the diagonal above it, and the diagonal below it. The algorithm is used in various fields such as physics, engineering, and finance.  
   
## Implementation 

```ocaml
(* Solver for tridiagonal matrix
 * Input: a[n], b[n], c[n], which together consist the tridiagonal matrix A, and the right side vector r[n]. Return: x[n].
 *)

let tridiag_solve_vec a b c r =
  let n = Array.length a in
  let n1 = Array.length b in
  let n2 = Array.length c in
  assert (n = n1 && n = n2);
  if b.(0) = 0.
  then raise (Invalid_argument "tridiag_solve_vec: 0 at the beginning of diagonal vector");
  let bet = ref b.(0) in
  let gam = Array.make n 0. in
  let x = Array.make n 0. in
  x.(0) <- r.(0) /. !bet;
  for j = 1 to n - 1 do
    gam.(j) <- c.(j - 1) /. !bet;
    bet := b.(j) -. (a.(j) *. gam.(j));
    if !bet = 0. then raise (Invalid_argument "tridiag_solve_vec: algorithm fails");
    x.(j) <- (r.(j) -. (a.(j) *. x.(j - 1))) /. !bet
  done;
  for j = n - 2 downto 0 do
    x.(j) <- x.(j) -. (gam.(j + 1) *. x.(j + 1))
  done;
  x
```

The implementation of the tridiagonal matrix algorithm is provided in OCaml. The function `tridiag_solve_vec` takes four input arrays `a`, `b`, `c`, and `r`, where `a`, `b`, and `c` are the arrays representing the tridiagonal matrix and `r` is the right-hand side vector. The function returns the solution vector `x`.   
  
## Step-by-step Explanation  
The tridiagonal matrix algorithm works by eliminating the coefficients below and above the main diagonal. The algorithm can be divided into three steps:  
   
1. Decomposition: In this step, the algorithm decomposes the matrix into two matrices, L and U, where L is a lower triangular matrix and U is an upper triangular matrix. The decomposition is done in such a way that the product of L and U is equal to the original matrix.  
   
2. Forward substitution: In this step, the algorithm solves the equation Ly = r, where L is the lower triangular matrix obtained in the previous step, y is an intermediate vector, and r is the right-hand side vector. This step is called forward substitution because it starts from the first equation and solves for y in terms of the previous y values.  
   
3. Backward substitution: In this step, the algorithm solves the equation Ux = y, where U is the upper triangular matrix obtained in the first step, x is the solution vector, and y is the intermediate vector obtained in the previous step. This step is called backward substitution because it starts from the last equation and solves for x in terms of the previous x values.  
   
The implementation of the tridiagonal matrix algorithm provided in OCaml is based on the Thomas algorithm, which is a simplified version of the tridiagonal matrix algorithm. The algorithm starts by checking that the diagonal element of the matrix is not zero. If it is zero, the algorithm raises an exception.   
  
The algorithm then proceeds to calculate the intermediate values `gam` and `bet`, which are used in the forward substitution step. The intermediate value `bet` is updated at each iteration, and if it becomes zero, the algorithm raises an exception. The solution vector `x` is also updated at each iteration.  
   
Finally, the algorithm performs the backward substitution step to obtain the final solution vector `x`.  
   
## Complexity Analysis  
The time complexity of the tridiagonal matrix algorithm is O(n), where n is the size of the matrix. The algorithm performs two loops, one for the forward substitution step and one for the backward substitution step, each of which takes O(n) time. The intermediate calculations take constant time, so the overall time complexity is O(n).  
   
The space complexity of the algorithm is also O(n), as it requires arrays of size n to store the intermediate values and the solution vector.