---
layout: post
title:  "Anagrams Problem"
date:   2022-06-26 19:36:57 +0200
categories: algorithm
---

## Introduction
Anagrams problem is a classic Computer Science problem that belongs to the domain of string manipulation. An anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once. The task is to find groups of words that are anagrams of each other in a given dictionary. For example, the words 'icomania' and 'animacio' are anagrams of each other. This problem has applications in various fields, including natural language processing and cryptography. 

## Implementation

```ocaml

(** Anagrams Problem *)

let explode str =
  let l = ref [] in
  let n = String.length str in
  for i = n - 1 downto 0 do
    l := str.[i] :: !l
  done;
  (!l)

let implode li =
  let n = List.length li in
  let s = String.create n in
  let i = ref 0 in
  List.iter (fun c -> s.[!i] <- c; incr i) li;
  (s)

let () =
  let h = Hashtbl.create 3571 in
  let ic = open_in "unixdict.txt" in
  try while true do
    let w = input_line ic in
    let k = implode (List.sort compare (explode w)) in
    let l =
      try Hashtbl.find h k
      with Not_found -> [] 
    in
    Hashtbl.replace h k (w::l);
  done with End_of_file -> ();
  let n = Hashtbl.fold (fun _ lw n -> max n (List.length lw)) h 0 in
  Hashtbl.iter (fun _ lw ->
    if List.length lw >= n then
    ( List.iter (Printf.printf " %s") lw;
      print_newline () )
  ) h

```

The algorithm reads in a dictionary of words, creates an anagram key for each word by sorting its letters, then inserts each word in the dictionary into a hash table using its anagram as the key. Finally, it iterates through the keys of the hash table and prints each group of words that has the highest number of anagrams.

## Step-by-step Explanation
1. The `explode` function breaks down a given string into a list of its constituent characters by traversing the input string from its end to its beginning, adding each character to the output list as it traverses.
2. The `implode` function takes a list of characters and combines them to form a string.
3. The main program first creates an empty hash table, `h`.
4. Next, it opens the given dictionary file in read-only mode and reads the words one by one.
5. For each word, it sorts the letters using the `List.sort` and creates an anagram key for it using the `implode` function.
6. It then checks if the key is already present in `h`.
7. If the key is already present, it retrieves the corresponding list of words, adds the current word to the list, and replaces the value of the key in `h` with the updated list. Otherwise, it creates a new list containing the current word and inserts it into `h` using the key as the hash function.
8. After reading all the words, it calculates the maximum number of anagrams present for any given anagram key.
9. It then iterates through all the keys in `h`.
10. For each key, it retrieves the corresponding list of words and checks its length.
11. If the length of the list is greater than or equal to the maximum number of anagrams calculated earlier, it prints all the words in the list using `Printf.printf " %s"` and then calls `print_newline()` to move to the next line.

## Complexity Analysis
The time complexity of the algorithm is O(nk log k), where n is the number of words in the dictionary and k is the maximum length of any word in the dictionary. The reason for the time complexity is that for each word, we perform a sort operation on its letters, which takes O(k log k) time. Since we perform this operation n times, the total time complexity is O(nk log k).

The space complexity of the algorithm is also O(nk). The reason for the space complexity is that the hash table `h` can potentially store all n words in the dictionary, each requiring k space to store its anagram key. In addition, the program uses the `explode` and `implode` functions to convert between strings and lists of characters, which can also take up space.