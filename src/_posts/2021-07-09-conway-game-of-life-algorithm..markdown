---
layout: post
title:  "Conway Game of Life algorithm."
date:   2021-07-09 17:39:15 +0200
categories: algorithm
---

## Introduction

The Conway Game of Life is a simulation of a cell population. It was developed by the mathematician John Horton Conway in 1970. This simulation is a classic example of a cellular automaton. 

The algorithm for the Conway Game of Life is widely known because it has been adopted and used in various applications. For instance, it has been employed to simulate the spread of a virus, the evolution of a bacterial colony, the behavior of an ant colony, and the movements of crowds.

## Implementation

```ocaml

(* Conway Game of Life *)

let get g x y =
  try g.(x).(y)
  with _ -> 0

let neighbourhood g x y =
  (get g (x-1) (y-1)) +
  (get g (x-1) (y  )) +
  (get g (x-1) (y+1)) +
  (get g (x  ) (y-1)) +
  (get g (x  ) (y+1)) +
  (get g (x+1) (y-1)) +
  (get g (x+1) (y  )) +
  (get g (x+1) (y+1)) 

let next_cell g x y =
  let n = neighbourhood g x y in
  match g.(x).(y), n with
  | 1, 0 | 1, 1                      -> 0  (* lonely *)
  | 1, 4 | 1, 5 | 1, 6 | 1, 7 | 1, 8 -> 0  (* overcrowded *)
  | 1, 2 | 1, 3                      -> 1  (* lives *)
  | 0, 3                             -> 1  (* get birth *)
  | _ (* 0, (0|1|2|4|5|6|7|8) *)     -> 0  (* barren *)

let copy g = Array.map Array.copy g

let next g =
  let width = Array.length g
  and height = Array.length g.(0)
  and new_g = copy g in
  for x = 0 to pred width do
    for y = 0 to pred height do
      new_g.(x).(y) <- (next_cell g x y)
    done
  done;
  (new_g)

let print g =
  let width = Array.length g
  and height = Array.length g.(0) in
  for x = 0 to pred width do
    for y = 0 to pred height do
      if g.(x).(y) = 0
      then print_char '.'
      else print_char 'o'
    done;
    print_newline ()
  done

```


The algorithm defines the following functions:
- `get`: A helper function that returns the value of a cell within a two-dimensional array. The arguments `x` and `y` represent the indices of the location of a cell.
- `neighbourhood`: A function that calculates the total number of living neighbors around a cell whose location is defined by the arguments `x` and `y`.
- `next_cell`: A function that determines the state of a cell based on its current state and the total number of living neighbors. This function applies the rules of the Conway Game of Life.
- `copy`: A function that creates and returns a copy of a two-dimensional array.
- `next`: A function that advances the simulation by one generation. This function returns a new two-dimensional array containing the next generation of cells.
- `print`: A function that prints the state of the cell population to the console.

## Step-by-step Explanation

1. `get` defines a helper function that takes a two-dimensional array `g` and two indices `x` and `y` and returns the value of the cell at position `(x, y)` in the array, or 0 if it is outside the array bounds.
2. `neighbourhood` calculates the total number of living neighbors around a cell whose location is defined by the arguments `x` and `y`. It does this by summing the values of the eight cells immediately surrounding the given cell, obtained through the `get` function.
3. `next_cell` determines the state of a cell in the next generation of the simulation, applying the rules of the Conway Game of Life. It takes two arguments, a two-dimensional array `g` and two indices `x` and `y`, corresponding to the location of the cell in question. It calculates the number of living neighbors using the `neighbourhood` function, and returns the updated state of the cell in the next generation.
4. `copy` creates a new two-dimensional array `new_g` that is a copy of the input two-dimensional array `g`, by using the `Array.map` function to apply the `Array.copy` function to each row of `g`.
5. `next` applies the `next_cell` function to each cell of the two-dimensional array `g`, creating a new two-dimensional array `new_g` that represents the state of the cell population in the next generation. It does this by looping over each element in the array and setting the corresponding value in `new_g` to the value returned by `next_cell`.
6. `print` takes a two-dimensional array `g` representing the cell population and prints its state to the console. It does this by printing a single character for each cell, representing whether it is alive or dead.

## Complexity Analysis

- `get`: This function is a constant-time operation. It just checks if the indices are within bounds and returns the value of a specific position in the two-dimensional array. Thus, the complexity of the function is O(1).
- `neighbourhood`: This function calculates the sum of the eight neighboring cells' values. It does this by accessing eight cells, which is a constant-time operation. Thus the complexity of the function is O(1).
- `next_cell`: This function implements the set of rules that dictate whether the cell should change its state, based on the value of its neighbors and its current state. The function only performs a constant number of operations - at most, two if statements and one simple arithmetic addition. As a result, its time complexity is also O(1).
- `copy`: This function iterates through the input two-dimensional array, and creates a copy of every element using the `Array.copy` method. Thus, the function has a time complexity of O(nm), where n and m are the dimensions of the input array.
- `next`: This function iterates over every element in the input two-dimensional array and sets the corresponding value in the new array by calling `next_cell`. This involves iterating over every row of the array `g`, and then iterating over every element of each row. Thus, the time complexity of this function is O(nm) where n and m are the dimensions of the input arrays.
- `print`: This function iterates through every element in the input two-dimensional array and then prints a corresponding string to the console. It also has a time complexity of O(nm) where n and m are the dimensions of the input arrays.