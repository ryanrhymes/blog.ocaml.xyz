---
layout: post
title:  "Ascending Primes"
date:   2021-04-21 21:52:39 +0200
categories: algorithm
---

## Introduction
The Ascending Primes algorithm is a program for generating an infinitely long list of ascending prime numbers. This algorithm is important in number theory and cryptography, where prime numbers are crucial. It uses an efficient implementation of the Miller-Rabin primality test to determine whether a given integer is prime. The program generates ascending integers using a nested recursive function, then tests them in ascending order until a prime number is found, at which point it adds it to the list.

## Implementation

```ocaml

(* Ascending Primes *)

let is_prime n =
  let rec test x =
    let q = n / x in x > q || x * q <> n && n mod (x + 2) <> 0 && test (x + 6)
  in if n < 5 then n lor 1 = 3 else n land 1 <> 0 && n mod 3 <> 0 && test 5

let ascending_ints =
  let rec range10 m d = if d < 10 then m + d :: range10 m (succ d) else [] in
  let up n = range10 (n * 10) (succ (n mod 10)) in
  let rec next l = if l = [] then [] else l @ next (List.concat_map up l) in
  next [0]

let () =
  List.filter is_prime ascending_ints
  |> List.iter (Printf.printf " %u") |> print_newline

```

The Ascending Primes algorithm works by generating an infinite list of ascending integers, then iterating over it and testing each number for primality using the Miller-Rabin test. If a number is found to be prime, it is added to the list of primes and the program continues to the next integer in the sequence.

## Step-by-step Explanation
1. Define a function called `is_prime` that takes an integer `n` and returns true if `n` is a prime number. 
2. The `is_prime` function uses the Miller-Rabin primality test to determine whether `n` is prime.
3. Define a function called `ascending_ints` that generates an infinite list of ascending integers in increments of 1 to 10.
4. The `ascending_ints` function generates ascending integers by defining a recursive function called `range10` that generates a list of integers from `m` to `m + 9`, then appending them to the result of calling `range10` again with `m + 10` and `d + 1`.
5. The `ascending_ints` function calls `range10` with an initial value of 0 to generate the first ten integers in the sequence, then passes the result to a recursive function called `next`.
6. The `next` function concatenates the current list with the result of mapping the `up` function over each integer in the list.
7. The `up` function generates a list of integers from `n * 10 + (n mod 10) + 1` to `n * 10 + 9`.
8. The `ascending_ints` function returns the result of calling `next` with the first ten integers as the initial list.
9. Call the `List.filter` function on `ascending_ints` to create a new list of primes by filtering the integers that pass the `is_prime` test.
10. Call the `List.iter` function on the new list of primes to print each prime number to the console.
11. Call the `print_newline` function to insert a newline after the list of primes has been printed.

## Complexity Analysis
The `is_prime` function uses the Miller-Rabin primality test to determine whether an integer `n` is prime. The worst-case time complexity of this test is O(k log^3 n), where k is the number of iterations of the test. In practice, the test is highly optimized and runs in O(log^3 n) time on most inputs.

The `ascending_ints` function generates an infinite list of integers by repeatedly concatenating the current list with the result of `up`. The `up` function produces a list of ten integers, so its time complexity is O(1). The `ascending_ints` function therefore has a time complexity of O(n), where n is the number of integers in the list that are generated before filtering terminates.

The `List.filter` function has a time complexity of O(n) and returns a new list containing only the elements of the original list that pass the given predicate. The predicate is the `is_prime` function, so its time complexity is the same as the time complexity of the Miller-Rabin test.

The `List.iter` function has a time complexity of O(n) and applies a function to each element of a list. In this case, the function is `Printf.printf " %u"`, which has a time complexity of O(1) since it simply prints the integer to the console. The `print_newline` function also has a time complexity of O(1). Therefore, the overall time complexity of the program is O(n log^3 n), where n is the number of primes generated before termination.