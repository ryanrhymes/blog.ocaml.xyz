---
layout: post
title:  "Calculate Median"
date:   2021-04-12 23:42:42 +0200
categories: algorithm
---

## Introduction 

In statistics, the median is the value that separates the higher half of a data sample from the lower half. It is often used as a measure of central tendency in data analysis. In this algorithm, we are provided with a list of numbers. The algorithm sorts the list in non-descending order and then calculates the median of the data.

## Implementation

```ocaml

(* Calculate Median *)

let median array =
  let len = Array.length array in
    Array.sort compare array;
    (array.((len-1)/2) +. array.(len/2)) /. 2.0;;

let a = [|4.1; 5.6; 7.2; 1.7; 9.3; 4.4; 3.2|];;
median a;;
let a = [|4.1; 7.2; 1.7; 9.3; 4.4; 3.2|];;
median a;;

```

The given algorithm implements a function that takes an array of float as input and returns the median of the data. The implementation of the algorithm is done in the following steps:

1. First, the length of the array is calculated.
2. The array is sorted in non-descending order using the built-in `sort` function in OCaml.
3. The median of the array is calculated by:
   * taking the average of the middle two elements if the array has even length, or 
   * returning the middle element if the array has odd length.

## Step-by-step Explanation

1. The array is passed as an argument to the `median` function.
2. The `len` variable is assigned the length of the input array using the `Array.length` function.
3. The `compare` function is used to sort the array in non-descending order using the built-in `Array.sort` function.
4. If the length of the array is even, then the median is calculated by adding the element at index `(len-1)/2` and `len/2`, and then dividing it by 2, which gives the average of the middle two elements.
5. If the length of the array is odd, then the median is the element at index `len/2`.
6. The median value is returned by the function.

## Complexity Analysis

The time complexity of this algorithm is O(nlogn) due to the use of the built-in `Array.sort` function, which has a time complexity of O(nlogn). The space complexity is O(n), as the array needs to be stored in memory. 

In addition to space and time complexities, it is worth noting that the algorithm modifies the input array, which could be a disadvantage in some contexts.