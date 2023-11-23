---
layout: post
title:  "Bucket Sort"
date:   2023-11-10 11:00:00 +0200
categories: algorithm
---

## Introduction  
Bucket sort is a sorting algorithm that works by distributing the elements of an array into a number of buckets. Each bucket is then sorted individually, either using a different sorting algorithm or by recursively applying the bucket sorting algorithm. Bucket sort is useful when the input is uniformly distributed over a range. It is commonly used in computer graphics, computational biology, and data mining.  
   
## Implementation  
Here is an implementation of bucket sort in OCaml:  
   
```ocaml
let bucket_sort arr =  
  let n = Array.length arr in  
  let buckets = Array.make n [] in  
  for i = 0 to n - 1 do  
    let bucket_index = int_of_float (arr.(i) *. (float_of_int n)) in  
    buckets.(bucket_index) <- arr.(i) :: buckets.(bucket_index)  
  done;  
  let sorted = ref [] in  
  for i = 0 to n - 1 do  
    buckets.(i) <- List.sort compare buckets.(i);  
    sorted := !sorted @ buckets.(i)  
  done;  
  Array.of_list !sorted  
```  
   
This implementation takes an array of floating-point numbers as input and returns a sorted array.  
   
## Step-by-step Explanation  
1. The input array is first traversed to determine the number of buckets required. This is done by taking the length of the array and creating an empty bucket for each index.  
```ocaml
let n = Array.length arr in  
let buckets = Array.make n [] in  
```  
   
2. The elements of the array are then distributed into the buckets. This is done by iterating over the array and calculating the index of the bucket for each element. The index is calculated by multiplying the element by the number of buckets and taking the integer part of the result.  
```ocaml
for i = 0 to n - 1 do  
  let bucket_index = int_of_float (arr.(i) *. (float_of_int n)) in  
  buckets.(bucket_index) <- arr.(i) :: buckets.(bucket_index)  
done;  
```  
   
3. Each bucket is then sorted individually. This is done using the OCaml `List.sort` function, which sorts a list in ascending order using the `compare` function.  
```ocaml
for i = 0 to n - 1 do  
  buckets.(i) <- List.sort compare buckets.(i);  
done;  
```  
   
4. Finally, the sorted elements are concatenated to form the final sorted array.  
```ocaml
let sorted = ref [] in  
for i = 0 to n - 1 do  
  sorted := !sorted @ buckets.(i)  
done;  
Array.of_list !sorted  
```  
   
## Complexity Analysis  
The time complexity of bucket sort depends on the sorting algorithm used to sort the individual buckets. In the worst case, if each bucket contains all the elements of the input array, the time complexity of bucket sort is O(n^2), where n is the number of elements in the input array. However, if the input is uniformly distributed over a range, the time complexity can be improved to O(n) by using a linear-time sorting algorithm like counting sort or radix sort to sort the individual buckets.  
   
The space complexity of bucket sort is O(n), where n is the number of elements in the input array. This is because we need to create a bucket for each element in the input array. However, if the input is uniformly distributed over a range, the space complexity can be improved to O(k), where k is the number of buckets required, which is much smaller than n.