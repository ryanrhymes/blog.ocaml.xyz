---
layout: post
title:  "Selection Sort"
date:   2023-11-10 19:00:00 +0200
categories: algorithm
---


## Introduction  
Selection sort is a simple sorting algorithm that works by repeatedly finding the minimum element from an unsorted list and putting it at the beginning. The algorithm is not very efficient for large lists, but it has the advantage of being simple to understand and implement.  
   
## Implementation  
Here is an implementation of selection sort in OCaml:  
   
```ocaml  
let rec find_min i arr =  
  if i = Array.length arr then -1  
  else  
    let min_idx = find_min (i+1) arr in  
    if min_idx = -1 || arr.(i) < arr.(min_idx) then i else min_idx  
   
let selection_sort arr =  
  for i = 0 to Array.length arr - 1 do  
    let min_idx = find_min i arr in  
    let tmp = arr.(i) in  
    arr.(i) <- arr.(min_idx);  
    arr.(min_idx) <- tmp  
  done  
```  
   
Here, `find_min` is a helper function that finds the index of the minimum element in the array `arr` starting from index `i`. It returns `-1` if `i` is already at the end of the array. `selection_sort` uses `find_min` to find the minimum element in the unsorted portion of the array and swaps it with the first element in the unsorted portion. This process is repeated until the entire array is sorted.  
   
Here is an example of using `selection_sort`:  
   
```ocaml  
let arr = [|5; 2; 9; 3; 6|]
let () = selection_sort arr
let () = Array.iter (Printf.printf "%d ") arr
(* arr is now [|2; 3; 5; 6; 9|] *)  
```  
   
## Step-by-step Explanation  
1. We start with an unsorted array `arr`.  
2. We iterate over the array from index `0` to `n-1`, where `n` is the length of the array.  
3. For each iteration, we call `find_min` to find the index of the minimum element in the unsorted portion of the array starting from index `i`.  
4. We swap the element at index `i` with the minimum element found in step 3.  
5. We repeat steps 3-4 until the entire array is sorted.  
   
## Complexity Analysis  
The time complexity of selection sort is O(n^2), where n is the length of the array. This is because we have to iterate over the entire array n times, and for each iteration, we have to find the minimum element in the unsorted portion of the array, which takes O(n) time. The space complexity of selection sort is O(1), because we only need to store a few variables to keep track of the indices and the temporary value during the swapping process.   
  
Selection sort is not very efficient for large arrays, but it has the advantage of being simple to implement and understand. It can also be useful in situations where memory is limited, since it only requires a constant amount of extra memory.