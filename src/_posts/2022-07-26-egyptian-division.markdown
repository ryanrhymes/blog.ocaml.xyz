---
layout: post
title:  "Egyptian Division"
date:   2022-07-26 16:17:36 +0200
categories: algorithm
---

# Introduction

The Egyptian division algorithm is used to perform integer division in a simple and efficient way, specifically for large numbers. It is also known as the “division by repeated subtraction” algorithm. This algorithm is based on the idea of finding the largest multiples of the divisor that can be subtracted from the dividend and repeating the process until the result is achieved. 

# Implementation

The egypt_div function in OCaml takes two integer parameters: x (the dividend) and y (the divisor). It returns the quotient of x divided by y using the Egyptian division algorithm. 

# Step-by-Step Explanation

1. The function creates a helper function called table. It takes three parameters, p (2^0), d (1), and an empty list called lst. 

2. The table function uses a conditional statement to determine whether or not d is greater than x (the dividend). 

3. If d is greater than x, the function returns the list.

4. If d is not greater than x, the function recursively calls itself with two new parameters: p * 2 (doubling p) and d * 2 (doubling d), as well as a new element added to the list (p, d).

5. After generating the list of multiples of the divisor, the function then creates a new helper function called consider. 

6. The consider function takes two parameters, q and a, and a tuple called (p, d).

7. Consider then checks to see if multiplying p by q and adding d to a is greater than x (the dividend). 

8. If the sum is greater than the dividend, then q and a are returned as the quotient and remainder. 

9. If the sum is less than or equal to the dividend, the process is repeated recursively by calling consider with updated parameters: q+p and a+d.

10. The list.fold_left function is then applied to the result of the table function and the initial value of (0,0). 

11. The list.fold_left function takes the consider function as its first argument, and (0,0) as its second argument.

12. The result is the quotient of the division. 

# Complexity Analysis

The time complexity of the egypt_div function is O(log n) where n is the dividend. This is because the table function generates a list of at most log(n) pairs of (p,d) before the conditional statement returns.

The space complexity of the function is O(log n) as well since the table function generates a list of at most log(n) pairs of (p,d).  

In the consider function, each iteration of the while loop reduces the difference between its parameter and the dividend by a factor of two. This reduces the maximum number of iterations from 2d (worst case) to log(d) (best case). Therefore, the worst-case time complexity of the consider function is O(d) while the best-case time complexity is O(log d). 