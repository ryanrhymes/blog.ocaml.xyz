---
layout: post
title:  "Red Black Tree"
date:   2023-11-21 01:00:00 +0200
categories: algorithm
---

## Introduction  
Depth First Search (DFS) is a graph traversal algorithm that explores as far as possible along each branch before backtracking. It is a fundamental algorithm in computer science and has many applications such as finding connected components, topological sorting, and solving puzzles like the maze.  
   
## Implementation  
Here is an implementation of DFS in OCaml:  
   
```ocaml  
type 'a graph = ('a * 'a list) list  
   
let rec explore visited graph node =  
  if List.mem node visited then visited  
  else node :: List.fold_left (explore (node :: visited) graph) visited (List.assoc node graph)  
   
let dfs graph start =  
  List.rev (explore [] graph start)  
```  
   
The `graph` is represented as a list of pairs, where each pair consists of a node and its adjacent nodes. The `explore` function recursively explores the graph by visiting each unvisited node and its adjacent nodes. The `visited` parameter keeps track of the visited nodes to avoid visiting them again. The `dfs` function calls `explore` with an empty `visited` list and the starting node and returns the visited nodes in reverse order.  
   
Here is an example of using the `dfs` function:  
   
```ocaml  
let graph = [("A", ["B"; "C"; "D"]);  
             ("B", ["E"]);  
             ("C", ["F"]);  
             ("D", []);  
             ("E", []);  
             ("F", ["G"]);  
             ("G", [])]  
   
let visited = dfs graph "A"  
   
let () = List.iter print_endline visited  
```  
   
The output should be:  
   
```  
A  
B  
E  
C  
F  
G  
D  
```  
   
## Step-by-step Explanation  
1. Start at the given starting node.  
2. Mark the node as visited and add it to the visited list.  
3. For each adjacent node that is not visited, recursively call `explore` with the visited list and the adjacent node.  
4. Return the visited list in reverse order.  
   
## Complexity Analysis  
The time complexity of DFS is O(|V| + |E|), where |V| is the number of vertices and |E| is the number of edges in the graph. This is because DFS visits each vertex and each edge once. The space complexity is O(|V|), where |V| is the maximum number of vertices in the recursion stack. This is because DFS stores the visited nodes in a list and uses recursion to explore the graph, which creates a stack of function calls.