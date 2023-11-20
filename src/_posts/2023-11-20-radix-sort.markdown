---
layout: post
title:  "Radix Sort"
date:   2023-11-20 23:15:00 +0200
categories: algorithm
---

## Introduction  
Radix sort is a sorting algorithm that sorts elements by grouping them based on their digits or bits. It is a non-comparative integer sorting algorithm that sorts data with integer keys by grouping the keys by individual digits that share the same significant position and value. Radix sort is often used for sorting large numbers of records, such as in big data applications.  
   
## Implementation  
Here is an implementation of radix sort in OCaml:  
   
```ocaml  
let radix_sort arr =  
  let rec loop arr exp =  
    if exp < 0 then arr else  
      let zeroes, ones = List.partition (fun x -> x land (1 lsl exp) = 0) arr in  
      loop zeroes (exp - 1) @ loop ones (exp - 1)  
  in  
  loop arr (List.fold_left (fun m x -> max m (int_of_float (log10 (float_of_int x)))) 0 arr)  
```  
   
Here, `radix_sort` takes in an array of integers `arr` and returns a sorted array. The `loop` function recursively sorts the array by partitioning it into two subarrays based on the value of the `exp`-th bit (starting from the most significant bit). The `exp` parameter is initially set to the maximum number of digits in any element of the array. The function `log10` is used to calculate the number of digits in each element.  
   
## Step-by-step Explanation  
1. Find the maximum number of digits in any element of the array.  
2. For each bit position starting from the most significant bit, partition the array into two subarrays based on the value of that bit.  
3. Recursively sort each subarray by repeating step 2 with the next bit position.  
4. Concatenate the sorted subarrays to get the final sorted array.  
   
For example, let's say we have the following array:  
   
```  
[170, 45, 75, 90, 802, 24, 2, 66]  
```  
   
The maximum number of digits is 3, so we start by partitioning the array based on the third bit:  
   
```  
[170, 90, 802, 2, 24, 45, 75, 66]  
```  
   
We then recursively sort each subarray by partitioning based on the second bit:  
   
```  
[802, 2, 24, 45, 66, 170, 75, 90]  
```  
   
```  
[2, 24, 45, 66, 75, 90, 170, 802]  
```  
   
Finally, we concatenate the sorted subarrays to get the final sorted array:  
   
```  
[2, 24, 45, 66, 75, 90, 170, 802]  
```  
   
## Complexity Analysis  
The time complexity of radix sort is O(d * (n + k)), where d is the maximum number of digits in any element of the array, n is the number of elements in the array, and k is the range of values (i.e., the maximum value minus the minimum value). The space complexity is O(n + k).  
   
In the worst case, when d is very large, radix sort can be slower than comparison-based sorting algorithms like quicksort or mergesort. However, radix sort has the advantage of being able to sort large integers and other non-comparable data types that cannot be easily sorted using comparison-based algorithms.