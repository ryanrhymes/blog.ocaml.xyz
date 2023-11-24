---
layout: post
title:  "Depth-First Search"
date:   2023-11-21 03:00:00 +0200
categories: algorithm
---

## Introduction  
The Fisher-Yates shuffle, also known as the Knuth shuffle, is an algorithm used to randomly shuffle a collection of elements. It was first described by Ronald Fisher and Frank Yates in 1938 and later popularized by Donald Knuth in his book "The Art of Computer Programming". The Fisher-Yates shuffle is widely used in computer science, particularly in applications such as games, music playlists, and cryptography.  
   
## Implementation  
Here is an implementation of the Fisher-Yates shuffle in OCaml:  
   
```ocaml  
let fisher_yates_shuffle arr =  
  let n = Array.length arr in  
  for i = n - 1 downto 1 do  
    let j = Random.int (i + 1) in  
    let tmp = arr.(j) in  
    arr.(j) <- arr.(i);  
    arr.(i) <- tmp  
  done;  
  arr  
```  

This function takes an array of elements and returns a shuffled version of the array. It works by iterating through the array from the last element to the first. For each element, it picks a random index between 0 and the current index, inclusive, and swaps the element at that index with the current element.  

Here is an example.

```ocaml
let deck = [| "Ace of Spades"; "2 of Spades"; "3 of Spades"; "King of Diamonds" |]  
let shuffled_deck = fisher_yates_shuffle deck  
let () = Array.iter (fun x -> print_endline x) shuffled_deck  
```

## Step-by-step Explanation  
1. Start by initializing a variable `n` to the length of the input array.  
2. Iterate through the array from the last element to the first using a for loop. For each element at index `i`:  
3. Generate a random integer `j` between 0 and `i` inclusive using the `Random.int` function.  
4. Swap the element at index `j` with the element at index `i`.  
5. Continue iterating until all elements have been shuffled.  
6. Return the shuffled array.  
   
## Complexity Analysis  
The time complexity of the Fisher-Yates shuffle is O(n), where n is the number of elements in the input array. This is because the algorithm iterates through the array once and performs a constant amount of work for each element.  
   
The space complexity of the Fisher-Yates shuffle is O(1), because the algorithm shuffles the input array in place and does not require any additional memory allocation.  
   
Overall, the Fisher-Yates shuffle is a simple and efficient algorithm for shuffling collections of elements.