---
layout: post
title:  "Dot Product"
date:   2022-03-26 00:42:53 +0200
categories: algorithm
---

## Introduction

The dot product is a mathematical operation that takes two vectors and returns a scalar value. In machine learning, the dot product is used to calculate the similarity between two vectors, which is a common operation for tasks such as text classification or image recognition. The dot product is also used in other fields, such as physics and engineering.

## Implementation

```ocaml

(* Dot Product *)

let dot v u =
  if Array.length v <> Array.length u
  then invalid_arg "Different array lengths";
  let times v u =
    Array.mapi (fun i v_i -> v_i *. u.(i)) v
  in Array.fold_left (+.) 0. (times v u)

```


This algorithm calculates the dot product of two vectors represented as arrays in OCaml. If the length of the two arrays is not the same, it raises an exception.

## Step-by-step Explanation

1. Define a function `dot` that takes two array arguments `v` and `u`.
2. Check if the length of array `v` is not equal to the length of array `u`. If it is different, raise an exception.
3. Define a helper function `times` that takes two arguments `v` and `u` and returns a new array where each element is the product of the corresponding elements of `v` and `u`.
   1. Use Array.mapi to iterate over the elements of array `v` and apply a function that multiplies the `i`-th element of `v` with the `i`-th element of `u`.
4. Use `Array.fold_left` to sum all the elements of the `times` array and return the result.

## Complexity Analysis

The time complexity of this algorithm is O(n), where n is the length of the input arrays. This is because the algorithm iterates over all the elements of the arrays once to calculate the dot product. The space complexity is also O(n) because we create a new array `times` to store the products of the corresponding elements in the input arrays. However, this array is not stored in memory once the function `dot` returns.