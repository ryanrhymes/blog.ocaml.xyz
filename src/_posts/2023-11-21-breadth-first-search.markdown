---
layout: post
title:  "Breadth-First Search"
date:   2023-11-21 02:00:00 +0200
categories: algorithm
---

# Breadth First Search  
   
## Introduction  
Breadth First Search (BFS) is a graph traversal algorithm that explores all the vertices of a graph in breadth-first order. It starts at the tree root (or some arbitrary node of a graph) and explores all the neighboring nodes at the present depth before moving on to the nodes at the next depth level. BFS is often used to find the shortest path between two nodes in an unweighted graph.  
   
## Implementation  
Here is an implementation of BFS in OCaml:  
   
```ocaml  
type 'a graph = ('a * 'a list) list  
   
let bfs (graph: 'a graph) (start: 'a) : 'a list =  
  let rec explore visited queue =  
    match queue with  
    | [] -> visited  
    | node :: rest ->  
      if List.mem node visited then  
        explore visited rest  
      else  
        let neighbors = List.assoc node graph in  
        let new_nodes = List.filter (fun n -> not (List.mem n visited)) neighbors in  
        explore (node :: visited) (rest @ new_nodes)  
  in explore [] [start]  
```  
   
The `graph` parameter is a list of tuples, where the first element is a node and the second element is a list of its neighboring nodes. The `start` parameter is the node where the search begins. The function returns a list of nodes visited in breadth-first order.  
   
Here is an example of using the `bfs` function:  
   
```ocaml  
let graph = [  
  ("A", ["B"; "C"]);  
  ("B", ["A"; "D"; "E"]);  
  ("C", ["A"; "F"]);  
  ("D", ["B"]);  
  ("E", ["B"; "F"]);  
  ("F", ["C"; "E"])  
]  
   
let result = bfs graph "A" (* ["A"; "B"; "C"; "D"; "E"; "F"] *)  
```  
   
## Step-by-step Explanation  
1. Create an empty `visited` list and a `queue` with the starting node in it.  
2. While the `queue` is not empty, take the first node from the `queue`.  
3. If the node has already been visited, skip it and continue to the next node in the `queue`.  
4. If the node has not been visited, add it to the `visited` list.  
5. Get the list of neighboring nodes for the current node from the `graph`.  
6. Filter out the nodes that have already been visited.  
7. Add the remaining nodes to the end of the `queue`.  
8. Repeat steps 2-7 until the `queue` is empty.  
9. Return the `visited` list.  
   
## Complexity Analysis  
The time complexity of BFS is O(|V| + |E|), where |V| is the number of vertices and |E| is the number of edges in the graph. This is because BFS visits each vertex and edge exactly once.   
  
The space complexity of BFS is O(|V|), where |V| is the number of vertices in the graph. This is because BFS uses a queue to store the nodes to be visited, and the maximum size of the queue is the number of vertices at the maximum depth of the graph. In the worst case, this is all the vertices, so the space complexity is O(|V|).