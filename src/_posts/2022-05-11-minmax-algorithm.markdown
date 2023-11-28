---
layout: post
title:  "MinMax Algorithm"
date:   2022-05-11 12:17:16 +0200
categories: algorithm
---

## Introduction

In computer science, the MinMax algorithm is a simple yet effective technique for finding both the minimum and maximum elements in an array iteratively. This algorithm has many real-world applications in fields such as mathematics, physics, and data science.

## Implementation

```ocaml

(* MinMax Algorithm *)

let minmax_i x =
  assert (Array.length x > 0);
  let _min = ref x.(0) in
  let _max = ref x.(0) in
  let _min_idx = ref 0 in
  let _max_idx = ref 0 in
  Array.iteri
    (fun i a ->
      if a < !_min
      then (
        _min := a;
        _min_idx := i)
      else if a > !_max
      then (
        _max := a;
        _max_idx := i))
    x;
  !_min_idx, !_max_idx

```


The MinMax algorithm takes an array of elements as input and returns a tuple containing the indices of the minimum and maximum elements in the array. The algorithm iteratively compares each element in the array to the current minimum and maximum elements and updates them accordingly. The resulting tuple contains the indices of the minimum and maximum elements respectively.

## Step-by-step Explanation

1. Initialize the minimum and maximum values to the first element and their indices to 0.
2. Iterate through all the elements of the array.
3. If a new minimum element is found, update the current minimum and its index.
4. If a new maximum element is found, update the current maximum and its index.
5. After all elements have been compared, return the tuple of the indices of the minimum and maximum elements.

## Complexity Analysis

The time complexity of the MinMax algorithm is O(n) since it requires only one pass through the array. The space complexity is O(1) since it uses only constant space to store the minimum and maximum values and their indices. This makes the MinMax algorithm very efficient and practical for real-world applications where time and space constraints are critical.