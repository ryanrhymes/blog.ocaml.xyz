---
layout: post
title:  "Dijkstra Algorithm"
date:   2023-11-23 00:00:00 +0200
categories: algorithm
---

## Introduction  
Dijkstra algorithm is a shortest path algorithm that is used to find the shortest path between two nodes in a graph. It was conceived by Edsger W. Dijkstra in 1956 and published three years later. The algorithm is used in various applications such as transportation networks, computer networks, and social networks.  
   
## Implementation  
The Dijkstra algorithm is implemented in OCaml as follows:  
   
```ocaml  
let list_vertices graph =
  List.fold_left (fun acc ((a, b), _) ->
    let acc = if List.mem b acc then acc else b::acc in
    let acc = if List.mem a acc then acc else a::acc in
    acc
  ) [] graph

let neighbors v =
  List.fold_left (fun acc ((a, b), d) ->
    if a = v then (b, d)::acc else acc
  ) []

let remove_from v lst =
  let rec aux acc = function [] -> failwith "remove_from"
  | x::xs -> if x = v then List.rev_append acc xs else aux (x::acc) xs
  in aux [] lst

let with_smallest_distance q dist =
  match q with
  | [] -> assert false
  | x::xs ->
      let rec aux distance v = function
      | x::xs ->
          let d = Hashtbl.find dist x in
          if d < distance
          then aux d x xs
          else aux distance v xs
      | [] -> (v, distance)
      in
      aux (Hashtbl.find dist x) x xs

let dijkstra max_val zero add graph source target =
  let vertices = list_vertices graph in
  let dist_between u v =
    try List.assoc (u, v) graph
    with _ -> zero
  in
  let dist = Hashtbl.create 1 in
  let previous = Hashtbl.create 1 in
  List.iter (fun v ->                  (* initializations *)
    Hashtbl.add dist v max_val         (* unknown distance function from source to v *)
  ) vertices;
  Hashtbl.replace dist source zero;    (* distance from source to source *)
  let rec loop = function [] -> ()
  | q ->
      let u, dist_u =
        with_smallest_distance q dist in   (* vertex in q with smallest distance in dist *)
      if dist_u = max_val then
        failwith "vertices inaccessible";  (* all remaining vertices are inaccessible from source *)
      if u = target then () else begin
        let q = remove_from u q in
        List.iter (fun (v, d) ->
          if List.mem v q then begin
            let alt = add dist_u (dist_between u v) in
            let dist_v = Hashtbl.find dist v in
            if alt < dist_v then begin       (* relax (u,v,a) *)
              Hashtbl.replace dist v alt;
              Hashtbl.replace previous v u;  (* previous node in optimal path from source *)
            end
          end
        ) (neighbors u graph);
        loop q
      end
  in
  loop vertices;
  let s = ref [] in
  let u = ref target in
  while Hashtbl.mem previous !u do
    s := !u :: !s;
    u := Hashtbl.find previous !u
  done;
  (source :: !s)
```  
   
The function `dijkstra` takes in a graph represented as a list of edges and their weights, a maximum value, a zero value, an addition function, a source node, and a target node. It returns a list of nodes representing the shortest path from the source to the target.  
   
Here is how to call the function.

```ocaml
let () =
  let graph =
    [ ("a", "b"), 7;
      ("a", "c"), 9;
      ("a", "f"), 14;
      ("b", "c"), 10;
      ("b", "d"), 15;
      ("c", "d"), 11;
      ("c", "f"), 2;
      ("d", "e"), 6;
      ("e", "f"), 9; ]
  in
  let p = dijkstra max_int 0 (+) graph "a" "e" in
  print_endline (String.concat " -> " p)
```

## Step-by-step Explanation  
1. The function `list_vertices` takes in the graph and returns a list of all the vertices in the graph. It does this by iterating over each edge in the graph and adding its endpoints to the list of vertices if they are not already in it.  
   
  2. The function `neighbors` takes in a vertex `v` and returns a list of its neighbors and their distances. It does this by iterating over each edge in the graph and adding the neighbor of `v` and its distance to the list if `v` is the start point of the edge.  
     
  3. The function `remove_from` takes in a vertex `v` and a list `lst` and returns a new list with `v` removed from it. It does this by recursively iterating over the list and adding each element to a new list except for `v`.  
     
  4. The function `with_smallest_distance` takes in a list of vertices `q` and a hash table of distances `dist` and returns the vertex in `q` with the smallest distance in `dist`. It does this by recursively iterating over the list and comparing the distance of each vertex to the current smallest distance.  
     
  5. The function `dijkstra` initializes the hash table of distances `dist` and the hash table of previous vertices `previous`. It then iterates over each vertex in the graph and adds it to `dist` with a distance of `max_val` except for the source node, which is added with a distance of `zero`. It then enters a loop where it selects the vertex in `q` with the smallest distance in `dist` using `with_smallest_distance`. If the distance is `max_val`, it means all remaining vertices are inaccessible from the source, so it throws an error. If the selected vertex is the target node, it exits the loop. Otherwise, it removes the selected vertex from `q` and iterates over its neighbors. For each neighbor, it computes a new distance `alt` from the source to the neighbor through the selected vertex and updates `dist` and `previous` if `alt` is smaller than the current distance in `dist`. It then continues the loop with the updated `q`.  
     
  6. The function `dijkstra` ends by constructing the shortest path from the source to the target using the hash table of previous vertices `previous`. It starts at the target and iteratively adds the previous vertex to the path until it reaches the source.  
     
## Complexity Analysis  
The time complexity of Dijkstra's algorithm is O((E+V)logV), where E is the number of edges and V is the number of vertices in the graph. This is because the algorithm iterates over each vertex once and each edge once, and it uses a priority queue to select the vertex with the smallest distance in each iteration, which takes logV time. The space complexity is O(V+E) because it stores the hash tables of distances and previous vertices, which have a size proportional to the number of vertices and edges in the graph. 

In terms of the input parameters, the time complexity of `list_vertices` is O(E), where E is the number of edges in the graph, because it iterates over each edge once. The time complexity of `neighbors` is also O(E), because it iterates over each edge once. The time complexity of `remove_from` is O(n), where n is the length of the input list, because it iterates over each element once. The time complexity of `with_smallest_distance` is O(V), where V is the number of vertices in the input list, because it iterates over each vertex once. Therefore, the overall time complexity of the Dijkstra algorithm is dominated by the time complexity of the main loop, which is O((E+V)logV).  
   
In terms of the space complexity, the hash tables used in the algorithm have a size proportional to the number of vertices in the graph, which is O(V). The priority queue used in `with_smallest_distance` also has a size proportional to the number of vertices, which is O(V). Therefore, the overall space complexity of the algorithm is O(V+E).  
   
Overall, the Dijkstra algorithm is an efficient algorithm for finding the shortest path between two nodes in a graph. Its time complexity is proportional to the number of edges and vertices in the graph, and its space complexity is proportional to the number of vertices.

