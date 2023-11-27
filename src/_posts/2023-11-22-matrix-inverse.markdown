---
layout: post
title:  "Matrix Inverse"
date:   2023-11-22 06:00:00 +0200
categories: algorithm
---

## Introduction  
The inverse of a matrix is a matrix that, when multiplied by the original matrix, results in the identity matrix. The inverse of a matrix is useful in solving systems of linear equations, computing determinants, and in other areas of mathematics. The algorithm we will be discussing is used to find the inverse of a square matrix.  
   
## Implementation 

```ocaml
let inv varr =
  let _k = M.kind varr in
  let _add = Owl_base_dense_common._add_elt _k in
  let _mul = Owl_base_dense_common._mul_elt _k in
  let _div = Owl_base_dense_common._div_elt _k in
  let _neg = Owl_base_dense_common._neg_elt _k in
  let _zero = Owl_const.zero _k in
  let _one = Owl_const.one _k in
  let dims = M.shape varr in
  let _ = _check_is_matrix dims in
  let n = Array.unsafe_get dims 0 in
  if Array.unsafe_get dims 1 != n
  then failwith "no inverse - the matrix is not square"
  else (
    let pivot_row = Array.make n _zero in
    let result_varr = M.copy varr in
    for p = 0 to n - 1 do
      let pivot_elem = M.get result_varr [| p; p |] in
      if M.get result_varr [| p; p |] = _zero
      then failwith "the matrix does not have an inverse";
      (* update elements of the pivot row, save old vals *)
      for j = 0 to n - 1 do
        pivot_row.(j) <- M.get result_varr [| p; j |];
        if j != p then M.set result_varr [| p; j |] (_div pivot_row.(j) pivot_elem)
      done;
      (* update elements of the pivot col *)
      for i = 0 to n - 1 do
        if i != p
        then
          M.set
            result_varr
            [| i; p |]
            (_div (M.get result_varr [| i; p |]) (_neg pivot_elem))
      done;
      (* update the rest of the matrix *)
      for i = 0 to n - 1 do
        let pivot_col_elem = M.get result_varr [| i; p |] in
        for j = 0 to n - 1 do
          if i != p && j != p
          then (
            let pivot_row_elem = pivot_row.(j) in
            (* use old value *)
            let old_val = M.get result_varr [| i; j |] in
            let new_val = _add old_val (_mul pivot_row_elem pivot_col_elem) in
            M.set result_varr [| i; j |] new_val)
        done
      done;
      (* update the pivot element *)
      M.set result_varr [| p; p |] (_div _one pivot_elem)
    done;
    result_varr)
```

The algorithm is implemented in the OCaml programming language. It takes as input a matrix represented as an Owl ndarray and returns the inverse of the matrix as an ndarray. Here is an example of how to use the `inv` function:  
   
```ocaml  
let m = Owl.Mat.of_array [| [| 1.; 2.; 3. |]; [| 4.; 5.; 6. |]; [| 7.; 8.; 10. |] |] in  
let m_inv = inv m in  
Owl.Mat.print m_inv  
```  
This will print the inverse of the matrix `m`.  
   
## Step-by-step Explanation  
The algorithm works by performing a series of operations on the input matrix until it is transformed into the identity matrix. These operations are performed on both the input matrix and an identity matrix, which is used to keep track of the transformations. Here are the steps of the algorithm:  
   
1. Check that the input matrix is square. If it is not, the function will return an error.  
2. Create a pivot row array that will be used to store the values of the row that is being transformed.  
3. Create a copy of the input matrix that will be transformed into the identity matrix.  
4. Iterate over the diagonal elements of the matrix (i.e., the pivot elements).  
5. If the pivot element is zero, the matrix does not have an inverse, so the function will return an error.  
6. Update the pivot row by dividing each element by the pivot element. Save the old values of the row in the pivot row array.  
7. Update the pivot column by dividing each element (except for the pivot element) by the negative of the pivot element.  
8. Update the rest of the matrix by subtracting the product of the pivot row and pivot column from each element (except for the pivot element).  
9. Update the pivot element by taking the reciprocal of the original pivot element.  
10. Return the transformed matrix, which is the inverse of the input matrix.  
   
## Complexity Analysis  
The time complexity of the algorithm is O(n^3), where n is the size of the input matrix. This is because the algorithm performs O(n) operations for each of the n diagonal elements, resulting in a total of O(n^3) operations. The space complexity of the algorithm is also O(n^3), since it creates a copy of the input matrix and a pivot row array of size n. Therefore, the algorithm is not suitable for very large matrices, as it may take a long time and use a lot of memory to compute the inverse.