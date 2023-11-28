---
layout: post
title:  "Numerical Differentiation"
date:   2022-06-14 03:11:31 +0200
categories: algorithm
---

## Introduction
The numerical differentiation method is one among many techniques used to numerically compute derivatives. This technique is widely used due to its simplicity and accuracy. Numerical differentiation method is mostly used in computer graphics, simulation of different scientific models, numerical models for climate forecast, numerical fluid flow analysis and is especially useful where an analytical solution is not obtainable.

## Implementation

```ocaml

(* Numerical Differentiation *)

open Owl_types

(* Functor of making numerical differentiation module of different precisions *)

module Make (A : Ndarray_Numdiff with type elt = float) = struct
  type arr = A.arr

  type elt = A.elt

  (* global epsilon value used in numerical differentiation *)
  let _eps = 0.00001

  let _ep1 = 1.0 /. _eps

  let _ep2 = 0.5 /. _eps

  (* derivative of f : scalar -> scalar *)
  let diff f x = (f (x +. _eps) -. f (x -. _eps)) *. _ep2

  (* derivative of f : scalar -> scalar, return both function value and derivative *)
  let diff' f x = f x, diff f x

  (* second order derivative of f : scalar -> scalar *)
  let diff2 f x = (f (x +. _eps) +. f (x -. _eps) -. (2. *. f x)) /. (_eps *. _eps)

  (* second order derivative of f : float -> float, return both function value and derivative *)
  let diff2' f x = f x, diff2 f x

  (* gradient of f : vector -> scalar, return both function value and gradient *)
  let grad' f x =
    let n = A.numel x in
    let g = A.create [| n |] (f x) in
    let gg =
      A.mapi
        (fun i xi ->
          let x' = A.copy x in
          A.set x' [| i |] (A.elt_to_float xi +. _eps);
          f x')
        x
    in
    g, A.((gg - g) *$ _ep1)


  (* gradient of f : vector -> scalar *)
  let grad f x = grad' f x |> snd

  (* transposed jacobian of f : vector -> vector, return both function value and jacobian *)
  let jacobianT' f x =
    let y = f x in
    let m, n = A.numel x, A.numel y in
    let j = A.tile y [| m; 1 |] in
    let jj = A.copy j in
    for i = 0 to m - 1 do
      let x' = A.copy x in
      let a = A.elt_to_float (A.get x [| i |]) in
      A.set x' [| i |] (a +. _eps);
      let y' = A.reshape (f x') [| 1; n |] in
      A.set_slice [ [ i ]; [] ] jj y'
    done;
    y, A.((jj - j) *$ _ep1)


  (* transposed jacobian of f : vector -> vector *)
  let jacobianT f x = jacobianT' f x |> snd

  (* jacobian of f : vector -> vector, return both function value and jacobian *)
  let jacobian' f x =
    let y, j = jacobianT' f x in
    y, A.transpose ~axis:[| 1; 0 |] j


  (* jacobian of f : vector -> vector *)
  let jacobian f x = jacobian' f x |> snd
end

```

The given algorithm implements the numerical differentiation method in OCaml to do the following.
- Calculate the derivative of a scalar to a scalar function
- Calculate the second-order derivative of a scalar to a scalar function
- Calculate the gradient of a vector to scalar function
- Calculate the transposed Jacobian of a vector to vector function
- Calculate the Jacobian of a vector to a vector function

These functions can also return the both function value and derivative as a tuple.

## Step-by-step Explanation
1. For the function `diff` and `diff'`, we define global values `_eps`, `_ep1`, and `_ep2` which are used in the numerical differentiation method. `_eps` is the value of Epsilon which is a small number used to approximate the derivative of a function. Optimally, it should be the square root of the machine's precision, which is roughly 1.0e-8 in 64-bit systems. `_ep1` and `_ep2` are two values used to calculate the derivative.
2. The function `diff` accepts a scalar to scalar function and a scalar value `x` as input, and returns the derivative of that function at `x`.
   - This is done by calculating the difference in function value at `x+_eps` and `x-_eps`, and then dividing it by `2*_eps`.
3. The function `diff'` is similar to `diff`, but also returns the function value along with the derivative in a tuple.
4. The function `diff2` is used to calculate the second-order derivative of a scalar to scalar function.
   - Similar to `diff`, this function accepts a scalar to scalar function and a scalar value as input and then returns the second-order derivative at `x`.
5. The function `diff2'` is similar to `diff2` but also returns the function value along with the second-order derivative in a tuple.
6. The function `grad'` accepts a vector to scalar function and a vector as input, and returns the gradient of that function at the given point as a tuple.
   - First, function value is evaluated at `x`. The number of elements in `x` is stored in `n`.
   - We initialise a gradient vector `g` with `n` number elements and equal to the function value. 
   - Then we create another matrix `gg` with `n` number columns and one row for each element in `x`. This matrix represents the function values evaluated at `x+_eps` and `x-_eps`.
   - For each element in `x`, the same value is copied to `x'` as in `x`. However, at the index of that element, `_eps` is added to its value.
   - Evaluating the function values at each of the points `x` and `x'` gives us a matrix `gg`. By subtracting `g` from `gg` and multiplying the result with `_ep1`, we eventually obtain the gradient.
7. The function `grad` is similar to `grad'`, but it only returns the calculated gradient.
8. The function `jacobianT'` is used to calculate the transposed Jacobian matrix of a vector to vector function.
   - First, the value of the function `f` is evaluated at `x`.
   - `j` is created as a `m x n` matrix where m is the number of elements in the vector `x` and n is the number of elements in the function evaluated at `x`.
   - We then copy `j` to `jj` so that we have a copy of the matrix j to which we will apply the finite difference method.
   - For each element in `x`, we create a copy of `x` and add `_eps` to the element at ith position to get `x'`.
   - The function is evaluated at `x'` and reshaped into an array of size `1 x n`. Then it is stored at the ith row of `jj`.
   - We subtract the matrix `j` from the resulting matrix `jj` and multiply the result with `_ep1` to get the transposed Jacobian of f.
9. The function `jacobianT` is similar to `jacobianT'`, but only returns the transpose Jacobian of the function.
10. The function `jacobian'` is used to calculate the Jacobian matrix of a vector to vector function.
   - First, we calculate the value of the function `f` at `x` and store it in variable `y`.
   - We then calculate the transpose jacobian of `f` at `x` and store it in variable `j`.
   - We return a tuple of `y` and the transposition of `j`.
11. The function `jacobian` is similar to `jacobian'`, but only returns the Jacobian of the function.

## Complexity Analysis
The time complexity of the given algorithm is dependent on the input function and the dimension of the input function parameters. The gradient and Jacobian functions have time complexity of O(n^2) where n is the dimension of the input, since they involve multiple evaluations of the input function. The functions `diff`, `diff'`, `diff2` and `diff2'` has time complexity of O(1), since they only require two evaluations of the input function. Due to the epsilon value used to approximate the derivative, these functions have a reduced accuracy compared to mathematical differentiation and may require a smaller value of epsilon to compute with better accuracy.