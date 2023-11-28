---
layout: post
title:  "Hadamard Matrix algorithm."
date:   2021-08-10 07:29:17 +0200
categories: algorithm
---

## Introduction

The Hadamard matrix, invented by French mathematician Jacques Philippe Marie Binet and named after Jacobi's friend, James Joseph Sylvester, is a square matrix whose size is a power of two integers. The Hadamard matrix is significant and is widely used in many applications in diverse fields such as coding theory, signal processing, quantum computing, data encryption, and compression, among others. The matrix is orthogonal, meaning its rows and columns are mutually orthogonal, and it has a determinant of either 1 or -1.

## Implementation

```ocaml

(* Hadamard Matrix *)

let hadamard k n =
  (* function to build up hadamard matrix recursively *)
  let rec _make_hadamard
      (cp_op : ('a, 'b) Owl_core_types.owl_arr_op18)
      (neg_op : ('a, 'b) Owl_core_types.owl_arr_op18)
      len
      n
      base
      x
    =
    if len = base
    then ()
    else (
      let len' = len / 2 in
      _make_hadamard cp_op neg_op len' n base x;
      let ofsx = ref 0 in
      for _i = 0 to len' - 1 do
        let x1_ofs = !ofsx + len' in
        let x2_ofs = !ofsx + (len' * n) in
        let x3_ofs = x2_ofs + len' in
        cp_op len' ~ofsx:!ofsx ~incx:1 ~ofsy:x1_ofs ~incy:1 x x;
        cp_op len' ~ofsx:!ofsx ~incx:1 ~ofsy:x2_ofs ~incy:1 x x;
        cp_op len' ~ofsx:!ofsx ~incx:1 ~ofsy:x3_ofs ~incy:1 x x;
        (* negate the bottom right block *)
        neg_op len' ~ofsx:x3_ofs ~incx:1 ~ofsy:x3_ofs ~incy:1 x x;
        ofsx := !ofsx + n
      done)
  in
  (* function to convert the pre-calculated hadamard array into type k *)
  let _float_array_to_k : type a b. (a, b) kind -> float array -> a array =
   fun k a ->
    match k with
    | Bigarray.Float32   -> a
    | Bigarray.Float64   -> a
    | Bigarray.Complex32 -> Array.map (fun b -> Complex.{ re = b; im = 0. }) a
    | Bigarray.Complex64 -> Array.map (fun b -> Complex.{ re = b; im = 0. }) a
    | _                  -> failwith "Owl_dense_matrix_generic.hadamard"
  in
  (* start building, only deal with pow2 of n, n/12, n/20. *)
  let x = empty k n n in
  let cp_op = _owl_copy k in
  let neg_op = _owl_neg k in
  if Owl_maths.is_pow2 n
  then (
    Owl_dense_ndarray_generic.set x [| 0; 0 |] (Owl_const.one k);
    _make_hadamard cp_op neg_op n n 1 x;
    x)
  else if Owl_maths.is_pow2 (n / 12) && Stdlib.(n mod 12) = 0
  then (
    let y = _float_array_to_k k _hadamard_12 in
    let y = of_array k y 12 12 in
    let _area = area 0 0 11 11 in
    copy_area_to y _area x _area;
    _make_hadamard cp_op neg_op n n 12 x;
    x)
  else if Owl_maths.is_pow2 (n / 20) && Stdlib.(n mod 20) = 0
  then (
    let y = _float_array_to_k k _hadamard_20 in
    let y = of_array k y 20 20 in
    let _area = area 0 0 19 19 in
    copy_area_to y _area x _area;
    _make_hadamard cp_op neg_op n n 20 x;
    x)
  else failwith "Owl_dense_matrix_generic:hadamard"

```


The Hadamard matrix algorithm in OCaml implements the process of generating Hadamard matrices using a recursive method. The algorithm builds a Hadamard matrix pad the base size of one by one. Afterward, the algorithm passes through the up-to-down and the left-to-right direction of the base size matrix, creating a block of tam by tam matrices that are negated. 

## Step-by-step Explanation

The Hadamard matrix algorithm builds up the Hadamard matrix using the following steps:

- The algorithm contains a recursive internal function `_make_hadamard` that takes the pre-calculated matrix and performs recursive operations on it.
- `_make_hadamard` is responsible for generating the Hadamard matrix recursively from the base size of one by one to the final size, n.
- If the length of the matrix equals the base size, then the execution stops. Otherwise, it continues to divide the matrix into four pieces.
- The first recursive call in building the matrix calls `_make_hadamard` function with the next size that is halved. The operation then keeps on continuing until it reaches the base case.
- The current matrix block gets negated in the bottom-right part of the matrix and continued to move to the rest nested levels.
- If n's size is not a power of two or n modulo 12 or 20 is not zero, the function fails.

## Complexity Analysis

The Hadamard matrix algorithm has a time complexity of O(n^2log⁡n). The algorithm uses a recursive function to build the matrix, and if the size `n` is a power of two, it recursively makes four calls with a matrix of size `n / 2`. The function complexity grows with the recursion depth, which is log⁡n. The recursive function visits all the cells in the matrix while maintaining a constant overhead per cell. Therefore, the running time is O(n^2log⁡n). The algorithm's space complexity is O(n^2) since it maintains a constant overhead for each cell.