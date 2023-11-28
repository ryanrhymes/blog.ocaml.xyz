---
layout: post
title:  "Babbage Problem"
date:   2022-09-28 03:17:00 +0200
categories: algorithm
---

## Introduction
The Babbage problem is a classic problem in computer science named after Charles Babbage, one of the fathers of computing. The problem is essentially asking for the smallest positive integer whose square ends in the digits 269696.

## Implementation

```ocaml

(** Babbage Problem *)

let rec f a=
if (a*a) mod 1000000 != 269696
then f(a+1)
else a
in
let a= f 1 in
Printf.printf "smallest positive integer whose square ends in the digits 269696 is %d
" a

```

The Babbage problem algorithm is implemented in OCaml and is a basic recursive algorithm. 

## Step-by-step Explanation
1. The function `f` takes in a parameter `a`.
2. Checks if `(a*a) mod 1000000` is not equal to `269696`.
3. If the above condition is true, the function recursively calls itself with `a+1`.
4. If the condition is false, the function returns `a`.
5. The main function `a` calls `f` with an initial parameter of `1`.
6. The result is printed out.

## Complexity Analysis
The algorithm is a basic recursive algorithm with a single parameter. The worst-case scenario in terms of time complexity is when the algorithm has to check a large number of values before it returns. 

Let's first consider the time complexity. This algorithm has no average case since the worst-case scenario would be checking all integers until the required value is found. The time complexity of the algorithm is `O(n)` where `n` is the number of integers checked before the correct value is found.

In terms of space complexity, the algorithm uses a constant amount of space on the stack. This is because only the parameter `a` is kept in the stack during each recursive call. Therefore, the space complexity is `O(1)`.

Overall, the Babbage problem algorithm is efficient with the time complexity of `O(n)` and constant space complexity of `O(1)`.