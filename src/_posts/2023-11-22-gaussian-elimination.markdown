---
layout: post
title:  "Gaussian Elimination"
date:   2023-11-22 03:00:00 +0200
categories: algorithm
---

## Introduction  
Gaussian elimination is a widely used method for solving systems of linear equations. The method is named after the German mathematician Carl Friedrich Gauss. The algorithm works by transforming the augmented matrix of the system into a row echelon form, and then into a reduced row echelon form, from which the solution can be read off directly. Gaussian elimination is used in many fields, including engineering, physics, economics, and computer graphics.  
   
## Implementation  
The OCaml implementation of the Gaussian elimination algorithm takes two matrices as input: a matrix `a` representing the coefficients of the linear equations, and a matrix `b` representing the constants of the linear equations. The function returns two matrices: the transformed matrix `a` and the solution matrix `b`.   
  
```ocaml  
let linsolve_gauss a b =
  let dims_a, dims_b = M.shape a, M.shape b in
  let _, _ = _check_is_matrix dims_a, _check_is_matrix dims_b in
  let a = M.copy a in
  let b = M.copy b in
  let n = dims_a.(0) in
  let m = dims_b.(1) in
  let icol = ref 0 in
  let irow = ref 0 in
  let dum = ref 0.0 in
  let pivinv = ref 0.0 in
  let indxc = Array.make n 0 in
  let indxr = Array.make n 0 in
  let ipiv = Array.make n 0 in
  (* Main loop over the columns to be reduced. *)
  for i = 0 to n - 1 do
    let big = ref 0.0 in
    (* Outer loop of the search for at pivot element *)
    for j = 0 to n - 1 do
      if ipiv.(j) <> 1
      then
        for k = 0 to n - 1 do
          if ipiv.(k) == 0
          then (
            let v = M.get a [| j; k |] |> abs_float in
            if v >= !big
            then (
              big := v;
              irow := j;
              icol := k))
        done
    done;
    ipiv.(!icol) <- ipiv.(!icol) + 1;
    if !irow <> !icol
    then (
      for l = 0 to n - 1 do
        let u = M.get a [| !irow; l |] in
        let v = M.get a [| !icol; l |] in
        M.set a [| !icol; l |] u;
        M.set a [| !irow; l |] v
      done;
      for l = 0 to m - 1 do
        let u = M.get b [| !irow; l |] in
        let v = M.get b [| !icol; l |] in
        M.set b [| !icol; l |] u;
        M.set b [| !irow; l |] v
      done);
    indxr.(i) <- !irow;
    indxc.(i) <- !icol;
    let p = M.get a [| !icol; !icol |] in
    if p = 0.0 then raise Owl_exception.SINGULAR;
    pivinv := 1.0 /. p;
    M.set a [| !icol; !icol |] 1.0;
    for l = 0 to n - 1 do
      let prev = M.get a [| !icol; l |] in
      M.set a [| !icol; l |] (prev *. !pivinv)
    done;
    for l = 0 to m - 1 do
      let prev = M.get b [| !icol; l |] in
      M.set b [| !icol; l |] (prev *. !pivinv)
    done;
    for ll = 0 to n - 1 do
      if ll <> !icol
      then (
        dum := M.get a [| ll; !icol |];
        M.set a [| ll; !icol |] 0.0;
        for l = 0 to n - 1 do
          let p = M.get a [| !icol; l |] in
          let prev = M.get a [| ll; l |] in
          M.set a [| ll; l |] (prev -. (p *. !dum))
        done;
        for l = 0 to m - 1 do
          let p = M.get b [| !icol; l |] in
          let prev = M.get b [| ll; l |] in
          M.set b [| ll; l |] (prev -. (p *. !dum))
        done)
    done
  done;
  for l = n - 1 downto 0 do
    if indxr.(l) <> indxc.(l)
    then
      for k = 0 to n - 1 do
        let u = M.get a [| k; indxr.(l) |] in
        let v = M.get a [| k; indxc.(l) |] in
        M.set a [| k; indxc.(l) |] u;
        M.set a [| k; indxr.(l) |] v
      done
  done;
  a, b
```  
   
## Step-by-step Explanation  
The Gaussian elimination algorithm works by transforming the augmented matrix of the system of linear equations into a row echelon form, and then into a reduced row echelon form. The row echelon form is a matrix where the leading coefficient of each row is to the right of the leading coefficient of the row above it. The reduced row echelon form is a matrix where the leading coefficient of each row is 1, and all other entries in the column are 0.  
   
The implementation of the algorithm in OCaml begins by checking that the input matrices `a` and `b` are valid matrices. The function then creates copies of `a` and `b` to avoid modifying the input matrices. The function also initializes several variables and arrays that are used in the algorithm.  
   
The main loop of the algorithm iterates over the columns of the matrix `a` to be reduced. In each iteration, the algorithm searches for a pivot element in the column, which is the largest absolute value element in the column that has not already been used as a pivot. The algorithm then swaps the rows and columns of the pivot element to move it to the diagonal position. If the pivot element is 0, the algorithm raises an exception to indicate that the matrix is singular.  
   
After the pivot element is moved to the diagonal position, the algorithm scales the pivot row so that the pivot element is 1. The algorithm then subtracts multiples of the pivot row from the other rows to eliminate the entries below the pivot element in the same column. This process continues until the matrix `a` is in row echelon form.  
   
The algorithm then performs back substitution to transform the row echelon form of `a` into the reduced row echelon form. The algorithm iterates over the rows of `a` in reverse order, starting with the bottom row. For each row, the algorithm swaps the columns to put the pivot element in the correct position. The algorithm then subtracts multiples of the pivot row from the other rows to eliminate the entries above the pivot element in the same column. This process continues until the matrix `a` is in reduced row echelon form.  
   
Finally, the algorithm returns the transformed matrix `a` and the solution matrix `b`.  
   
## Complexity Analysis  
The time complexity of the Gaussian elimination algorithm is O(n^3), where n is the number of unknowns in the system of linear equations. This is because the algorithm involves three nested loops, and each loop iterates n times. Therefore, the total number of operations is proportional to n^3.  
   
The space complexity of the algorithm is O(n^2), which is the size of the matrices `a` and `b`. The algorithm creates copies of `a` and `b`, so the space complexity is doubled.  
   
In practice, the Gaussian elimination algorithm is very efficient for small to medium-sized systems of linear equations, but it can become slow for very large systems. In this case, other algorithms such as iterative methods or sparse matrix methods may be more appropriate.