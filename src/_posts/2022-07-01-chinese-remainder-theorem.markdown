---
layout: post
title:  "Chinese remainder theorem"
date:   2022-07-01 14:18:08 +0200
categories: algorithm
---

## Introduction
The Chinese remainder theorem is a mathematical concept that says you can find a unique solution to a set of simultaneous congruences using the remainders obtained when the same number is divided by different numbers. The theorem has a wide range of applications, including finding solutions to the congruence equations. The algorithm based on this theorem aims to find the solution to a set of congruences simultaneously.

## Implementation

```ocaml

(* Chinese remainder theorem *)

exception Modular_inverse
let inverse_mod a = function
  | 1 -> 1
  | b -> let rec inner a b x0 x1 =
           if a <= 1 then x1
           else if  b = 0 then raise Modular_inverse
           else inner b (a mod b) (x1 - (a / b) * x0) x0 in
         let x = inner a b 0 1 in
         if x < 0 then x + b else x

let chinese_remainder_exn congruences = 
  let mtot = congruences
             |> List.map (fun (_, x) -> x)
             |> List.fold_left ( *) 1 in
   (List.fold_left (fun acc (r, n) ->
                  acc + r * inverse_mod (mtot / n) n * (mtot / n)
                ) 0 congruences)
             mod mtot

let chinese_remainder congruences =
   try Some (chinese_remainder_exn congruences)
   with modular_inverse -> None

```

This algorithm finds the solutions to a set of simultaneous congruences using the Chinese remainder theorem. The algorithm takes a list of tuples, each containing a remainder and a modulus. The algorithm returns the solution, which is a number that gives the remainders when divided by each modulus in the inputted `congruences`. If the solution is not possible, the algorithm returns `None`.

## Step-by-step Explanation
The algorithm for finding the solution to a set of simultaneous congruences using the Chinese remainder theorem is outlined below:

1. Take a list of (remainder, modulus) tuples as the input. 

2. Calculate `mtot`, which is the product of all the moduli.

3. Iterate through each tuple in the inputted `congruences` and calculate a multiplier `mtot / n` where `n` is the modulus of the current tuple.

4. Calculate the modular inverse of the multiplier obtained in step 3 modulo the modulus of the current tuple using extended Euclidean algorithm.

5. Multiply the remainder of the current tuple with the value obtained in the step 4, and then multiply the result obtained with the multiplier calculated in step 3.

6. Add the result obtained in step 5 to a running sum.

7. Once all the tuples in `congruences` have been accounted for, the final result will be the sum modulo `mtot`.

8. If the modular inverse cannot be calculated in step 4, it means there is no solution for the given set of congruences. In this case, return None.

## Complexity Analysis
The time complexity of the algorithm is `O(n^2)` where `n` is the number of `congruences` in the input. Calculation of modular inverse uses the extended Euclidean algorithm. The algorithm has a time complexity of `O(log b)` where `b` is the value of the modulus. Therefore, the overall time complexity of the algorithm is `O(n^2 * log b)`. The space complexity of the algorithm is `O(1)` as the provided inputs are not stored but rather used on the fly.