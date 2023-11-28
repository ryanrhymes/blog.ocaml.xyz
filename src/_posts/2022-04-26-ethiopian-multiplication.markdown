---
layout: post
title:  "Ethiopian multiplication"
date:   2022-04-26 08:36:22 +0200
categories: algorithm
---

## Introduction
Ethiopian multiplication, also known as Ethiopian algorithm, is a technique for multiplying two integers. This technique is based on the principle of halving and doubling numbers. The algorithm originated from ancient Egypt and Ethiopia, where it was used to perform multiplications without using multiplication tables or calculators. Ethiopians used this method to perform fast and efficient calculations without the need for writing anything down.

## Implementation

```ocaml

(* Ethiopian multiplication *)

(* We optimize a bit by not keeping the intermediate lists, and summing
   the right column on-the-fly, like in the C version.
   The function takes "halve" and "double" operators and "is_even" predicate as arguments,
   but also "is_zero", "zero" and "add". This allows for more general uses of the
   ethiopian multiplication. *)
let ethiopian is_zero is_even halve zero double add b a =
  let rec g a b r =
    if is_zero a
    then (r)
    else g (halve a) (double b) (if not (is_even a) then (add b r) else (r))
  in
  g a b zero
;;

let imul =
  ethiopian (( = ) 0) (fun x -> x mod 2 = 0) (fun x -> x / 2) 0 (( * ) 2) ( + );;

imul 17 34;;
(* - : int = 578 *)

(* Now, we have implemented the same algorithm as "rapid exponentiation",
   merely changing operator names *)
let ipow =
  ethiopian (( = ) 0) (fun x -> x mod 2 = 0) (fun x -> x / 2) 1 (fun x -> x*x) ( * )
;;

ipow 2 16;;
(* - : int = 65536 *)

(* still renaming operators, if "halving" is just subtracting one,
   and "doubling", adding one, then we get an addition *)
let iadd a b =
  ethiopian (( = ) 0) (fun x -> false) (pred) b (function x -> x) (fun x y -> succ y) 0 a
;;

iadd 421 1000;;
(* - : int = 1421 *)

(* One can do much more with "ethiopian multiplication",
   since the two "multiplicands" and the result may be of three different types,
   as shown by the typing system of ocaml *)

ethiopian;;
- : ('a -> bool) ->          (* is_zero *)
    ('a -> bool) ->          (* is_even *)
    ('a -> 'a) ->            (* halve *)
    'b ->                    (* zero *)
    ('c -> 'c) ->            (* double *)
    ('c -> 'b -> 'b) ->      (* add *)
    'c ->                    (* b *)
    'a ->                    (* a *)
    'b                       (* result *)
= <fun>

(* Here zero is the starting value for the accumulator of the sums
   of values in the right column in the original algorithm. But the "add"
   me do something else, see for example the RosettaCode page on 
   "Exponentiation operator". *)

```

The algorithm takes two integers as input and returns their product. It uses `halve`, `double`, and `is_even` operators as arguments to perform the multiplication operation. The `halve` operator divides a number by 2, the `double` operator multiplies a number by 2, and the `is_even` predicate checks if the given number is even or odd. It also takes `add`, `zero`, and `is_zero` as arguments to perform accumulator operations.

## Step-by-step Explanation
1. Take two numbers a and b as input.
2. Check if the value of a is zero. If it is, return the accumulator value.
3. Divide a by 2 using the halve operator and multiply b by 2 using the double operator.
4. If the value of a is odd, add b to the accumulator value using the add operator.
5. Repeat steps 2 to 4 until the value of a becomes zero.
6. Return the accumulator value as the product of a and b.

## Complexity Analysis
The time complexity of the Ethiopian multiplication algorithm is O(log n), where n is the value of the larger integer. This is because at each step, the value of a is halved, which leads to a reduction in the number of steps exponentially. The space complexity of the algorithm is O(1), as it uses only a few variables to store intermediate calculations and an accumulator value to store the final result. Overall, the Ethiopian multiplication algorithm is a very efficient way of multiplying two integers, especially for larger values.