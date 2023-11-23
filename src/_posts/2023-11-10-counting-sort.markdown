---
layout: post
title:  "Counting Sort"
date:   2023-11-10 20:00:00 +0200
categories: algorithm
---

## Introduction  
Counting sort is an efficient sorting algorithm that can be used to sort elements in a range of integers. It is a non-comparison based sorting algorithm that works by counting the number of occurrences of each element in the array and then using this information to determine the position of each element in the sorted output array.  
   
## Implementation  
Here is an implementation of counting sort in OCaml:  
   
```ocaml  
let counting_sort arr =
  let max_val = Array.fold_left (fun acc x -> max acc x) arr.(0) arr in
  let count_arr = Array.make (max_val + 1) 0 in
  Array.iter (fun x -> count_arr.(x) <- count_arr.(x) + 1) arr;
  let index_arr = Array.make (max_val + 1) 0 in
  let rec fill_index_arr i j =
    if i <= max_val then
      begin
        index_arr.(i) <- j;
        fill_index_arr (i + 1) (j + count_arr.(i))
      end
  in
  fill_index_arr 0 0;
  let sorted_arr = Array.make (Array.length arr) 0 in
  Array.iteri (fun i x -> sorted_arr.(index_arr.(x)) <- x; index_arr.(x) <- index_arr.(x) + 1) arr;
  sorted_arr
```  
   
Here, `arr` is the input array to be sorted. The function `counting_sort` first finds the maximum value in the input array and creates a new array `count_arr` to count the number of occurrences of each element in the input array. It then creates another array `index_arr` to store the index of each element in the sorted output array. The function `fill_index_arr` is a helper function that fills in the `index_arr` array based on the counts in `count_arr`. Finally, the function creates a new array `sorted_arr` and fills it in by iterating over the input array and using the `index_arr` array to determine the position of each element in the sorted output array.  

```ocaml
let arr = [|5; 2; 9; 3; 6|]
let sorted_arr = counting_sort arr
let () = Array.iter (Printf.printf "%d ") sorted_arr
```


## Step-by-step Explanation  
1. Find the maximum value in the input array.  
2. Create a new array `count_arr` of length `max_val + 1` and initialize all elements to 0.  
3. Iterate over the input array and increment the count of each element in `count_arr`.  
4. Create a new array `index_arr` of length `max_val + 1` and initialize all elements to 0.  
5. Define a helper function `fill_index_arr` that fills in the `index_arr` array based on the counts in `count_arr`. The function takes two arguments: `i` and `j`, where `i` is the current index in `count_arr` and `j` is the current index in `index_arr`.  
6. Call the `fill_index_arr` function with initial arguments `0` and `0`.  
7. Create a new array `sorted_arr` of length `n`, where `n` is the length of the input array.  
8. Iterate over the input array and use the `index_arr` array to determine the position of each element in the sorted output array. Store each element in the corresponding position in `sorted_arr`.  
9. Return `sorted_arr`.  
   
## Complexity Analysis  
The time complexity of counting sort is O(n + k), where n is the length of the input array and k is the range of the input integers. The space complexity is also O(n + k).   
  
The algorithm iterates over the input array twice: once to count the occurrences of each element and once to fill in the sorted output array. The time complexity of these two iterations is O(n). The algorithm also creates two additional arrays: `count_arr` and `index_arr`, each of length k. Therefore, the space complexity of the algorithm is O(n + k).
