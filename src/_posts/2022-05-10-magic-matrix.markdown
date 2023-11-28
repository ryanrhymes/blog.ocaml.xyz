---
layout: post
title:  "Magic Matrix"
date:   2022-05-10 06:18:03 +0200
categories: algorithm
---

## Introduction
In the field of recreational mathematics, the Magic Square is a popular puzzle. A magic square is a square grid filled with numbers such that every row, column, and diagonal have the same sum. The concept is not entirely new; it has been around for centuries. The puzzle was popularized by Albrecht Dürer, a German artist in the Renaissance period. The algorithm 'magic' generates a magic square of a given order. 

## Implementation

```ocaml

(* Magic Matrix *)

let magic k n =
  let error () =
    let s = Printf.sprintf "magic requires n >= 3, whereas input n = %i" n in
    Owl_exception.INVALID_ARGUMENT s
  in
  Owl_exception.verify (n >= 3) error;
  let a0 = Owl_const.zero k in
  let a1 = Owl_const.one k in
  let _magic_odd n a =
    let x = zeros k n n in
    let i = ref 0 in
    let j = ref (n / 2) in
    let ac = ref (float_of_int a |> _float_typ_elt k) in
    let m = (n * n) + a - 1 |> float_of_int |> _float_typ_elt k in
    while !ac <= m do
      if get x !i !j = a0
      then (
        set x !i !j !ac;
        ac := _add_elt k !ac a1)
      else (
        let i' = (!i - 1 + n) mod n in
        let j' = (!j + 1) mod n in
        if get x i' j' = a0
        then (
          i := i';
          j := j')
        else i := (!i + 1) mod n)
    done;
    x
  in
  let _magic_doubly_even n =
    let x = zeros k n n in
    let _seq_inc x i0 j0 i1 j1 =
      for i = i0 to i1 do
        let ac = ref ((n * i) + j0 + 1 |> float_of_int |> _float_typ_elt k) in
        for j = j0 to j1 do
          set x i j !ac;
          ac := _add_elt k !ac a1
        done
      done
    in
    let _seq_dec x i0 j0 i1 j1 =
      let ac = ref (n * n |> float_of_int |> _float_typ_elt k) in
      for i = i0 to i1 do
        for j = j0 to j1 do
          if get x i j = a0 then set x i j !ac;
          ac := _sub_elt k !ac a1
        done
      done
    in
    let m = n / 4 in
    _seq_inc x 0 0 (m - 1) (m - 1);
    _seq_inc x 0 (3 * m) (m - 1) ((4 * m) - 1);
    _seq_inc x m m ((3 * m) - 1) ((3 * m) - 1);
    _seq_inc x (3 * m) 0 ((4 * m) - 1) (m - 1);
    _seq_inc x (3 * m) (3 * m) ((4 * m) - 1) ((4 * m) - 1);
    _seq_dec x 0 0 (n - 1) (n - 1);
    x
  in
  let _magic_singly_even n =
    let m = n / 2 in
    let m2 = m * m in
    let xa = _magic_odd m 1 in
    let xb = _magic_odd m (m2 + 1) in
    let xc = _magic_odd m ((2 * m2) + 1) in
    let xd = _magic_odd m ((3 * m2) + 1) in
    let m3 = m / 2 in
    let xa' =
      concat_horizontal
        (get_slice [ []; [ 0; m3 - 1 ] ] xd)
        (get_slice [ []; [ m3; -1 ] ] xa)
    in
    let xd' =
      concat_horizontal
        (get_slice [ []; [ 0; m3 - 1 ] ] xa)
        (get_slice [ []; [ m3; -1 ] ] xd)
    in
    let xb' =
      if m3 - 1 = 0
      then xb
      else
        concat_horizontal
          (get_slice [ []; [ 0; m - m3 ] ] xb)
          (get_slice [ []; [ 1 - m3; -1 ] ] xc)
    in
    let xc' =
      if m3 - 1 = 0
      then xc
      else
        concat_horizontal
          (get_slice [ []; [ 0; m - m3 ] ] xc)
          (get_slice [ []; [ 1 - m3; -1 ] ] xb)
    in
    set xa' m3 0 (get xa m3 0);
    set xa' m3 m3 (get xd m3 m3);
    set xd' m3 0 (get xd m3 0);
    set xd' m3 m3 (get xa m3 m3);
    concat_horizontal (concat_vertical xa' xd') (concat_vertical xc' xb')
  in
  (* n is odd *)
  if Owl_maths.is_odd n
  then _magic_odd n 1 (* n is doubly even *)
  else if n mod 4 = 0
  then _magic_doubly_even n (* n is singly even *)
  else _magic_singly_even n

```

The implementation has a function named "magic", which generates a magic square of a given order and it has two parameters. The first is the precision level, k, which is used to define the data type of the entries in the magic square. The second parameter is the order of the magic square.


## Step-by-step Explanation

This algorithm follows four steps to generate a magic square of any order (n).

### 1. Verification of Validity
The function first checks the validity of input parameter n. The minimum valid value of n is three. 

### 2. Determination of Magic Square Type
The algorithm proceeds to determine the type of magic square required. There are three types of magic squares:
* Odd-order: This type of magic square occurs when n is odd.
* Singly Even-order: This type of magic square occurs when n is even, but not divisible by 4.
* Doubly Even-order: This type of magic square occurs when n is divisible by 4.

### 3. Determination of Entries 
The third step involves determining the entries of the magic square by computing the columns, rows, and diagonals. The magic square matrix is called x. The implementation uses the following three functions to compute the magic square.
#### i. _magic_odd 
This function is called if n is an odd integer. This function computes an odd-ordered magic square. The algorithm begins from the top-middle cell of the magic square matrix. Next, the algorithm calculates the next cell using the rules specified for generating odd-order magic squares. The calculation involves moving one row up and one column right. If the calculated cell is not already filled, the next element in the sequence is filled in. If the cell is already occupied, the algorithm then moves one row down and repeats the process. 
#### ii. _magic_singly_even 
This function is called if n is an even integer and has a remainder of 2 when divided by 4. This function generates a singly even-ordered magic square. A singly even-ordered magic square of order n is built from four magic squares of order n/2. The sub-squares are arranged in a rotated position, and the diagonals of the sub-squares are filled in the larger square to obtain the required magic square.
#### iii. _magic_doubly_even
This function is called if n is an even integer that is divisible by 4.  This function generates a doubly even-ordered magic square. The sub-squares of a doubly even-ordered magic square are filled using the formula X[n-j+1][n-i+1] = k2 - X[i][j] + 1.  

### 4. Returning the Magic Square
The final step involves returning the magic square created in step 3.

## Complexity Analysis
The time complexity of this algorithm is O(n^2). The space complexity of the implementation is the memory used to store the nxn magic square matrix, which is O(n^2). However, the precision level k used can also affect the space complexity. 