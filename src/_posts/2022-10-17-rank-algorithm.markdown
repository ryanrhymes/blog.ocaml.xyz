---
layout: post
title:  "Rank Algorithm"
date:   2022-10-17 13:33:36 +0200
categories: algorithm
---

## Introduction
Ranking is an important problem in various fields, such as sports, academic performance, and search relevance. This algorithm, the Rank Algorithm, aims to assign the correct rank to each element in an array of values, such that ties are handled correctly using a specified tie-breaking strategy.

## Implementation

```ocaml

(* Rank Algorithm *)

let rank ?(ties_strategy = `Average) vs =
  let n = Array.length vs in
  let order = argsort vs in
  let ranks = Array.make n 0. in
  let d = ref 0 in
  for i = 0 to n - 1 do
    if i == n - 1 || compare vs.(order.(i)) vs.(order.(i + 1)) <> 0
    then (
      let tie_rank = _resolve_ties (i + 1) !d ties_strategy in
      for j = i - !d to i do
        ranks.(order.(j)) <- tie_rank
      done;
      d := 0)
    else incr d (* Found a duplicate! *)
  done;
  ranks

```

The Rank Algorithm is implemented in OCaml and takes in an array of values `vs` as input. It also includes an optional parameter `ties_strategy`, which specifies the tie-breaking strategy to be used when there are ties in the values. The default strategy is the average of the tied ranks. 

## Step-by-step Explanation
1. The algorithm starts by getting the length of the input array and initializing an array of integers `order` containing the indices of the elements in `vs`, sorted in ascending order of their values. 
2. It also initializes an array `ranks` of the same length as `vs` with all elements initially set to 0. This array will contain the final ranks assigned to each element in `vs`.
3. A reference variable `d` is created and set to 0.
4. The algorithm then loops through each element in the sorted array `order` and performs the following steps:
   - If the current element is the last element or has a different value from the next element, then this signifies the end of a run of duplicate values. 
   - The algorithm then calls a helper function `_resolve_ties` to determine the rank to assign to each element in the run. The function takes in the length of the run and the current value of the reference `d` (which contains the number of previous duplicates encountered so far). It also takes in the tie-breaking strategy specified in `ties_strategy`.
   - The helper function returns the rank to assign to each element in the run, and the algorithm updates the corresponding ranks in the `ranks` array using the original indices from `order`.
   - The reference `d` is set to 0 to start counting duplicates for the next run.
   - If the current element has the same value as the next element, then this signifies a duplicate value. The reference `d` is incremented and the loop continues to the next element.
5. Once the loop is finished, the algorithm returns the `ranks` array containing the assigned ranks.

## Complexity Analysis
The Rank Algorithm has a time complexity of `O(n log n)` due to the initial sorting of the values in `vs`. The loop that assigns ranks to each value has a time complexity of `O(n)` since each element is visited once. The `_resolve_ties` helper function has a time complexity of `O(n)` in the worst-case scenario where all `n` elements are tied. Overall, the time complexity of the Rank Algorithm is `O(n log n)` + `O(n)` + `O(n)` = `O(n log n)`. The space complexity of the algorithm is `O(n)` since the `order` and `ranks` arrays both have size `n`.