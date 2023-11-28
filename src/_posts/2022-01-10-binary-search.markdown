---
layout: post
title:  "Binary Search"
date:   2022-01-10 07:03:06 +0200
categories: algorithm
---

## Introduction
Binary Search is a searching algorithm that is used to search an element in a sorted list or array. It works by repeatedly dividing the search interval in half. The idea is to begin by comparing the middle element of the array with the target value. If the target value matches the middle element, the search ends successfully. Otherwise, depending on whether the target value is greater or less than the middle element, the search continues either in the left or the right half of the array respectively. It is a widely-used algorithm in computer science due to its efficiency in searching.

## Implementation

```ocaml

(* Binary Search *)

let rec binary_search a value low high =
  if high = low then
    if a.(low) = value then
      low
    else
      raise Not_found
  else let mid = (low + high) / 2 in
    if a.(mid) > value then
      binary_search a value low (mid - 1)
    else if a.(mid) < value then
      binary_search a value (mid + 1) high
    else
      mid

```

The binary_search algorithm takes in four parameters: an array `a`, the `value` to be searched for in the array, the `low` index and the `high` index of the search interval. It then proceeds based on the value comparison between the middle element and the search value.

## Step-by-step Explanation
1. The algorithm begins with the search interval defined by the `low` and `high` indices.
2. If the `low` and `high` indices are the same, the algorithm checks if the element at the `low` index is equal to the search `value`. If it is, the algorithm returns the index `low`, otherwise, it raises a `Not_found` exception indicating that the element is not in the array.
3. If the search interval has a length greater than 1 (`high` > `low`), the algorithm proceeds to calculate the `mid` index by taking the average value of the `low` and `high` indices: `mid = (low + high) / 2`.
4. The algorithm then checks if the `value` to be searched for is greater than or less than the element at the `mid` index: 
   1. If the `mid` element is greater than the `value`, the search interval is partitioned into the left half, and the algorithm is called recursively with the search interval now defined by the `low` index and (`mid` - 1) index.
   2. If the `mid` element is less than the `value`, the search interval is partitioned into the right half, and the algorithm is called recursively with the search interval now defined by the (`mid` + 1) index and `high` index.
   3. If the `mid` element is equal to the `value`, then the algorithm returns the index of the `mid` element.

5. The algorithm repeats steps 2–4 until the search value is found, or the entire array has been traversed unsuccessfully.

## Complexity Analysis
The best-case time complexity for binary search is O(1), which occurs when the target element being searched for is the middle element of the sorted array. The worst-case time complexity is O(log n), whereby the search interval is partitioned into halves at each step of the recursive call until the target element is found or the search space is exhausted. The space complexity for binary search is O(1) as it does not require additional space other than what is used to store the sorted array.