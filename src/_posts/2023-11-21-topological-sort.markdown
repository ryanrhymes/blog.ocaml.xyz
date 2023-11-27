---
layout: post
title:  "Topological Sort"
date:   2023-11-21 04:00:00 +0200
categories: algorithm
---

## Introduction  
Topological sort is a sorting algorithm used to sort a directed acyclic graph (DAG) in a specific order. It is commonly used in many applications such as task scheduling, dependency resolution, and data processing.  
   
## Implementation  
Here is an implementation of topological sort in OCaml:  
   
```ocaml  
let topological_sort (graph: int list array) : int list =  
  let n = Array.length graph in  
  let visited = Array.make n false in  
  let stack = ref [] in  
  
  let rec dfs (u: int) : unit =  
    visited.(u) <- true;  
    List.iter (fun v -> if not visited.(v) then dfs v) graph.(u);  
    stack := u :: !stack  
  in  
  
  for i = 0 to n - 1 do  
    if not visited.(i) then dfs i  
  done;  
  
  !stack  
```  
   
Here, `graph` is a representation of the DAG in the form of an adjacency list. The function returns a list of nodes in topological order.  Here is an example.

```ocaml
let courses = ["C1"; "C2"; "C3"; "C4"; "C5"]  
let prerequisites = [("C1", ["C2"; "C3"]); ("C2", ["C4"]); ("C3", ["C4"]); ("C4", ["C5"])]  
  
let course_graph =  
  let n = List.length courses in  
  let graph = Array.make n [] in  
  let index_of_course c =
    match find_index (fun x -> x = c) courses with
    | Some(index) -> index
    | None -> failwith "Course not found"
  in  
  List.iter (fun (c, prereqs) ->  
    let u = index_of_course c in  
    let vs = List.map index_of_course prereqs in  
    List.iter (fun v -> graph.(u) <- v :: graph.(u)) vs  
  ) prerequisites;  
  graph  
  
let course_order = topological_sort course_graph |> List.map (fun i -> List.nth courses i)

let print_course_order course_order =
  List.iter (fun course -> Printf.printf "%s " course) course_order;
  Printf.printf "\n"

let () =
  print_course_order course_order
```
   
## Step-by-step Explanation  
1. Create an array `visited` of size `n`, where `n` is the number of nodes in the graph. Initialize all elements to `false`.  
2. Create an empty stack `stack`.  
3. Define a recursive function `dfs` that takes a node `u` as input.  
4. Mark `u` as visited by setting `visited.(u)` to `true`.  
5. For each neighbor `v` of `u` in the graph, if `v` has not been visited, recursively call `dfs v`.  
6. Push `u` onto the `stack`.  
7. Loop through all nodes in the graph. If a node has not been visited, call `dfs` on it.  
8. Return the `stack`.  
   
The algorithm works by performing a depth-first search (DFS) on the graph. When a node is finished being explored, it is pushed onto the stack. Since a DAG has no cycles, the nodes can be ordered in reverse order of their finishing times, which gives a valid topological order.  
   
## Complexity Analysis  
The time complexity of topological sort is O(V+E), where V is the number of vertices and E is the number of edges in the graph. This is because each vertex and edge is visited once during the DFS traversal. The space complexity is also O(V+E), since the adjacency list takes up O(V+E) space and the stack can contain all vertices in the worst case.