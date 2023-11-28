---
layout: post
title:  "Disarium numbers"
date:   2022-03-13 23:10:48 +0200
categories: algorithm
---

## Introduction
The Disarium number is a special type of number in which the sum of its digits powered with their respective positions gives the original number itself. This algorithm checks whether a given number is a Disarium number or not.

## Implementation

```ocaml

(* Disarium numbers *)

let rec pow b n =
  if n land 1 = 0
  then if n = 2 then b * b else pow (b * b) (n lsr 1)
  else if n = 3 then b * b * b else b * pow (b * b) (n lsr 1)

let is_disarium n =
  let rec aux x f =
    if x < 10
    then f 2 x
    else aux (x / 10) (fun l y -> f (succ l) (y + pow (x mod 10) l))
  in
  n = aux n Fun.(const id)

let () =
  Seq.(ints 0 |> filter is_disarium |> take 19 |> iter (Printf.printf " %u%!"))
  |> print_newline

```

The algorithm has two functions:

The first function named `pow` computes the result of the exponentiation of the given base `b` to the given exponent `n`. It recursively divides the exponent by 2 to repeatedly square the base by repeated multiplication, so that fewer total multiplication operations are necessary.

The second function named `is_disarium` checks if the given integer `n` is a Disarium number. It computes the sum of the powers by iterating over the digits of the given number from the least significant digit towards the most significant one. It recursively divides the numbers by 10 and computes the sum of the powered digit values.

The last block of code invokes the `Seq` module and generates a stream of integers from `0` onward, filters out all Disarium numbers, takes the first `19` values, and prints them out with the `iter` function.

## Step-by-Step Explanation
1. The `pow` function takes two parameters: the base `b` and the exponent `n`.
2. It first checks if `n` is an odd number by checking if `n land 1 = 0`. If it is even, it recursively divides the exponent by 2 and squares the base by repeated multiplication until the exponent becomes 2.
3. If `n` is odd, it checks if `n` is equal to `3`. If it is, it multiplies the base by itself three times in order to get the result.
4. If `n` is an odd number that is greater than `3`, it recursively divides the exponent by 2 and squares the base by repeated multiplication until the exponent becomes 3, then it multiplies it by the squared base.
5. The `is_disarium` function takes one integer parameter `n`.
6. It uses the `aux` function, which takes two parameters, `x` and `f`. `x` represents the number whose digits are summed up, and `f` is a function that applies the power function to each digit and adds them together.
7. If `x` is less than `10`, it is the last digit of the given number, and the `aux` function applies the power function `2` to it and returns the result.
8. If `x` is greater than `10`, it is the combination of two things: the result of the repeated division by `10` and the last digit obtained by computing the remainder of `x` divided by `10`.
9. The `aux` function applies the power function to the last digit of `x`, adds it to the result of the recursive call to `aux` with the quotient, and increments the power of the digits by `1`.
10. The `is_disarium` function returns `true` if and only if the sum of the powered digits `x` is equal to the original number `n`.

## Complexity Analysis
The `pow` function has a time complexity of $O(\log n)$, where `n` is the exponent, because it recursively divides the exponent by two at each recursive call, reducing the number of multiplication operations needed.

The `is_disarium` function iterates over the digits of the number `n`, so the time complexity depends on the number of digits. The number of digits is proportional to the logarithm of `n`, so the time complexity is $O(\log n)$.

Overall, the time complexity of the algorithm is $O(\log n)$.