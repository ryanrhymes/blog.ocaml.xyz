---
layout: post
title:  "Combinations with Repetitions"
date:   2021-01-25 02:11:06 +0200
categories: algorithm
---

## Introduction
Combinations with repetitions is a computer algorithm that is primarily used in the field of probability and statistics. The algorithm is usually implemented to find all the possible combinations that can be made from a set of items when each item can be used more than once. This type of problem is often encountered in mining, physics, chemistry and other fields that rely on statistics to make decisions or predictions.

## Implementation

```ocaml

(* Combinations with repetitions *)

let rec combs_with_rep k xxs =
  match k, xxs with
  | 0,  _ -> [[]]
  | _, [] -> []
  | k, x::xs ->
      List.map (fun ys -> x::ys) (combs_with_rep (k-1) xxs)
      @ combs_with_rep k xs

```

The Combinations with Repetitions algorithm is implemented in the programming language OCaml. The algorithm is recursive and uses pattern matching to break down the problem into smaller sub-problems.

## Step-by-step Explanation
1. The function `combs_with_rep` takes in two arguments: `k` which is the size of the combination to be generated and `xxs` which is the list of items to be combined.
2. The first step checks if `k` is zero. If it is, an empty list is returned since there are no combinations of zero elements.
3. The second step checks if `xxs` is an empty list. If it is, an empty list is returned since there are no items to form a combination with.
4. The third step breaks the list `xxs` into its head `x` and its tail `xs`.
5. A recursive call is made to the function `combs_with_rep` with one less element to select (`k-1`) and the rest of the list of items as the second argument `xs`. This gives us all the possible combinations of size `k-1` that can be formed from the list `xs`.
6. The result of the recursive call is then mapped over using a lambda function that adds the element `x` to the head of each sub-list. This generates all the possible combinations of size `k` that can be formed with `x` added.
7. The two lists of combinations formed in steps 5 and 6 are then concatenated to form the final list of combinations with repetitions of size `k`.

## Complexity Analysis
The time complexity of the Combinations with Repetitions algorithm can be approximated as O(n^k) where `n` is the size of the list of items and `k` is the size of the combination to be generated. This is because the algorithm generates all the possible combinations of size `k` by recursively breaking the problem into smaller sub-problems and iterating over the entire list each time. The space complexity of the algorithm is also O(n^k) since it generates all the possible combinations before returning them as a list.