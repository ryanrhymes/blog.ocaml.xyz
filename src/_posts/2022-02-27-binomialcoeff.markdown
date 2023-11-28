---
layout: post
title:  "Binomial coefficients"
date:   2022-02-27 12:05:36 +0200
categories: algorithm
---

## Introduction

Binomial coefficients are mathematical entities that arise in combinatorics, probability theory, and algebra. The binomial coefficient `n choose k` represents the number of ways to choose k elements from a set of n elements without regard to order. The algorithm we will discuss computes the binomial coefficient using a combinatorial formula that iteratively multiplies the numerator by n - i and the denominator by i + 1, starting from 1 and going up to the smaller of k and n - k.

## Implementation

```ocaml

(* Evaluate binomial coefficients *)

let binomialCoeff n p =
  let p = if p < n -. p then p else n -. p in
  let rec cm res num denum =
    if denum <= p then cm ((res *. num) /. denum) (num -. 1.) (denum +. 1.)
    else res in
  cm 1. n 1.

```


The binomial coefficient algorithm takes two arguments: `n` and `p`. It calculates the binomial coefficient of `n` and `p`. It first computes the minimum of `p` and `n - p`, then initializes variables `res`, `num`, and `denum` and passes them to a helper function `cm`.

`cm` takes a result `res`, a numerator `num`, and a denominator `denum` and iteratively multiplies `res` by `num` and divides by `denum`, decrementing `num` and incrementing `denum` until `denum` is greater than `p`. The final result of `cm` is the binomial coefficient of `n` and `p`.

## Step-by-step Explanation

1. Check if `p` is greater than `n - p`. If it is, set `p` to `n - p` to reduce the number of computations. This is because `n choose k` is equivalent to `n choose n-k`, and `k` must be no greater than `n / 2`.

2. Initialize a result variable `res` to 1, a numerator variable `num` to `n`, and a denominator variable `denum` to 1.

3. Pass `res`, `num`, and `denum` to a helper function `cm`.

4. In `cm`, calculate `res * num / denum` and assign to `res`.

5. Decrement `num` by 1 and increment `denum` by 1.

6. If `denum` is less than or equal to `p`, recurse by calling `cm` with `res`, `num`, and `denum` as arguments.

7. If `denum` is greater than `p`, return `res` as the binomial coefficient of `n` and `p`.

## Complexity Analysis

The algorithm performs a constant amount of initialization before calling the helper function `cm`. The helper function `cm` performs `p` iterations, each of which performs one multiplication and one division. Therefore, the time complexity of the algorithm is O(p), where `p` is the smaller of `p` and `n - p`.

The space complexity of the algorithm is O(1), as it only initializes a constant number of variables regardless of the input size.