---
layout: post
title:  "Distinct power numbers"
date:   2021-11-11 04:55:51 +0200
categories: algorithm
---

## Introduction:
This algorithm aims to find the distinct power numbers for a certain range of integers. A power number of an integer is the number obtained by raising it to the power of another integer. For example, 4 is a power number of 2 because 2 raised to the power of 2 equals 4. This algorithm uses a mathematical formula to calculate power numbers and then puts them in a set to eliminate duplicates.

## Implementation

```ocaml

(* Distinct power numbers *)

module IntSet = Set.Make(Int)

let pow x =
  let rec aux acc b = function
    | 0 -> acc
    | y -> aux (if y land 1 = 0 then acc else acc * b) (b * b) (y lsr 1)
  in
  aux 1 x

let distinct_powers first count =
  let sq = Seq.(take count (ints first)) in
    IntSet.of_seq (Seq.map_product pow sq sq)

let () = distinct_powers 2 4
  (* output *)
  |> IntSet.to_seq |> Seq.map string_of_int
  |> List.of_seq |> String.concat " " |> print_endline

```
:
The algorithm defines a function called `distinct_powers` which takes two arguments: `first` and `count`. `first` is the first number in the range of integers for which we want to find the power numbers, and `count` is the number of integers in that range. The function then generates this range of integers using the `Seq` module and applies the `pow` function to every element. `pow` function takes an integer and calculates its power number using a mathematical formula. Finally, the `IntSet` module is used to convert the sequence of power numbers into a set of distinct power numbers.

## Step-by-step Explanation:
1. Define a function named `pow` that takes an integer and returns its power number.
2. Calculate the power number of the input integer using the `aux` function, which does the following:
    1. Initialize an accumulator variable called `acc` to 1 and a base variable called `b` to the input integer.
    2. If the input integer is 0, return the accumulator variable.
    3. If the input integer is not 0:
        1. Check if the integer is odd by looking at its least significant bit. If it is odd, multiply the accumulator by the base.
        2. Multiply the base variable by itself.
        3. Shift the input integer to the right by 1 bit.
        4. Repeat step 3 until the input integer becomes 0.
3. Define a function named `distinct_powers` that takes two arguments: `first` and `count`.
4. Generate a sequence of integers using the `Seq` module's `take` function with `count` as the parameter and the `ints` function with `first` as the parameter.
5. Apply the `pow` function to every element in the sequence using the `Seq.map_product` function.
6. Convert the resulting sequence of power numbers to a set of distinct power numbers using the `IntSet.of_seq` function.

## Complexity Analysis:
The time complexity of the algorithm is O(n * log n), where n is the range of integers for which we want to find the power numbers. The `Seq.take` function takes O(n) time to generate the sequence of integers, and the `Seq.map_product` function takes O(n * log n) time to apply the `pow` function to each element in the sequence. The `IntSet.of_seq` function takes O(n * log n) time to convert the resulting sequence to a set of distinct power numbers. Therefore, the overall time complexity of the algorithm is O(n * log n). The space complexity of the algorithm is also O(n * log n) because it needs to store the sequence of power numbers, which takes O(n * log n) space, and the set of distinct power numbers, which takes O(n * log n) space.