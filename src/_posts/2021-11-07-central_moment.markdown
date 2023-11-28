---
layout: post
title:  "Central Moment"
date:   2021-11-07 22:04:39 +0200
categories: algorithm
---

## Introduction
The central moment is a statistical measure that describes the shape of a probability distribution. It measures the deviation of values in a dataset from the mean, and is often used in fields such as physics, engineering, and finance. 

## Implementation

```ocaml

(* Calculate Central Moment *)

let central_moment n x =
  let m = float_of_int n in
  let u = mean x in
  let x = Array.map (fun x -> (x -. u) ** m) x in
  let a = Array.fold_left ( +. ) 0. x in
  a /. float_of_int (Array.length x)

```

The OCaml code implements an algorithm to calculate the nth central moment of a dataset given as an array.

## Step-by-step Explanation
1. The function `central_moment` takes two parameters - 'n' and an array 'x'.
2. 'n' is the order of the moment to be calculated.
3. The mean of the array 'x' is calculated using the built-in 'mean' function and assigned to 'u'.
4. The values in the array 'x' are then modified to represent their deviation from the mean: each value is subtracted by 'u' and then raised to the 'n'th power.
5. The resulting array is assigned to 'x'.
6. The sum of all values in 'x' is calculated using the built-in 'fold_left' function, and assigned to 'a'.
7. The value of 'a' is divided by the length of 'x' to determine the average of the modified values, which represents the nth central moment.

## Complexity Analysis
The code has a time complexity of O(n), where 'n' is the length of the input array 'x'. This is due to the need to iterate over the entire array twice - once to calculate the mean, and again to modify the deviation of each value from the mean. The built-in 'fold_left' function is also O(n) in complexity. The space complexity of the algorithm is proportional to the size of the input array 'x', and is therefore O(n) as well.