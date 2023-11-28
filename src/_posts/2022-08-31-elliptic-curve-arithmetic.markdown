---
layout: post
title:  "Elliptic curve arithmetic"
date:   2022-08-31 10:41:29 +0200
categories: algorithm
---

## Introduction
Elliptic curve cryptography is a form of public-key cryptography that is often used in secure web applications. The algorithm based on elliptic curves has been shown to be more secure than conventional cryptosystems like RSA. The elliptic curve addition operation can be defined as "Bring a line through two points, the third intersection with the curve is equal to the inverse of the sum and counts as the result." This algorithm is important in adding two points on the elliptic curve, doubling a point on the elliptic curve, and multiplying a point on the elliptic curve by a scalar.

## Implementation

```ocaml

(* Elliptic curve arithmetic *)

(*  
    Using the secp256k1 elliptic curve (a=0, b=7),
    define the addition operation on points on the curve.
    Extra credit:   define the full elliptic curve arithmetic 
    (still not modular, though) by defining a "multiply" function.
*)

(*** Helpers ***)

type ec_point = Point of float * float | Inf

type ec_curve = { a : float; b : float }

(* By default, cube root doesn't work for negative bases *)
let cube_root : float -> float = 
    let third = 1. /. 3. in
    let f x = 
        if x > 0.
        then x ** third
        else ~-. (~-. x ** third)
    in
    f

(* Finds the left-most x on this curve *)
let ec_minx ({a; b} : ec_curve) : float =
    let factor = ~-. b *. 0.5 in
    let discr = (factor ** 2.) +. (a ** 3. /. 27.) in
    if discr <= 0.
    then failwith "Not a simple curve"
    else
    let root = sqrt discr in
    cube_root (factor +. root) +. cube_root (factor -. root)

(* Negates the point by negating y coord *)
let ec_neg : ec_point -> ec_point = function
    | Inf -> Inf
    | Point (x, y) -> Point (x, ~-. y)

(*** Actual task at hand ***)

(* Generates a random point in the vicinity of x=0 *)
let ec_random ({a; b} as c : ec_curve) : ec_point =
    let minx = ec_minx c in
    let x = Random.float (~-. minx *. 2.) +. minx in
    let rhs = x ** 3. +. a *. x +. b in
    Point (x, sqrt rhs)

(*  Verifies that the point is on curve. 
    Due to rounding errors, sometimes these calculations aren't perfect.
*)
let on_curve ?(debug : bool = false) ({a; b} : ec_curve) : ec_point -> bool = function
    | Inf -> true
    | Point (x, y) -> 
        let lhs = y *. y in
        let rhs = x ** 3. +. a *. x +. b in
        let delta = abs_float (lhs -. rhs) in
        (
            if debug then Printf.printf "Delta = %.8f" delta;
            delta < 0.000001
        )

(* Doubles a point on the curve (adds a point to itself) *)
let ec_double ({a; b} as c : ec_curve) : ec_point -> ec_point = function
    | Inf -> Inf
    | Point (x, y) as p -> 
        if not (on_curve c p)
        then failwith "Point not on this curve."
        else if y = 0. 
        then Inf
        else
        let s = (3. *. x *. x +. a) /. (2. *. y) in
        let x' = s *. s -. 2. *. x in
        let y' = y +. s *. (x' -. x) in
        Point (x', -. y')

(* Adds any two points on the curve *)
let ec_add ({a; b} as c : ec_curve) (p : ec_point) (q : ec_point) : ec_point =
    match p, q with
    | Inf, x | x, Inf -> x
    | Point (px, py), Point (qx, qy) ->
        if not (on_curve c p) || not (on_curve c q)
        then failwith "Point not on this curve."
        else if abs_float (px -. qx) < 0.000001 then
        begin
            if abs_float (py +. qy) < 0.000001
            then Inf
            else 
            (* py must equal qy here, otherwise something goes real bad *)
            ec_double c p |> ec_neg
        end
        else
        let s = (py -. qy) /. (px -. qx) in
        let rx = s *. s -. px -. qx in
        let ry = py +. s *. (rx -. px) in
        Point (rx, -. ry)

(* Extra credit : multiplies a point by a scalar *)
let ec_mul ({a; b} as c : ec_curve) (p : ec_point) (n : int) : ec_point = 
    let rec helper n curPow acc =
        if n = 0 then acc
        else 
        let doubled = ec_double c curPow in
        if n mod 2 = 0 
        then helper (n / 2) doubled acc
        else helper (n / 2) doubled (ec_add c acc curPow)
    in
    helper n p Inf

(*** Output ***)

let string_of_point : ec_point -> string = function
    | Inf -> "Zero"
    | Point (x, y) -> Printf.sprintf "(%.4f, %.4f)" x y

let print_output () = 
    let c = { a = 0.; b = 7. } in
    let p = ec_random c in
    let q = ec_random c in
    let r = ec_add c p q in
    let t = ec_neg r in
    Printf.printf "p = %s
" (string_of_point p);
    Printf.printf "q = %s
" (string_of_point q);
    Printf.printf "r = p + q = %s
" (string_of_point r);
    Printf.printf "t = -r = %s
" (string_of_point t);
    Printf.printf "r + t = %s
" (ec_add c r t |> string_of_point);
    Printf.printf "p + (q + t) = %s
" (ec_add c q t |> ec_add c p |> string_of_point);
    Printf.printf "p * 12345 = %s
" (ec_mul c p 12345 |> string_of_point)

let _ =
    print_output ();
    print_output ()

```

This algorithm defines addition, doubling, and multiplication of points on the secp256k1 elliptic curve (a=0, b=7), which is the curve used in Bitcoin and other cryptocurrencies.

## Step-by-step Explanation
1. Firstly, two points are given. These points can be generated using the `ec_random(c)` function. This function will return a random point on the elliptic curve, where c represents the curve `c = {a; b}`, and `a` and `b` are defined.
2. Next, if any of the given points is the infinity point `Inf`, it returns the other point as the result.
3. After checking both points are on the curve, doubling operation will be performed on the given point `p` or `q`, if `y = 0`. Doubling involves a relatively simple formula, which can be expressed as `3x^2 + A / 2y`.
4. Otherwise, points will be added using the formula `(py - qy) / (px - qx)`.
5. Finally, similarly, a multiplication function will be performed on a point and a scalar value `n`. It will double the point `n` times by utilizing `helper` recursive function which will return the accumulated value.

## Complexity Analysis
- The `ec_minx` function runs in `O(1)` time complexity as there are no loops or recursive calls in it.
- The `cube_root` function runs in `O(1)` time complexity as it involves only mathematical computations.
- The `on_curve` function runs in `O(1)` time complexity as it needs to check only a few mathematical expressions.
- The `ec_random` function runs in `O(1)` time complexity as it involves only the generation of random floating-point numbers and simple mathematical expressions.
- The `ec_mul` function has a time complexity of `O(log n)`. As it is based on the recursive helper function, recursion requires `O(log2n)` iterations.
- The `helper` function itself runs in `O(log n)` time since each iteration of the recursion requires a square root calculation.
- The `ec_add` and `ec_double` functions also run in `O(1)` time complexity as they involve only a few mathematical expressions and a small number of checks.
- Therefore, the overall time complexity of the provided algorithm is `O(log n)` and considered very efficient.