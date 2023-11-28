---
layout: post
title:  "Discordant"
date:   2021-08-14 11:29:33 +0200
categories: algorithm
---

## Introduction
The algorithm is called "Discordant". It is used for counting the number of pairs of elements in two arrays that are in "discordance" with each other. In this context, discordance means one element of the first array is smaller than one element of the second array, but the positions of the two elements are reversed. The algorithm is applicable to many domains such as genetics, economics, and social networks.

## Implementation

```ocaml

(* Discordant *)

let discordant x0 x1 =
  let c = ref 0 in
  for i = 0 to Array.length x0 - 2 do
    for j = i + 1 to Array.length x0 - 1 do
      if i <> j
         && ((x0.(i) < x0.(j) && x1.(i) > x1.(j)) || (x0.(i) > x0.(j) && x1.(i) < x1.(j)))
      then c := !c + 1
    done
  done;
  !c

```

The algorithm, called "Discordant", takes two arrays of integers as input and returns the number of pairs of discordant elements in the arrays. It conducts a brute-force search by comparing each element of the arrays with all the other elements to count the number of discordant pairs.

## Step-by-Step Explanation
Here is a step-by-step explanation of how the Discordant algorithm works:

1. Initialize a variable 'c' to zero to count the number of discordant pairs of elements in the two arrays.
2. Start a loop from zero to the length of the first array minus two (i.e., the second-last element). This loop selects an element of the first array as the "reference" element.
3. Within the first loop, start a nested loop from the next element of the outer loop until the last element of the first array. This nested loop selects another element of the first array as the "comparing" element.
4. If the indices of the two elements are different (i.e., not the same element), check whether they are discordant or not. Elements are discordant if the reference element is smaller than the comparing element in the first array and larger than the comparing element in the second array, or if the reference element is larger than the comparing element in the first array and smaller than the comparing element in the second array.
5. If the two elements are discordant, increment the value of 'c'.
6. Continue the nested loop until the last element of the first array.
7. Continue the outer loop until the second-last element of the first array.
8. Once the loops are finished, return the value of 'c'.

## Complexity Analysis
The Discordant algorithm uses a brute-force search technique to check each pair of elements in the two arrays. Therefore, the time complexity of the algorithm is O(n^2) where 'n' is the length of the input arrays. In other words, the runtime of the algorithm increases quadratically with the length of the input arrays. Since the algorithm doesn't use any extra space other than a few integer variables, its space complexity is O(1). For smaller arrays, the brute-force search technique used by the algorithm may be computationaly efficient. However, for larger arrays, the time complexity might become unmanageable, and more efficient algorithms should be employed.