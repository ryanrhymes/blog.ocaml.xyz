---
layout: post
title:  "Cholesky decomposition"
date:   2021-02-04 17:41:44 +0200
categories: algorithm
---

## Introduction
The Cholesky decomposition is a matrix factorization technique used to solve systems of linear equations in the form of A*x=b, where A is a symmetric, positive-definite matrix. The Cholesky decomposition is a unique representation of A as L*L^T, where L is a lower-triangular matrix with positive diagonal elements and L^T is the transpose of L matrix. The Cholesky factorization has applications in various fields, such as statistics, finance, physics, and engineering.

## Implementation

```ocaml

(* Cholesky decomposition *)

let cholesky inp =
   let n = Array.length inp in
   let res = Array.make_matrix n n 0.0 in
   let factor i k =
      let rec sum j =
         if j = k then 0.0 else
         res.(i).(j) *. res.(k).(j) +. sum (j+1) in
      inp.(i).(k) -. sum 0 in
   for col = 0 to n-1 do
      res.(col).(col) <- sqrt (factor col col);
      for row = col+1 to n-1 do
         res.(row).(col) <- (factor row col) /. res.(col).(col)
      done
   done;
   res

let pr_vec v = Array.iter (Printf.printf " %9.5f") v; print_newline()
let show = Array.iter pr_vec
let test a =
   print_endline "
in:"; show a;
   print_endline "out:"; show (cholesky a)

let _ =
   test [| [|25.0; 15.0; -5.0|];
           [|15.0; 18.0;  0.0|];
           [|-5.0;  0.0; 11.0|] |];
   test [| [|18.0; 22.0;  54.0;  42.0|];
           [|22.0; 70.0;  86.0;  62.0|];
           [|54.0; 86.0; 174.0; 134.0|];
           [|42.0; 62.0; 134.0; 106.0|] |];

```

The algorithm takes an input matrix, performs the Cholesky decomposition, and returns its lower-triangular matrix. The main steps include initializing the result matrix with zero, computing the diagonal elements of the matrix recursively, and calculating the off-diagonal elements using the diagonal elements.

## Step-by-step Explanation
1. Initialize a matrix with n rows and columns, where n is the length of the input matrix. Set all elements to zero.
2. For each column k, from 0 to n-1, do the following:
   1. Compute the diagonal element of the kth column by subtracting the sum of the squares of the previous elements in the column from the kth element: 
   
      `res.(k).(k) <- sqrt (inp.(k).(k) − sum(res.(k).(j)^2), for j = 0 to k-1)`

   2. For each row i, from k+1 to n-1, do the following:
       1. Compute the off-diagonal elements of the kth column by subtracting the sum of the products of the corresponding elements in the ith and previous rows in the column from the ith element:
    
          `res.(i).(k) <- (inp.(i).(k) − sum(res.(i).(j) * res.(k).(j)), for j = 0 to k-1) / res.(k).(k)`

   3. End of loop
3. End of loop
4. Return the lower-triangular matrix.

## Complexity Analysis
The Cholesky decomposition algorithm has a time complexity of O(n^3/3), where n is the length of the input matrix. This is because the algorithm loops through each element of the matrix twice for each row and column, resulting in O(n^2), and the computation of the diagonal and off-diagonal elements take O(n) and O(n^2/2), respectively, leading to a total complexity of O(n^3/3). The space complexity of the algorithm is O(n^2) for storing the result matrix. However, since the input matrix is symmetric, only the lower half of the matrix is needed, reducing the space complexity to O(n^2/2). The Cholesky factorization is an efficient method for solving systems of linear equations with a symmetric, positive-definite matrix and is preferred over other methods like Gaussian elimination when applicable.