---
layout: post
title:  "Concordant"
date:   2021-01-13 14:20:56 +0200
categories: algorithm
---

## Introduction
The Concordant algorithm essentially calculates the similarities between two ordered list of elements (arrays). This algorithm is commonly applied in several fields including statistics, bioinformatics, and computational biology. The main application of the Concordant algorithm is in measuring the degree of dependence between two entities, and particularly between two datasets.

## Implementation

```ocaml

(* Concordant *)

let concordant x0 x1 =
  let c = ref 0 in
  for i = 0 to Array.length x0 - 2 do
    for j = i + 1 to Array.length x0 - 1 do
      if i <> j
         && ((x0.(i) < x0.(j) && x1.(i) < x1.(j)) || (x0.(i) > x0.(j) && x1.(i) > x1.(j)))
      then c := !c + 1
    done
  done;
  !c

```

The Concordant algorithm takes two arrays x0 and x1 of the same length and returns a count of the number of pairs of items whose direction of inequality is the same in both arrays. 

## Step-by-step Explanation
1. Start by initializing a counter variable c to zero
2. Using a nested for loop, iterate over the elements of the input arrays x0 and x1.
3. For each pair of elements (i,j):
    a. Confirm that i and j are not equal
    b. Compare the elements of x0 and x1 at indices i and j to determine direction of inequality in each array
    c. If the direction of inequality is the same for both elements, increment the counter variable c by 1.
4. Return the counter variable c

## Complexity Analysis
The Concordant Algorithm has a nested loop that runs over all elements in the arrays, resulting in a time complexity of **O(N^2)** where N is the length of the arrays. Hence, for large arrays, Concordant algorithm may take considerably more time to run. However, the algorithm uses a constant amount of space regardless of the input length since it only uses two input arrays for storing input values and a single integer to store the count of pairs that are concordant.