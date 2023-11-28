---
layout: post
title:  "Shuffle Algorithm"
date:   2022-06-18 08:03:56 +0200
categories: algorithm
---

## Introduction
The shuffle algorithm is a commonly used algorithm that rearranges elements in an array. The algorithm shuffles the array in a random order. It is widely used in various applications ranging from computer game development to randomizing survey questions.

## Implementation

```ocaml

(* Shuffle Algorithm *)

let shuffle x =
  let y = Array.copy x in
  let n = Array.length x in
  for i = n - 1 downto 1 do
    let s = float_of_int (i + 1) in
    let j = int_of_float (std_uniform_rvs () *. s) in
    Owl_utils_array.swap y i j
  done;
  y

```

The algorithm takes an array x as input and returns an array shuffled randomly. The algorithm uses a for loop to iterate over the array and swap each element at a given index i with another random element at index j. 

## Step-by-step Explanation
1. Copy the array x and store it in array y.
2. Find the length of the array n.
3. Start a for loop, starting at index n-1 and ending at index 1.
4. For each iteration, generate a random float s between 1 and i+1.
5. Compute j as the integer value of s.
6. Swap the elements at positions i and j in array y.
7. End the for loop and return the shuffled array y.

## Complexity Analysis
The shuffle algorithm has a time complexity of O(n), where n is the length of the input array. This is because the algorithm only requires one pass through the array. The complexity of each iteration of the loop is constant time. Therefore, the algorithm has a linear time complexity. The space complexity of the shuffle algorithm is O(n), where n is the length of the input array. This is because the algorithm creates a copy of the input array to prevent modifying the original array. The space complexity of the copy operation is proportional to the size of the input array. Therefore, the shuffle algorithm has a linear space complexity.