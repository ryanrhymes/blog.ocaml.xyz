---
layout: post
title:  "Bubble Sort"
date:   2023-11-10 18:00:00 +0200
categories: algorithm
---

## Introduction  
Bubble Sort is a simple sorting algorithm that repeatedly swaps adjacent elements if they are in the wrong order. It is named as bubble sort because the smaller elements bubble up to the top of the list as the algorithm iterates through the list. Bubble Sort is not an efficient algorithm for large lists and is generally used for educational purposes.  
   
## Implementation  
Here is an implementation of Bubble Sort in OCaml:  
   
```ocaml
let bubble_sort arr =  
  let n = Array.length arr in  
  for i = 0 to n - 2 do  
    for j = 0 to n - i - 2 do  
      if arr.(j) > arr.(j+1) then  
        let temp = arr.(j) in  
        arr.(j) <- arr.(j+1);  
        arr.(j+1) <- temp  
    done  
  done;;  
```  
   
Here, `arr` is the array to be sorted. The function `bubble_sort` uses two nested loops to iterate through the array and compare adjacent elements. If the elements are in the wrong order, they are swapped. The outer loop iterates from 0 to n-2, while the inner loop iterates from 0 to n-i-2. This is because the largest element in the list will already be in its correct position after each iteration of the outer loop.  
   
## Step-by-step Explanation  
1. Start with an unsorted array of n elements.  
2. Compare the first two elements. If the first element is greater than the second element, swap them.  
3. Move to the next pair of adjacent elements and repeat step 2 until the end of the array is reached.  
4. Repeat steps 2 and 3 for each pair of adjacent elements until the end of the array is reached.  
5. After each iteration, the largest element will be in its correct position at the end of the array.  
6. Repeat steps 2-5 n-1 times to sort the entire array.  
   
## Complexity Analysis  
The time complexity of Bubble Sort is O(n^2), where n is the number of elements in the array. This is because the algorithm uses two nested loops to iterate through the array and compare adjacent elements. In the worst case scenario, where the array is in reverse order, the algorithm will need to make n*(n-1)/2 comparisons and swaps. The space complexity of Bubble Sort is O(1), as the algorithm only requires a constant amount of additional memory to store temporary variables for swapping elements.