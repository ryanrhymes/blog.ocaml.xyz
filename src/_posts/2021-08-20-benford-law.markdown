---
layout: post
title:  "Benford Law"
date:   2021-08-20 01:07:08 +0200
categories: algorithm
---

## Introduction

The Benford Law is an algorithm to measure the likelihood distribution of first digits in a set of numbers. This law predicts that in many naturally occurring sets of numbers, such as populations, stock prices, or scientific data, the first digit is more likely to be 1 than any other digit. Also, the following digits have decreassing probability to be the leading digit. 

## Implementation

```ocaml

(* Benford Law *)

open Num

let fib =
  let rec fib_aux f0 f1 = function
    | 0 -> f0
    | 1 -> f1
    | n -> fib_aux f1 (f1 +/ f0) (n - 1)
  in
  fib_aux (num_of_int 0) (num_of_int 1) ;;

let create_fibo_string = function n -> string_of_num (fib n) ;;
let rec range i j = if i > j then [] else i :: (range (i + 1) j)

let n_max = 1000 ;;

let numbers = range 1 n_max in
  let get_first_digit = function s -> Char.escaped (String.get s 0) in
    let first_digits = List.map get_first_digit (List.map create_fibo_string numbers) in
  let data = Array.create 9 0 in
    let fill_data vec = function n -> vec.(n - 1) <- vec.(n - 1) + 1 in
    List.iter (fill_data data) (List.map int_of_string first_digits) ;
    Printf.printf "
Frequency of the first digits in the Fibonacci sequence:
" ;
    Array.iter (Printf.printf "%f ")
      (Array.map (fun x -> (float x) /. float (n_max)) data) ;

let xvalues = range 1 9 in
  let benfords_law = function x -> log10 (1.0 +. 1.0 /. float (x)) in
    Printf.printf "
Prediction of Benford's law:
 " ;
    List.iter (Printf.printf "%f ") (List.map benfords_law xvalues) ;
    Printf.printf "
" ;;

```


The code is written in OCaml. The algorithm is implemented by generating the first n Fibonacci numbers, and then calculating the first digit of each number. We then count the frequency of each digit from 1 to 9. Finally, we compare the obtained frequency values with their expected values according to Benford's law. 

## Step-by-Step Explanation

1. Define the Fibonacci function using two integers and a value of n.
2. Create a string from the n-th number in the Fibonacci sequence.
3. Create a list of numbers in the range (1, 1000).
4. Extract the first digits of a given number using a custom function.
5. Map all the numbers in the input list to their first digits.
6. Create an array of size 9.
7. Iterate over the list of first digits and increment the corresponding index in the array.
8. Print the frequency of the first digits in the Fibonacci sequence.
9. Create a list of xvalues using the range function.
10. Define Benford's law and map the xvalues to the expected values.
11. Print out the Benford's law prediction.

## Complexity Analysis

The time complexity of this implementation is O(n), where "n" is the maximum number of Fibonacci numbers to be generated. Here, n_max = 1000. The space complexity of this program is O(1), as only a fixed-size array is used to store the frequency counts of the first digits.

Overall, this implementation is efficient and should work well for small values of "n_max". However, for larger values of "n_max", the time complexity can increase significantly, and the program may become computationally expensive.