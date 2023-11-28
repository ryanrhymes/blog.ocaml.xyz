---
layout: post
title:  "Kendall Tau"
date:   2021-08-08 07:05:20 +0200
categories: algorithm
---

## Introduction
Kendall Tau is a correlation measure that provides a non-parametric way of determining the similarity between two rankings. Rank correlation is a statistical measure that analyses the relationship between two variables by comparing their rankings among themselves. The Kendall Tau distance method is widely used in machine learning, bioinformatics, and information retrieval to measure data similarity and classification.

## Implementation

```ocaml

(* Kendall Tau *)

let kendall_tau x0 x1 =
  let a = float_of_int (concordant x0 x1) in
  let b = float_of_int (discordant x0 x1) in
  let n = float_of_int (Array.length x0) in
  2. *. (a -. b) /. (n *. (n -. 1.))

```

The Kendall Tau algorithm is implemented in OCaml programming language. Given two arrays x0 and x1 of the same length, the function kendall_tau calculates the Kendall Tau value and returns the result.

## Step-by-step Explanation
1. Define concordant and discordant pairs of elements in x0 and x1, respectively. For example, in the arrays [1;2;3;4] and [2;4;1;3], pair (1, 2) is concordant and (1, 3) is discordant.
2. Calculate the number of concordant pairs, a, and the discordant pairs, b.
3. Determine the length of the arrays n.
4. Use the formula 2 * (a - b) / (n * (n - 1)) to calculate the Kendall Tau value.
5. Return the result.

## Complexity Analysis
The time complexity of this algorithm is O(n^2) because the algorithm must compare each element in the two arrays to each other. However, in practice, the algorithm is often more efficient because the concordant and discordant pairs are computed simultaneously at the same time, reducing the actual number of necessary comparisons. 

The space complexity of this algorithm is O(n) because the space required does not depend on the input size. This is because the arrays are passed by reference and do not need to be copied.