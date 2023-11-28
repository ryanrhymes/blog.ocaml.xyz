---
layout: post
title:  "Catalan numbers"
date:   2021-04-07 17:27:33 +0200
categories: algorithm
---

## Introduction
The Catalan numbers are a sequence of natural numbers that have important applications in several fields such as combinatorics, algebra, geometry and several other branches of mathematics. This sequence is named after Eugène Charles Catalan, a Belgian mathematician. Catalan numbers describe the number of ways to perform several tasks such as arranging brackets in expressions, forming triangulations from non-convex polygons, arranging ball sequences, etc.

## Implementation

```ocaml

(* Catalan numbers *)

let imp_catalan n =
  let return = ref 1 in
  for i = 1 to n do
    return := !return * 2 * (2 * i - 1) / (i + 1)
  done;
  !return

let rec catalan = function
  | 0 -> 1
  | n -> catalan (n - 1) * 2 * (2 * n - 1) / (n + 1)

let memoize f =
  let cache = Hashtbl.create 20 in
  fun n ->
    match Hashtbl.find_opt cache n with
    | None ->
      let x = f n in
      Hashtbl.replace cache n x;
      x
    | Some x -> x

let catalan_cache = Hashtbl.create 20

let rec memo_catalan n =
  if n = 0 then 1 else
    match Hashtbl.find_opt catalan_cache n with
    | None ->
      let x = memo_catalan (n - 1) * 2 * (2 * n - 1) / (n + 1) in
      Hashtbl.replace catalan_cache n x;
      x
    | Some x -> x

let () =
  if not !Sys.interactive then
    let bench label f n times =
      let start = Unix.gettimeofday () in
      begin
        for i = 1 to times do f n done;
        let stop = Unix.gettimeofday () in
        Printf.printf "%s (%d runs) : %.3f
"
          label times (stop -. start)
      end in
    let show f g h f' n =
      for i = 0 to n do
        Printf.printf "%2d %7d %7d %7d %7d
"
          i (f i) (g i) (h i) (f' i)
      done
    in
    List.iter (fun (l, f) -> bench l f 15 10_000_000)
      ["imperative", imp_catalan;
       "recursive", catalan;
       "hand-memoized", memo_catalan;
       "memoized", (memoize catalan)];
    show imp_catalan catalan memo_catalan (memoize catalan) 15

```

The algorithm calculates the Catalan number using four different implementation methods:
- `imp_catalan` calculates Catalan numbers using imperative programming
- `catalan` calculates Catalan numbers using a recursive algorithm
- `memoize` constructs a memoized function
- `memo_catalan` calculates Catalan numbers by memoization using the `memoize` function constructed above

## Step-by-Step Explanation
1. In `imp_catalan`, a reference value `return` is initialized to 1.
2. A for-loop runs from 1 to `n`.
3. For each iteration, `return` is updated as follows:
     - multiplied by `2`
     - multiplied by `2i-1` (where `i` is the current iteration number)
     - divided by `i+2`
4. Finally, `!return` (i.e., the value in the reference `return`) is returned.

1. In `catalan`, the function takes an integer argument `n`.
2. If `n` is zero, the function returns one, else it calculates and returns the Catalan number recursively as follows:
     - call `catalan` with `(n-1)` as argument
     - multiply the result of the recursive call by `2`
     - multiply it by `2n-1`
     - divide it by `n+1`
     
1. In `memoize`, the function takes as input another function `f`. 
2. The result is a new function that caches the output of `f` for a given input `n`.
3. First, a `cache` hash table is created to hold the function values.
4. The new function takes as input an argument `n`, and first checks if the value for this input is already in the cache.
5. If it is not in the cache, the original function `f` is applied to `n`, and the result is stored in the cache.
6. Finally, the function returns the value either found in the cache or computed by `f`.

1. In `memo_catalan`, the function takes the argument `n`. 
2. If the input is `0`, the function returns `1`.
3. Otherwise, it checks if the value is already in the cache. 
4. If the value is not in the cache, calculate the Catalan number recursively using `(n-1)`, multiplied by `2`, multiplied by `2n-1`, and divided by `n+1`.
5. The calculated value is then stored in the cache and returned.

1. `show` function prints out a table of Catalan numbers for the first `n` integers, computing the results using various methods.

## Complexity Analysis
- The time complexity of `imp_catalan` is O(n) since the for-loop runs for n iterations.
- The time complexity of `catalan` is O(2^n) since it solves the problem recursively.
- The time complexity of `memo_catalan` is O(n) since it computes `n` values recursively before reusing previously memoized solutions.
- The time complexity of `memoize` is O(n) since the memoization table holds a maximum of `n` values.
- The space complexity of `imp_catalan`, `catalan`, `memoize`, and `memo_catalan` is O(1) as they require a constant amount of memory to compute each Catalan number.