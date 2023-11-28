---
layout: post
title:  "Skewness Algorithm"
date:   2022-09-11 07:01:26 +0200
categories: algorithm
---

## Introduction
Skewness is a measure of the asymmetry of a probability distribution. It is used in various fields including finance, economics, and social sciences. Skewness is a fundamental statistic that helps to provide a better understanding of the underlying distribution of data. 

## Implementation

```ocaml

(* Calculate Skewness *)

let skew ?mean ?sd x =
  let m = _get_mean mean x in
  let s =
    match sd with
    | Some a -> a
    | None   -> std ~mean:m x
  in
  let t = ref 0. in
  Array.iter
    (fun a ->
      let s = (a -. m) /. s in
      t := !t +. (s *. s *. s))
    x;
  let n = float_of_int (Array.length x) in
  !t /. n

```

The `skew` function is used to calculate the skewness of an array of values. It takes three optional input arguments: `mean`, `sd`, and `x`. Here, `mean` is used to specify the mean value of the input array. `sd` is used to specify the standard deviation of the input array, and `x` is the input array. 

## Step-by-step Explanation
The `skew` function works in the following steps:

1. The `mean` value of the input array is calculated using the `_get_mean` function. If a `mean` value is specified as an input argument, that value is used. 
2. If a standard deviation value is specified as an input argument `sd`, it is used to calculate the skewness of the array. If no standard deviation value is specified, the `std` function is used to calculate the standard deviation of the input array. The `std` function also uses the `_get_mean` function to calculate the mean value of the input array.
3. An iteration loop is run for each value in the input array.
4. For each value in the array, the skewness value is calculated as follows: 
    - Subtract the mean value of the input array from the current array value.
    - Divide the result by the standard deviation value of the input array.
    - Calculate the cube of the result obtained from the previous step.
    - Add the result obtained to a running sum, `t`.
5. Divide the final value of `t` by the length of the input array to obtain the skewness value.

## Complexity Analysis
The `skew` function iterates over every value in the input array once, and for each value, performs a constant number of operations. Therefore, the time complexity of this function is O(n), where n is the length of the input array. The time complexity of the `_get_mean` function and the `std` function is also O(n), as both of these functions iterate over the input array once to compute their respective values. Thus, the overall time complexity of the `skew` function is dominated by the time complexity of the `_get_mean` and `std` functions. The space complexity of this function is O(1), as it uses a constant amount of memory for storing the `mean`, `sd`, `t`, and `n` values.