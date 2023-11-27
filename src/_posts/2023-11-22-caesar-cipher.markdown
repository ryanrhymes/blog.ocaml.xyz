---
layout: post
title:  "Caesar Cipher"
date:   2023-11-22 14:00:00 +0200
categories: algorithm
---

## Introduction  
The Caesar cipher is one of the simplest and most widely known encryption techniques. It is a type of substitution cipher in which each letter in the plaintext is shifted a certain number of places down the alphabet. For example, with a shift of 1, A would be replaced by B, B would become C, and so on. The method is apparently named after Julius Caesar, who used it to communicate with his officials.   
  
## Implementation  
The Caesar cipher is implemented in OCaml as follows:  
   
```ocaml  
let islower c =
  c >= 'a' && c <= 'z'

let isupper c =
  c >= 'A' && c <= 'Z'

let rot x str =
  let upchars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
  and lowchars = "abcdefghijklmnopqrstuvwxyz" in
  let rec decal x =
    if x < 0 then decal (x + 26) else x
  in
  let x = (decal x) mod 26 in
  let decal_up = x - (int_of_char 'A')
  and decal_low = x - (int_of_char 'a') in
  String.map (fun c ->
    if islower c then
      let j = ((int_of_char c) + decal_low) mod 26 in
      lowchars.[j]
    else if isupper c then
      let j = ((int_of_char c) + decal_up) mod 26 in
      upchars.[j]
    else
      c
  ) str
```  
   
The `rot` function takes an integer `x` and a string `str` as arguments. The integer `x` represents the number of positions to shift each letter in the string `str`. The function returns a new string that is the result of applying the Caesar cipher with the given shift.  

Here is how to use the function.

```ocaml
let () =
  let key = 3 in
  let orig = "The five boxing wizards jump quickly" in
  let enciphered = rot key orig in
  print_endline enciphered;
  let deciphered = rot (- key) enciphered in
  print_endline deciphered;
  Printf.printf "equal: %b\n" (orig = deciphered)
;;
```


## Step-by-Step Explanation  
1. The `islower` function checks if a given character is a lowercase letter.  
2. The `isupper` function checks if a given character is an uppercase letter.  
3. The `rot` function initializes two strings: `upchars` containing all uppercase letters and `lowchars` containing all lowercase letters.  
4. The `decal` function takes an integer `x` and returns the result of decrementing `x` by 26 until it is non-negative.  
5. The `x` variable is assigned the result of applying `decal` to `x` and then taking the modulus of 26.  
6. The `decal_up` variable is assigned the value of `x` minus the integer value of the character 'A'.  
7. The `decal_low` variable is assigned the value of `x` minus the integer value of the character 'a'.  
8. The `String.map` function applies a function to each character in the string `str`.  
9. For each character `c` in the string `str`, the function checks if it is a lowercase letter using the `islower` function.  
10. If `c` is a lowercase letter, the function calculates a new index `j` by adding the integer value of `c` to `decal_low` and taking the modulus of 26. The new character is then obtained by taking the `j`-th character of the `lowchars` string.  
11. If `c` is an uppercase letter, the function calculates a new index `j` by adding the integer value of `c` to `decal_up` and taking the modulus of 26. The new character is then obtained by taking the `j`-th character of the `upchars` string.  
12. If `c` is not a letter, the function returns `c` unchanged.  
13. The function returns the resulting string.  
   
## Complexity Analysis  
The time complexity of the Caesar cipher algorithm is O(n), where `n` is the length of the input string `str`. This is because the `String.map` function applies a function to each character in the string `str`, and this operation takes O(1) time per character. Therefore, the overall time complexity is O(n).  
   
The space complexity of the algorithm is also O(n), since the resulting string has the same length as the input string.  
   
In terms of security, the Caesar cipher is very weak and easy to break, since there are only 26 possible shifts and an attacker can easily try all of them. Therefore, it is not suitable for use in modern cryptography, but it can still be used for simple purposes such as obfuscation or educational purposes.