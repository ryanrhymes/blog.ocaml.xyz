---
layout: post
title:  "Church numerals"
date:   2021-09-12 04:24:42 +0200
categories: algorithm
---

## Introduction
The Church numeral is a technique used to represent natural numbers in arithmetic formulas using functional programming. Church numerals are named after the logician Alonzo Church, who extensively used them in his work. The Church numerals involve encoding natural numbers as functions that can be applied to other functions and values using functional programming. It is used in the field of type theory and computer science to illustrate that a computational language can be simple and expressive. 

## Implementation

```ocaml

(* Church numerals *)
(*  Using type as suggested in https://stackoverflow.com/questions/43426709/does-ocamls-type-system-prevent-it-from-modeling-church-numerals 
    This is an explicitly polymorphic type : it says that f must be of type ('a -> 'a) -> 'a -> 'a for any possible a "at same time".
*)
type church_num = { f : 'a. ('a -> 'a) -> 'a -> 'a }

(* Zero means apply f 0 times to x, aka return x *)
let ch_zero : church_num = { f = fun _ -> fun x -> x }

(* One simplifies to just returning the function *)
let ch_one : church_num = { f = fun fn -> fn }

(* The next numeral of a church numeral would apply f one more time *)
let ch_succ (c : church_num) : church_num = { f = fun fn x -> fn (c.f fn x) }

(* Adding m and n is applying f m times and then also n times *)
let ch_add  (m : church_num) (n : church_num) : church_num =
    { f = fun fn x -> n.f fn (m.f fn x) }

(* Multiplying is repeated addition : add n, m times *)
let ch_mul  (m : church_num) (n : church_num) : church_num =
    { f = fun fn x -> m.f (n.f fn) x }

(*  Exp is repeated multiplication : multiply by base, exp times.
    However, Church numeral n is in some sense the n'th power of a function f applied to x
    So exp base = apply function base to the exp'th power = base^exp.
*)
let ch_exp (base : church_num) (exp : church_num) : church_num =
    { f = fun fn x -> (exp.f base.f) fn x }

(* extended Church functions: *)

(* test function for church zero *)
let ch_is_zero (c : church_num) : church_num =
    { f = fun fn x -> c.f (fun _ -> fun _ -> fun xi -> xi) (* when argument is not ch_zero *)
                          (fun fi -> fi) (* when argument is ch_zero *) fn x }

(* church predecessor function; reduces function calls by one unless already church zero *)
let ch_pred (c : church_num) : church_num =
    { f = fun fn x -> c.f (fun g h -> h (g fn)) (fun _ -> x) (fun xi -> xi) }

(* church subtraction function; calls predecessor function second argument times on first *)
let ch_sub (m : church_num) (n : church_num) : church_num = n.f ch_pred m

(* church division function; counts number of times divisor can be recursively
   subtracted from dividend *)
let ch_div (dvdnd : church_num) (dvsr : church_num) : church_num =
  let rec divr n = (fun v -> v.f (fun _ -> (ch_succ (divr v)))
                                  ch_zero) (ch_sub n dvsr)
  in divr (ch_succ dvdnd)

(* conversion functions: *)

(* Convert a number to a church_num via recursion *)
let church_of_int (n : int) : church_num = 
    if n < 0 
    then raise (Invalid_argument (string_of_int n ^ " is not a natural number"))
    else 
    (* Tail-recursed helper *)
    let rec helper n acc = 
        if n = 0 
        then acc
        else helper (n-1) (ch_succ acc)
    in helper n ch_zero

(*  Convert a church_num to an int is rather easy! Just +1 n times. *)
let int_of_church (n : church_num) : int = n.f succ 0

(* Now the tasks at hand: *)

(* Derive Church numerals three, four, eleven, and twelve,
   in terms of Church zero and a Church successor function *)

let ch_three = church_of_int 3
let ch_four = ch_three |> ch_succ
let ch_eleven = church_of_int 11
let ch_twelve = ch_eleven |> ch_succ

(* Use Church numeral arithmetic to obtain the the sum and the product of Church 3 and Church 4 *)
let ch_7 = ch_add ch_three ch_four
let ch_12 = ch_mul ch_three ch_four

(* Similarly obtain 4^3 and 3^4 in terms of Church numerals, using a Church numeral exponentiation function *)
let ch_64 = ch_exp ch_four ch_three
let ch_81 = ch_exp ch_three ch_four

(* check that ch_is_zero works *)
let ch_1 = ch_is_zero ch_zero
let ch_0 = ch_is_zero ch_three

(* check church predecessor, subtraction, and division, functions work *)
let ch_2 = ch_pred ch_three
let ch_8 = ch_sub ch_eleven ch_three
let ch_3 = ch_div ch_eleven ch_three
let ch_4 = ch_div ch_twelve ch_three

(* Convert each result back to an integer, and return it as a string *)
let result = List.map (fun c -> string_of_int(int_of_church c)) 
                      [ ch_three; ch_four; ch_7; ch_12; ch_64; ch_81;
                        ch_eleven; ch_twelve; ch_1; ch_0; ch_2; ch_8; ch_3; ch_4 ]
             |> String.concat "; " |> Printf.sprintf "[ %s ]"

;;

print_endline result

```

This algorithm implements Church numerals using the functional programming language, OCaml. The algorithm defines the basic Church numerals (zero and one), addition, multiplication, exponential, and extended features such as testing for zeros, subtracting, dividing, and converting between the Church numerals and integers. 

## Step-by-step Explanation
1. Define the Church numerals data structure as a type in OCaml
2. Define Church numerals zero and one
3. Define the sucession function, `ch_succ`, which takes a Church numeral and returns a numeral one greater.
4. Define addition and multiplication of Church numerals as two separate functions. For addition, add the numbers by applying one numeral to another. For multiplication, apply a Church numeral as many times as specified by another numeral.
5. Define exponentiation (ch_exp) as a multiplication of numbers where the base Church numeral applied to the power of another Church numeral.
6. Define additional Church numeral features such as testing for zeros (ch_is_zero), predecessor (ch_pred), subtraction (ch_sub), and division (ch_div).
7. Define conversion numbers to and from Church numerals, `church_of_int` and `int_of_church`, using recursion.
8. Apply the Church numeral and features to obtain numerical results and convert them back to integers as required.

## Complexity Analysis
The complexity of this Church numeral implementation is high, as it requires the computation of function compositions to obtain exponentiation, multiplication, and addition. The worst-case time complexity of addition and multiplication is O(n), where n is the value of the greater numeral. For example, if multiplying 3 by 4, it will take four total applications of the succession function to build one Church numeral, then it will iterate three times to produce the resulting Church numeral. Thus the time complexity of the multiplication of 3 and 4 Church numerals is 4 * 3 operations on `ch_succ`. For exponentiation, the worst-case time complexity is O(m*n), where m and n represent the two Church numerals used as arguments. It takes m applications of the base function (which is passed over n times) to raise the base by the value of the exponent. The time complexity of testing for zeros, predecessor, subtraction, and division is all O(n), with n being the value of the Church numeral. The space complexity for each operations is O(1) since each operation solely relies on function calls.