---
layout: post
title:  "Selection Sort"
date:   2023-11-10 23:00:00 +0200
categories: algorithm
---

## Introduction  
Shell sort is an efficient sorting algorithm that is a variation of insertion sort. It was invented by Donald Shell in 1959. The algorithm works by sorting elements that are far apart from each other and then gradually reducing the gap between them. This makes the algorithm faster than insertion sort for larger lists.  
   
## Implementation  
Here is an implementation of Shell sort in OCaml:  
   
```ocaml  
let shell_sort arr =  
  let n = Array.length arr in  
  let gap = ref (n / 2) in  
  while !gap > 0 do  
    for i = !gap to n - 1 do  
      let temp = arr.(i) in  
      let j = ref i in  
      while !j >= !gap && arr.(!j - !gap) > temp do  
        arr.(!j) <- arr.(!j - !gap);  
        j := !j - !gap  
      done;  
      arr.(!j) <- temp  
    done;  
    gap := !gap / 2  
  done;  
  arr  
```  
   
To use this function, simply pass in an array of elements to sort:  
   
```ocaml  
let arr = [| 5; 3; 8; 4; 2 |]  
let sorted_arr = shell_sort arr  
```  
   
The `sorted_arr` variable will contain the sorted array.  
   
## Step-by-step Explanation  
1. Start by dividing the list into sublists of equal intervals. The interval is called the gap.   
2. Sort each sublist using insertion sort.  
3. Reduce the gap by half and repeat step 2 until the gap is 1.  
   
For example, let's say we have the following list of integers: `[5, 3, 8, 4, 2]`. We'll use a gap of 2 for the first pass.  
   
1. Divide the list into sublists with a gap of 2: `[5, 8, 2]` and `[3, 4]`.  
2. Sort each sublist using insertion sort. The first sublist becomes `[2, 5, 8]` and the second sublist stays the same: `[3, 4]`.  
3. Reduce the gap to 1 and sort the entire list using insertion sort. The sorted list is `[2, 3, 4, 5, 8]`.  
   
## Complexity Analysis  
The time complexity of Shell sort depends on the gap sequence used. The worst-case time complexity is O(n^2), which occurs when the gap sequence is 1, making the algorithm equivalent to insertion sort. However, the average-case time complexity is O(n log n), which is faster than insertion sort for larger lists.  
   
The space complexity of Shell sort is O(1) because it sorts the list in place.