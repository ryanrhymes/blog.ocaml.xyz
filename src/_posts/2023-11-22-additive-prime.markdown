---
layout: post
title:  "Additive Prime Algorithm"
date:   2023-11-22 08:00:00 +0200
categories: algorithm
---

   
## Introduction  
Additive prime is a prime number whose digits add up to another prime number. For example, 23 is an additive prime because 2 + 3 = 5, and both 23 and 5 are prime numbers. This algorithm checks whether a given number is an additive prime.  
   
## Implementation  
The algorithm is implemented in OCaml. The function `digit_sum` calculates the sum of digits of a given number. The function `is_prime` checks whether a given number is prime. The function `is_additive_prime` checks whether a given number is an additive prime by checking if the number and its digit sum are both prime.  
   
```ocaml  
let rec digit_sum n =
  if n < 10 then n else n mod 10 + digit_sum (n / 10)

let is_prime n =
  let rec test x =
    let q = n / x in x > q || x * q <> n && n mod (x + 2) <> 0 && test (x + 6)
  in if n < 5 then n lor 1 = 3 else n land 1 <> 0 && n mod 3 <> 0 && test 5

let is_additive_prime n =
  is_prime n && is_prime (digit_sum n)
```  
   
Here is an example.

```ocaml
let () =
  Seq.ints 0 |> Seq.take_while ((>) 500) |> Seq.filter is_additive_prime
  |> Seq.iter (Printf.printf " %u") |> print_newline
```

## Step-by-step Explanation  
1. The function `digit_sum` takes a positive integer `n` as input.  
2. If `n` is less than 10, then `n` is returned as the sum of digits of a single-digit number is the number itself.  
3. If `n` is greater than or equal to 10, then the last digit of `n` is added to the sum of digits of the remaining digits of `n` by recursively calling the `digit_sum` function.  
4. The function `is_prime` takes a positive integer `n` as input.  
5. If `n` is less than 5, then `n` is prime if and only if it is equal to 2 or 3.  
6. If `n` is greater than or equal to 5, then the function `test` is called with an initial value of 5.  
7. The function `test` takes a positive integer `x` as input and checks if `n` is divisible by `x` or `x + 2`, or if `n` is divisible by any number of the form `x + 6k` where `k` is a non-negative integer less than or equal to `(n / x - x) / 6`.  
8. If `x` is greater than `(n / x - x) / 6`, then `n` is prime.  
9. The function `is_additive_prime` takes a positive integer `n` as input.  
10. If `n` is not prime, then `is_additive_prime` returns `false`.  
11. If `n` is prime, then the function `digit_sum` is called with `n` as input.  
12. If the digit sum of `n` is not prime, then `is_additive_prime` returns `false`.  
13. If both `n` and its digit sum are prime, then `is_additive_prime` returns `true`.  
   
## Complexity Analysis  
The time complexity of the `digit_sum` function is O(log n) as the number of digits in `n` is proportional to log n. The time complexity of the `is_prime` function is O(sqrt n) as it checks divisibility up to the square root of `n`. The worst-case time complexity of the `is_additive_prime` function is O(sqrt n log n) as it checks the primality of `n` and its digit sum, which each have a worst-case time complexity of O(sqrt n). The space complexity of all