---
layout: post
title:  "Shannon Entropy"
date:   2022-03-09 13:57:42 +0200
categories: algorithm
---

# Shannon Entropy

## Introduction
Shannon entropy is a concept in information theory that measures the uncertainty or randomness associated with a set of data. It was introduced by Claude Shannon, a mathematician and electrical engineer, in his 1948 paper "A Mathematical Theory of Communication".

Shannon entropy is widely used in cryptography, data compression, and other fields where information needs to be encoded and transmitted efficiently and securely.

## Implementation

```ocaml

(* Shannon Entropy *)

module CharMap = Map.Make(Char)

let entropy s =
  let count map c =
    CharMap.update c (function Some n -> Some (n +. 1.) | None -> Some 1.) map
  and calc _ n sum =
    sum +. n *. Float.log2 n
  in
  let sum = CharMap.fold calc (String.fold_left count CharMap.empty s) 0.
  and len = float (String.length s) in
  Float.log2 len -. sum /. len

let () =
  entropy "1223334444" |> string_of_float |> print_endline

```

The implementation of the Shannon entropy algorithm is written in OCaml. It takes a string as input and returns the entropy of the string as a floating-point number. 

## Step-by-step Explanation
1. Define a module CharMap as a map from characters to floating-point numbers using the OCaml built-in Map library.
2. Define the `entropy` function which takes a string `s` as an argument.
3. Define the `count` function which takes a CharMap `map` and a character `c` as arguments. It counts the number of occurrences of character `c` in the string and updates the map accordingly. If `c` is not in the map, it is added with a count of 1.
4. Define the `calc` function which takes a character and its frequency in the string as arguments. It returns the product of frequency and the base-2 logarithm of frequency.
5. In the `entropy` function, fold `count` over the input string `s` starting with an empty CharMap to get a mapping of each character to its frequency.
6. Fold `calc` over the CharMap obtained in step 5 to calculate the Shannon entropy for the input string.
7. Calculate the length of the input string as a float using `String.length` and perform the final calculation to get the entropy as a floating-point number.
8. The entropy value is returned from the `entropy` function.

## Complexity Analysis
The Shannon entropy algorithm calculates the frequency of each character in a given string and then applies the Shannon entropy formula using that frequency distribution. 

Let `n` be the length of the input string. Then:

- The `String.fold_left` call in step 5 takes `O(n)` time to traverse the input string and update the character frequency map.
- The `CharMap.fold` call in step 6 takes `O(k log k)` time where `k` is the number of distinct characters in the input string. In the worst case, `k` can be as large as the total number of Unicode characters which is around 143,000.
- The final calculation in step 7 takes constant time.

Therefore, the overall time complexity of the algorithm is `O(n + k log k)`. In practice, the `k log k` term dominates since it is much larger than `n` for most input strings.