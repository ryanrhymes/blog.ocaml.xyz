---
layout: post
title:  "Best-First Search"
date:   2023-11-22 08:00:00 +0200
categories: algorithm
---

## Introduction  
Best-first search is a graph search algorithm that aims to find the optimal path between two nodes in a graph. It is commonly used in artificial intelligence and computer science, particularly in the field of pathfinding. The algorithm works by exploring the most promising nodes first, based on a heuristic function that estimates the distance to the goal node. Best-first search can be used in a variety of applications, such as route planning, game AI, and logistics.  
   
## Implementation  
Here is an implementation of Best-first search in OCaml:  
   
```ocaml  
type 'a node = {  
  state: 'a;  
  parent: 'a node option;  
  cost: float;  
  heuristic: float;  
}  
   
let best_first_search initial_state goal_state successors heuristic =  
  let is_goal n = n.state = goal_state in  
  let rec search frontier visited =  
    match frontier with  
    | [] -> None  
    | n :: _ when is_goal n -> Some n  
    | n :: ns ->  
        let successors = successors n.state in  
        let add_node frontier node =  
          if List.exists (fun n -> n.state = node.state) visited then frontier  
          else List.merge (fun n1 n2 -> compare n1.cost n2.cost)  
                 frontier [node] in  
        let nodes = List.map (fun (state, cost) ->  
            let heuristic = heuristic state in  
            {state; parent = Some n; cost; heuristic}) successors in  
        let frontier' = List.fold_left add_node ns nodes in  
        let visited' = n :: visited in  
        search frontier' visited'  
  in  
  let initial_node = {state = initial_state; parent = None; cost = 0.; heuristic = heuristic initial_state} in  
  search [initial_node] []  
   
```  
   
Here, `type 'a node` represents a node in the search tree. Each node has a state, a parent node, a cost (i.e., the cumulative cost from the initial state), and a heuristic value (i.e., the estimated distance to the goal state). The `best_first_search` function takes as input the initial state, the goal state, a function that generates the successors of a given state, and a heuristic function that estimates the distance to the goal state. The function returns an optional node that represents the goal state, or `None` if no path was found.  

## Example

Here's an example of using Best-first search to find the shortest path between two cities in a graph:  
   
```ocaml  
let graph = [  
  ("A", [("B", 7.); ("C", 3.)]);  
  ("B", [("A", 7.); ("C", 1.); ("D", 2.)]);  
  ("C", [("A", 3.); ("B", 1.); ("D", 2.)]);  
  ("D", [("B", 2.); ("C", 2.); ("E", 4.)]);  
  ("E", [("D", 4.)])  
]  
   
let successors state =  
  let rec find_node state = function  
    | [] -> failwith "Node not found"  
    | (s, _) :: _ when s = state -> []  
    | _ :: rest -> find_node state rest  
  in  
  let (_, edges) = List.find (fun (s, _) -> s = state) graph in  
  List.map (fun (s, c) -> (s, c)) edges @ find_node state graph  
   
let heuristic state =  
  match state with  
  | "A" -> 6.  
  | "B" -> 2.  
  | "C" -> 4.  
  | "D" -> 2.  
  | "E" -> 0.  
  | _ -> failwith "Invalid state"  
   
let path = best_first_search "A" "E" successors heuristic  
   
let print_path path =  
  let rec print_node node =  
    match node.parent with  
    | None -> print_endline node.state  
    | Some parent ->  
        print_node parent;  
        print_endline node.state  
  in  
  match path with  
  | None -> print_endline "No path found"  
  | Some node -> print_node node  
   
let () = print_path path  
```  
   
This code defines a graph as a list of nodes, where each node is a tuple of a state and a list of edges to other nodes, each with a cost. The `successors` function generates the successors of a given state by looking up the corresponding node in the graph and returning its edges. The `heuristic` function estimates the distance to the goal state based on a predefined heuristic value for each state.  
   
The `path` variable contains the result of running Best-first search on the graph, starting from state "A" and ending at state "E". Finally, the `print_path` function prints out the path from the initial state to the goal state.  
   
When we run this code, we should see the following output:  
   
```  
A  
C  
D  
E  
```   
  
This indicates that the shortest path from "A" to "E" is "A" -> "C" -> "D" -> "E", with a total cost of 9.

## Step-by-step Explanation  
1. Create a node that represents the initial state, with a cost of 0 and a heuristic value based on the initial state.  
2. Create an empty list of visited nodes and a list that contains only the initial node as the frontier.  
3. If the frontier is empty, return `None` (i.e., no path was found).  
4. Otherwise, take the node at the front of the frontier and check if it is the goal node. If so, return it.  
5. Otherwise, generate the successors of the current node, create nodes for each of them, and add them to the frontier.  
6. Sort the frontier based on the cost of each node (i.e., the cumulative cost from the initial state).  
7. Add the current node to the list of visited nodes.  
8. Repeat from step 3.  
   
The key idea behind Best-first search is to explore the most promising nodes first, based on a heuristic function that estimates the distance to the goal node. This allows the algorithm to quickly converge to the optimal path, while avoiding exploring unpromising paths.  
   
## Complexity Analysis  
The time complexity of Best-first search depends on the quality of the heuristic function. In the worst case, where the heuristic function is not informative (i.e., it always returns 0), the algorithm degenerates to a breadth-first search, with a time complexity of O(b^d), where b is the branching factor of the graph and d is the depth of the goal node. In the