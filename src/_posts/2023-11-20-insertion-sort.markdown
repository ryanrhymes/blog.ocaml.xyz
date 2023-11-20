---
layout: post
title:  "Insertion Sort"
date:   2023-11-20 23:15:00 +0200
categories: algorithm
---

## Introduction  
Insertion sort is a simple sorting algorithm that is widely used in computer science. It works by taking one element from the unsorted list and inserting it into the correct position in the sorted list. The algorithm is efficient for small data sets but becomes inefficient for larger ones. Insertion sort can be used in situations where the data is already partially sorted or where the cost of swapping elements is high.  
   
## Implementation  
Here is an implementation of insertion sort in OCaml:  
   
```ocaml  
let rec insertion_sort = function  
  | [] -> []  
  | x :: xs -> insert x (insertion_sort xs)  
and insert x = function  
  | [] -> [x]  
  | y :: ys -> if x < y then x :: y :: ys else y :: insert x ys  
```  
   
This implementation uses a recursive function to sort the list. The `insertion_sort` function takes a list as input and returns a sorted list. The `insert` function takes an element `x` and a sorted list as input and returns a new sorted list with `x` inserted in the correct position.  
   
## Step-by-step Explanation  
1. The `insertion_sort` function is called with a list as input.  
2. If the list is empty, an empty list is returned.  
3. Otherwise, the first element `x` is removed from the list and the `insert` function is called with `x` and the sorted list returned by a recursive call to `insertion_sort`.  
4. The `insert` function takes `x` and a sorted list as input.  
5. If the list is empty, a list with `x` as the only element is returned.  
6. Otherwise, the first element `y` is removed from the list and the `insert` function is called recursively with `x` and the remaining list `ys`.  
7. If `x` is less than `y`, a new list with `x` and `y` followed by `ys` is returned.  
8. Otherwise, a new list with `y` followed by the result of calling `insert` with `x` and `ys` is returned.  
9. The sorted list returned by `insert` is returned by `insertion_sort`.  
   
## Complexity Analysis  
The time complexity of insertion sort is O(n^2) in the worst case, where n is the number of elements in the list. This is because each element must be compared to every other element in the list, resulting in n^2 comparisons. However, in the best case (when the list is already sorted), the time complexity is O(n) because each element only needs to be compared to the previous element. The space complexity of insertion sort is O(1) because the algorithm sorts the list in place without using any additional memory.