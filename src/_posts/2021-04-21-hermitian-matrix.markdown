---
layout: post
title:  "Hermitian Matrix"
date:   2021-04-21 03:29:09 +0200
categories: algorithm
---

## Introduction

The Hermitian matrix is a square matrix with complex entries, which is equal to its own conjugate transpose. This matrix has many applications in physics, engineering, and signal processing, particularly in quantum mechanics. This algorithm implements the Hermitian of a given square matrix.

## Implementation

```ocaml

(* Hermitian Matrix *)

let hermitian ?(upper = true) x =
  let m, n = shape x in
  Owl_exception.(check (m = n) (NOT_SQUARE [| m; n |]));
  let y = copy x in
  let _y = flatten y |> Bigarray.array1_of_genarray in
  let _conj_op = _owl_conj (kind x) in
  let ofs = ref 0 in
  let incx, incy =
    match upper with
    | true  -> 1, m
    | false -> m, 1
  in
  for i = 0 to m - 1 do
    (* copy and conjugate *)
    _conj_op (m - i) ~ofsx:!ofs ~incx ~ofsy:!ofs ~incy y y;
    (* set the imaginary part to zero by default. *)
    let a = _y.{!ofs} in
    _y.{!ofs} <- Complex.{ re = a.re; im = 0. };
    ofs := !ofs + n + 1
  done;
  (* return the symmetric matrix *)
  y

```


The function `hermitian` implements the Hermitian of a given matrix. It takes an optional argument `upper` with a default value `true`, which specifies whether the Hermitian matrix should be an upper or lower triangular matrix. If `upper` is true, then the upper triangular part of the matrix is computed, and if it is false, then the lower triangular part is computed. The function returns the Hermitian matrix.

## Step-by-step Explanation

The `hermitian` function works as follows:

1. It first gets the dimensions of the input matrix `x` using the `shape` function and checks if it is a square matrix. If it is not a square matrix, then it raises an exception.
   
2. It then makes a copy of the input matrix `x` and creates a flat vector `_y` from the copy using the `flatten` function and `Bigarray.array1_of_genarray` function.
   
3. It defines a new function `_conj_op` that takes an element in `_y` and returns its conjugate using the `conj` function. It uses this operator to conjugate the matrix elements.
   
4. It initializes the `ofs` variable to 0, which is initially the offset of the vector `_y`.
   
5. It defines the `incx` and `incy` variables based on the `upper` argument. If `upper` is true, then `incx` is 1, and `incy` is equal to the number of rows in the matrix. If `upper` is false, then `incx` is equal to the number of rows in the matrix, and `incy` is 1.
   
6. It then starts a loop from 0 to the number of rows in the matrix minus 1.
   
7. In each iteration, it first conjugates the remaining part of the matrix using the `_conj_op` function and sets the imaginary part of the conjugation to zero by default. This step produces the Hermitian of the matrix.
   
8. It then updates the offset `ofs` based on the value of `incx` and `incy`.
   
9. After the loop, it returns the Hermitian matrix.

## Complexity Analysis

The time complexity of the `hermitian` function depends on the size of the input matrix. If the input matrix has dimensions `n` by `n`, the for-loop runs `n` times. The inner `for` loop does `n-i` conjugations and each conjugation operation is O(1). Therefore, the time complexity is O(n^3), which is the same as the complexity of matrix multiplication. The space complexity is O(n^2), which is due to the copy of the input matrix, and the flat vector `_y`.