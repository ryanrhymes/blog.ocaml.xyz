---
layout: post
title:  "Closest-pair problem"
date:   2021-09-12 00:05:16 +0200
categories: algorithm
---

## Introduction
The closest-pair problem in computational geometry is the problem of finding the pair of two closest points given a set of points in Euclidean space. This algorithm uses the divide-and-conquer approach for the Euclidean space. The algorithm is used in geographical information systems to help find the proximity of two points in a coordinate plane, image processing in detecting the closely related pixel regions, and proximity-based clustering, etc.

## Implementation

```ocaml

(* Closest-pair problem *)

type point = { x : float; y : float }

let cmpPointX (a : point) (b : point) = compare a.x b.x 
let cmpPointY (a : point) (b : point) = compare a.y b.y 


let distSqrd (seg : (point * point) option) =
  match seg with
  | None -> max_float
  | Some(line) ->
    let a = fst line in
    let b = snd line in

    let dx = a.x -. b.x in
    let dy = a.y -. b.y in
  
    dx*.dx +. dy*.dy


let dist seg =
  sqrt (distSqrd seg)


let shortest l1 l2 =
  if distSqrd l1 < distSqrd l2 then
    l1
  else
    l2


let halve l =
  let n = List.length l in
  BatList.split_at (n/2) l


let rec closestBoundY from maxY (ptsByY : point list) =
  match ptsByY with
  | [] -> None
  | hd :: tl ->
    if hd.y > maxY then
      None
    else
      let toHd = Some(from, hd) in
      let bestToRest = closestBoundY from maxY tl in
      shortest toHd bestToRest


let rec closestInRange ptsByY maxDy =
  match ptsByY with
  | [] -> None
  | hd :: tl ->
    let fromHd = closestBoundY hd (hd.y +. maxDy) tl in
    let fromRest = closestInRange tl maxDy in
    shortest fromHd fromRest


let rec closestPairByX (ptsByX : point list) =
   if List.length ptsByX < 2 then
       None
   else
       let (left, right) = halve ptsByX in
       let leftResult = closestPairByX left in
       let rightResult = closestPairByX right in

       let bestInHalf = shortest  leftResult rightResult in
       let bestLength = dist bestInHalf in

       let divideX = (List.hd right).x in
       let inBand = List.filter(fun(p) -> abs_float(p.x -. divideX) < bestLength) ptsByX in

       let byY = List.sort cmpPointY inBand in
       let bestCross = closestInRange byY bestLength in
       shortest bestInHalf bestCross      


let closestPair pts =
  let ptsByX = List.sort cmpPointX pts in
  closestPairByX ptsByX


let parsePoint str =
  let sep = Str.regexp_string "," in
  let tokens = Str.split sep str in
  let xStr = List.nth tokens 0 in
  let yStr = List.nth tokens 1 in

  let xVal = (float_of_string xStr) in
  let yVal = (float_of_string yStr) in
  
  { x = xVal; y = yVal }


let loadPoints filename =
  let ic = open_in filename in
  let result = ref [] in
  try
    while true do
      let s = input_line ic in
      if s <> "" then
        let p = parsePoint s in
        result := p :: !result;
    done;
    !result
  with End_of_file ->
    close_in ic;
    !result
;;

let loaded = (loadPoints "Points.txt") in
let start = Sys.time() in
let c = closestPair loaded in
let taken = Sys.time() -. start in
Printf.printf "Took %f [s]
" taken;

match c with
| None -> Printf.printf "No closest pair
"
| Some(seg) -> 
  let a = fst seg in
  let b = snd seg in

  Printf.printf "(%f, %f) (%f, %f) Dist %f
" a.x a.y b.x b.y (dist c)

```

The algorithm consists of dividing the set of n points into two subsets of n/2 points, finding the closest pair in each subset recursively, and then combining these pairs to make the final result. The combining step where two closest pairs cross the midpoint takes O(n) time because it sorts the set of points in Y dimension using the two closest pairs' distance to select the boundary points.

The algorithm has the following steps:

1. Sort the points according to their x coordinates.
2. For each half of the set, recursively find the closest pair.
3. Let d be the minimum distance obtained between a point in the left subset and a point in the right subset.
4. Combine the two closest pairs obtained from the left and right subsets (from the recursive calls).
5. Check the strip with width d centred at the middle x-coordinate of the closest pair of the boundary lines, for pairs of points closer than d apart. If there is a pair within d, return the closest pair.

## Step-by-step Explanation
1. The algorithm requires the points to be given as an array, sorted based on their x-coordinate.
2. For the given points, the algorithm recursively divides the points into two halves based on their x-coordinate.
3. The recursion continues until the number of points becomes too small for partitioning.
4. Then, the closest points in the adjacency are considered and their distance is computed.
5. Points with a distance below the threshold will be excluded from the final result. 
6. The points are sorted based on their y-coordinates, allowing us to quickly find the closest pair in the vertical strip of points near the dividing line.
7. Check whether the minimum distance obtained in each of the subproblems less than the distance across the center strip. Combine the answers obtained from each subproblem. 

## Complexity Analysis
Let's assume that the given set of points has n points. 

The algorithm sorts the points according to their x-coordinates, which takes O(n lg n) time. The algorithm uses a divide-and-conquer approach in which half of the points are assigned to the left and right independently. Therefore, we can describe the running time of the algorithm as follows:
- T(n) = 2 T(n/2) + O(n)

We have to find the closest pair in the entire set by merging the closest pair in both the right and left parts. It takes O(n) time to combine the two solutions. 

The time complexity of the algorithm can be expressed as: 

- T(n) = 2T(n/2) + n + O(n)  
- T(n) = 2T(n/2) + O(n) 

Using the Master theorem, we can conclude that the above recurrence relation's time complexity is O(nlogn).

Therefore, the total time complexity of the algorithm will become O(n log n). The space complexity of the algorithm is also O(n), as we are sorting and combining the points by the highest and lowest variables. 

The algorithm is effective in eliminating all but closest-pair candidates within the strip, which results in a relatively small number of candidates. It is robust and works well even for large data sets.