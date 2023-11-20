---
layout: post
title:  "Merge Sort"
date:   2023-11-20 23:01:00 +0200
categories: algorithm
---

## Introduction  
   
Merge sort is a divide-and-conquer algorithm that sorts an array or list of elements by recursively dividing it into two halves, sorting each half, and then merging the sorted halves back together. It is a highly efficient sorting algorithm with a time complexity of O(n log n), making it suitable for large datasets. Merge sort is widely used in various applications, including sorting large databases, numerical analysis, and parallel computing.  
   
## Implementation  
   
Here is an implementation of merge sort in OCaml:  
   
```ocaml  
let rec merge_sort = function  
  | [] -> []  
  | [x] -> [x]  
  | xs ->  
      let rec split left right = function  
        | [] -> (left, right)  
        | x :: xs -> split right (x :: left) xs  
      in  
      let rec merge left right = match left, right with  
        | [], _ -> right  
        | _, [] -> left  
        | x :: xs, y :: ys ->  
            if x < y then x :: merge xs right  
            else y :: merge left ys  
      in  
      let left, right = split [] [] xs in  
      merge (merge_sort left) (merge_sort right)  
```  
   
Here is an example of using this implementation to sort a list of integers:  
   
```ocaml  
let sorted_list = merge_sort [4; 2; 7; 1; 3; 6; 5];;  
```  
   
The resulting `sorted_list` will be `[1; 2; 3; 4; 5; 6; 7]`.  
   
## Step-by-step Explanation  
   
1. The `merge_sort` function takes a list of elements as input.  
2. If the list is empty or contains only one element, it is already sorted, so the function returns the list as is.  
3. Otherwise, the function recursively divides the list into two halves using the `split` function.  
4. The `split` function takes two empty lists (`left` and `right`) and the original list as input.  
5. If the original list is empty, the function returns the two half lists.  
6. Otherwise, the function takes the first element of the original list and adds it to the `right` list, while the rest of the list is recursively split with the `right` list becoming the new `left` list.  
7. The `merge` function takes two sorted lists (`left` and `right`) as input and returns a single sorted list.  
8. If one of the input lists is empty, the function returns the other list as is.  
9. Otherwise, the function compares the first elements of the two lists and adds the smaller one to the output list.  
10. The function then recursively calls itself with the remaining elements of the input lists until both input lists are empty.  
11. Finally, the `merge_sort` function merges the two sorted halves of the original list using the `merge` function.  
   
## Complexity Analysis  
   
The time complexity of merge sort is O(n log n), where n is the number of elements in the input list. This is because the algorithm recursively divides the input list into halves until each half contains only one element, which takes O(log n) time. Then, the algorithm merges the sorted halves back together, which takes O(n) time. Therefore, the total time complexity is O(n log n).  
   
The space complexity of merge sort is O(n), where n is the number of elements in the input list. This is because the algorithm creates temporary lists to store the halves of the input list during the merge process. However, these temporary lists are deallocated after each recursive call, so the overall space complexity is O(n).