---
layout: post
title:  "Red Black Tree"
date:   2021-01-01 18:00:00 +0200
categories: algorithm
---

## Introduction  
Red-black tree is a self-balancing binary search tree. It was invented in 1972 by Rudolf Bayer who called them "symmetric binary B-trees," but acquired its modern name in a paper in 1978 by Leo J. Guibas and Robert Sedgewick. Red-black trees are often used in C++ STL (Standard Template Library) implementation of map and set.  
   
## Implementation  
Here is an implementation of the red-black tree in OCaml:  
   
```ocaml  
type color = Red | Black  
   
type 'a tree =  
  | Leaf  
  | Node of color * 'a tree * 'a * 'a tree  
   
let balance = function  
  | Black, Node (Red, Node (Red, a, x, b), y, c), z, d  
  | Black, Node (Red, a, x, Node (Red, b, y, c)), z, d  
  | Black, a, x, Node (Red, Node (Red, b, y, c), z, d)  
  | Black, a, x, Node (Red, b, y, Node (Red, c, z, d)) ->  
      Node (Red, Node (Black, a, x, b), y, Node (Black, c, z, d))  
  | color, left, value, right -> Node (color, left, value, right)  
   
let rec insert x = function  
  | Leaf -> Node (Red, Leaf, x, Leaf)  
  | Node (color, left, value, right) as node ->  
      if x < value then balance (color, insert x left, value, right)  
      else if x > value then balance (color, left, value, insert x right)  
      else node  
```  
   
Here, we define the type of color as either Red or Black. We also define the type of a tree as either a Leaf or a Node with a color, left subtree, value, and right subtree. The `balance` function takes a tuple and returns a balanced node. The `insert` function takes a value and a tree and returns a new tree with the value inserted in the correct position.  
   
## Step-by-step Explanation  
1. A new node is always inserted as a red node.  
2. If the node is the root node, it is colored black.  
3. If the parent of a red node is also red, the tree is rebalanced.  
4. If a node is red, its children must be black.  
5. The black height of a subtree must be equal on both sides.  
6. When rebalancing, the red node is rotated and recolored to maintain the above properties.  
   
## Complexity Analysis  
The worst-case time complexity for insertion, deletion, and search operations in a red-black tree is O(log n), where n is the number of nodes in the tree. This is because the height of a red-black tree is always log n. The space complexity is also O(n), where n is the number of nodes in the tree, as each node requires a constant amount of memory.