---
layout: post
title:  "Balanced Brackets Problem"
date:   2023-11-22 12:00:00 +0200
categories: algorithm
---

## Introduction  
   
Balanced brackets is a problem that requires checking if a given string of brackets is balanced or not. A balanced string of brackets is one that has the same number of opening and closing brackets, and the brackets are properly nested. This problem has many applications, such as in compilers, text editors, and syntax checkers.  
   
## Implementation  

```ocaml
let generate_brackets n =
  let rec aux i acc =
    if i <= 0 then acc else
      aux (pred i) ('['::']'::acc)
  in
  let brk = aux n [] in
  List.sort (fun _ _ -> (Random.int 3) - 1) brk 

let is_balanced brk =
  let rec aux = function
    | [], 0 -> true
    | '['::brk, level -> aux (brk, succ level)
    | ']'::brk, 0 -> false
    | ']'::brk, level -> aux (brk, pred level)
    | _ -> assert false
  in
  aux (brk, 0)

let () =
  let n = int_of_string Sys.argv.(1) in
  Random.self_init();
  let brk = generate_brackets n in
  List.iter print_char brk;
  Printf.printf " %B\n" (is_balanced brk);
;;
```

The algorithm implemented here generates a random string of n brackets, either `[` or `]`, and then checks if it is balanced. The `generate_brackets` function generates a list of n brackets, alternating between `[` and `]`, and then shuffles the list randomly. The `is_balanced` function checks if the brackets are balanced by using a stack to keep track of the number of opening brackets seen so far. If a closing bracket is encountered while the stack is empty, or if the closing bracket does not match the most recent opening bracket, the string is not balanced.  
   
## Step-by-step Explanation  
   
1. The `generate_brackets` function takes an integer `n` as input and returns a list of `n` brackets.  
2. It uses a helper function `aux` that takes an integer `i` and a list `acc` as input, where `i` is the number of brackets left to generate and `acc` is the accumulator list that holds the generated brackets so far.  
3. If `i` is less than or equal to 0, the function returns the accumulator list.  
4. Otherwise, it calls `aux` recursively with `i-1` and `['['; ']'] @ acc`, which adds an opening and closing bracket to the accumulator list.  
5. The `generate_brackets` function then shuffles the list randomly using `List.sort` and a comparison function that randomly returns -1, 0, or 1.  
6. The `is_balanced` function takes a list of brackets `brk` as input and returns a boolean indicating if the brackets are balanced or not.  
7. It uses a helper function `aux` that takes a tuple `(brk, level)` as input, where `brk` is the remaining list of brackets and `level` is the number of opening brackets seen so far.  
8. If `brk` is empty and `level` is 0, the function returns true, indicating that the brackets are balanced.  
9. If the first character of `brk` is `[`, the function calls `aux` recursively with the tail of `brk` and `level+1`.  
10. If the first character of `brk` is `]`, the function checks if `level` is 0, indicating that there is no opening bracket to match the closing bracket. If `level` is not 0, the function calls `aux` recursively with the tail of `brk` and `level-1`.  
11. If the first character of `brk` is neither `[` nor `]`, the function raises an assertion error, since this should never happen.  
12. The `is_balanced` function calls `aux` with `(brk, 0)` as the initial tuple, since there are no opening brackets seen so far.  
13. The main function reads an integer `n` from the command line arguments, generates a random list of `n` brackets using `generate_brackets`, prints the brackets to the console, and then checks if they are balanced using `is_balanced`.  
   
## Complexity Analysis  
   
The time complexity of generating the list of brackets is O(n), since it calls the `aux` function recursively n times, and each call takes constant time to append two brackets to the accumulator list. The time complexity of shuffling the list is O(n log n), since it uses `List.sort` with a comparison function that takes constant time. The time complexity of checking if the brackets are balanced is O(n), since it scans the list once and uses a stack to keep track of the opening brackets seen so far. The space complexity of generating the list of brackets is O(n), since it creates a list of size n. The space complexity of checking if the brackets are balanced is also O(n), since it uses a stack that can hold up to n opening brackets.  
   
In summary, the overall time complexity of the algorithm is O(n log n), due to the sorting step, and the overall space complexity is O(n), due to the list and stack used.