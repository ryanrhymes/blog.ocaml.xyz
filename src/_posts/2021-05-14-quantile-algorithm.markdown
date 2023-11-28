---
layout: post
title:  "Quantile Algorithm"
date:   2021-05-14 19:30:04 +0200
categories: algorithm
---

# Quantile Algorithm

## Introduction
The quantile algorithm is a statistical algorithm used to find the data value that splits a set of data samples into two equal halves. A quantile can be expressed as a percentage value (a percentile) or as a fraction, and is a commonly used statistical measure in fields such as finance, business, and healthcare.

## Implementation

```ocaml

(* Quantile Algorithm *)

let quantile x =
  let y = sort ~inc:true x in
  let n = Array.length y in
  fun p ->
    if p < 0. || p > 1.
    then
      raise (Invalid_argument "Owl_base_stats.quantile: expected float between 0 and 1")
    else (
      let index = p *. float_of_int (n - 1) in
      let lhs = int_of_float index in
      let delta = index -. float_of_int lhs in
      if n = 0
      then 0.
      else if lhs = n - 1
      then y.(lhs)
      else ((1. -. delta) *. y.(lhs)) +. (delta *. y.(lhs + 1)))

```

The implementation shown for this algorithm is written in the programming language OCaml. It sorts the input array of data in ascending order, and then calculates the quantile value based on the input value for p (the quantile value to be calculated). 

## Step-by-step Explanation
1. Sort the input array of data, x, in ascending order.
2. Calculate the length of the sorted array, n.
3. Define a function that takes in a value for p, the quantile to be found.
4. If the input value for p is less than 0 or greater than 1, raise an exception indicating an invalid input value.
5. Calculate the index of the quantile by multiplying p by n-1.
6. Get the integer value of the calculated index, and assign it to lhs.
7. Compute the delta value which is the difference between the calculated index and lhs.
8. If n is 0, return 0.
9. If lhs is equal to n - 1, return the last value in the sorted array.
10. Otherwise, calculate the quantile value using the formula ((1-delta) * y.(lhs)) + (delta * y.(lhs + 1)), where y.(lhs) is the value at index lhs in the sorted array.

## Complexity Analysis
The sorting operation in the algorithm takes O(n log n) time, where n is the length of the input array. However, the rest of the algorithm is a constant time operation, so the overall time complexity of the algorithm is O(n log n) due to the sorting operation. The worst-case space complexity of the algorithm is O(n) due to the space required to store the sorted array. Overall, the algorithm is efficient in most cases and provides a fast way to compute quantiles.