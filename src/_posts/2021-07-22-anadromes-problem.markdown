---
layout: post
title:  "Anadromes Problem"
date:   2021-07-22 06:18:42 +0200
categories: algorithm
---

## Introduction
An anadrome is a string that forms another word when read in reverse. For example, "top" and "pot" are anadromes of each other. This algorithm takes a set of strings and returns a set of all the pairs of anadromes in the input set.

## Implementation

```ocaml

(** Anadromes Problem *)

module StrSet = Set.Make(String)

let read_line_seq ch =
  let rec repeat () =
    match input_line ch with
    | s -> Seq.Cons (s, repeat)
    | exception End_of_file -> Nil
  in repeat

let string_rev s =
  let last = pred (String.length s) in
  String.init (succ last) (fun i -> s.[last - i])

let get_anadromes set =
  let aux s =
    let r = string_rev s in
    if s < r && StrSet.mem r set
    then Some (s, r)
    else None
  in
  Seq.filter_map aux (StrSet.to_seq set)

let () = read_line_seq stdin |> Seq.filter (fun s -> String.length s > 6)
  |> Seq.map String.lowercase_ascii |> StrSet.of_seq |> get_anadromes
  |> Seq.iter (fun (s, r) -> Printf.printf "%9s | %s
" s r)

```

The algorithm reads lines from standard input until end-of-file is reached. It filters out lines that are too short to have anadromes (less than 7 characters), transforms all the letters to lowercase using `String.lowercase_ascii`, and stores the resulting strings in a `StrSet`. Then it calls `get_anadromes` on the set, which returns a sequence of pairs of anadromes. Finally, it prints each pair as a line with its two elements separated by a vertical bar.

## Step-by-step Explanation
1. Define a module `StrSet` that provides a set implementation for strings.
2. Define a function `read_line_seq` that returns a sequence of lines read from a channel until end-of-file is reached. Use `Seq.Cons` to build the sequence recursively, and a `match` expression to handle end-of-file as an exception.
3. Define a function `string_rev` that takes a string `s` and returns its reverse. Use `String.length` to find the index of the last character, `String.init` to initialize a new string with the same length as `s`, and a lambda function to fill each position with the corresponding character from `s`.
4. Define a function `get_anadromes` that takes a set `set` of strings and returns a sequence of all the pairs of anadromes in `set`. Use `Seq.filter_map` to apply the auxiliary function `aux` to each element of `set` and keep only the ones that return `Some` (i.e., anadromes).
5. Define an auxiliary function `aux` that takes a string `s` and checks whether its reverse is in `set`. If it is, return `Some (s, r)` where `r` is the reverse of `s`. If it isn't, return `None`.
6. In the main program, read lines from standard input using `read_line_seq`, filter out the ones that are too short using `Seq.filter`, transform them to lowercase using `Seq.map` and `String.lowercase_ascii`, build a `StrSet` with `StrSet.of_seq`, call `get_anadromes` on it, and iterate over the resulting sequence with `Seq.iter`, printing each pair as a line with `Printf.printf`.

## Complexity Analysis
Let `n` be the number of strings in the input set, and `m` be the maximum length of a string.

The time complexity of `String.length` is O(1), the time complexity of `String.init` is O(m), and the time complexity of the lambda function is O(1), so the time complexity of `string_rev` is O(m).

The time complexity of `StrSet.mem` is logarithmic in the size of the set, so the time complexity of `aux` is O(m log n).

The time complexity of `Seq.filter_map` is proportional to the size of the sequence, which is at most `n`, and the time complexity of `get_anadromes` is proportional to the number of anadrome pairs in the set, which is at most `n/2`, so the time complexity of the main algorithm is O(nm log n).

The space complexity of the algorithm is dominated by the `StrSet` data structure, which stores up to `n` strings of up to length `m`, so the space complexity is O(nm). The other data structures used (sequences, pairs, variables) have constant size.