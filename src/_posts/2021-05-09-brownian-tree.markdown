---
layout: post
title:  "Brownian Tree"
date:   2021-05-09 19:35:09 +0200
categories: algorithm
---

## Introduction
Brownian Tree algorithm is a probabilistic method that creates a pattern resembling branches of a tree and is used mainly in computer graphics and physics simulations. The algorithm was named after Robert Brown, the first person who studied the movement of microscopic particles in liquids, known as Brownian motion. In image processing, the Brownian Tree can simulate the aggregation of particles under different conditions and generate fractal-like patterns.

## Implementation

```ocaml

(* Brownian Tree *)

let world_width = 400
let world_height = 400
let num_particles = 20_000
 
let () =
  assert(num_particles > 0);
  assert(world_width * world_height > num_particles);
;; 
 
let dla ~world =
  (* put the tree seed *)
  world.(world_height / 2).(world_width / 2) <- 1;
 
  for i = 1 to num_particles do
    (* looping helper function *)
    let rec aux px py =
      (* randomly choose a direction *)
      let dx = (Random.int 3) - 1  (* offsets *)
      and dy = (Random.int 3) - 1 in

      if dx + px < 0 || dx + px >= world_width ||
         dy + py < 0 || dy + py >= world_height then
        (* plop the particle into some other random location *)
        aux (Random.int world_width) (Random.int world_height)
      else if world.(py + dy).(px + dx) <> 0 then
        (* bumped into something, particle set *)
        world.(py).(px) <- 1
      else
        aux (px + dx) (py + dy)
    in
    (* set particle's initial position *)
    aux (Random.int world_width) (Random.int world_height)
  done
 
let to_pbm ~world =
  print_endline "P1";  (* Type=Portable bitmap, Encoding=ASCII *)
  Printf.printf "%d %d
" world_width world_height;
  Array.iter (fun line ->
    Array.iter print_int line;
    print_newline()
  ) world
 
let () =
  Random.self_init();
  let world = Array.make_matrix world_width world_height 0 in
  dla ~world;
  to_pbm ~world;
;;

```

The Brownian Tree algorithm starts with a seed particle placed at the center of an image, representing the trunk of a tree. The algorithm then randomly adds particles, called walkers, to the image. Each walker moves in a random direction until it collides with another particle, which forms a new branch. The process repeats until the desired number of particles is reached, resulting in a fractal-like pattern.

The implementation is done using the programming language OCaml. The code creates a two-dimensional world with given dimensions `world_width` and `world_height`. The number of particles to add is defined by `num_particles`. The `dla` function implements the algorithm according to the above description. The `to_pbm` function converts the resulting matrix into a Portable Bitmap file format.

## Step-by-step Explanation
1. The `world` matrix is initialized with zeros.
2. The center of the matrix is set to `1`, representing the seed particle.
3. A loop from `1` to `num_particles` adds random particles called walkers.
4. The `aux` function is a helper function that takes the position `(px, py)` of the current walker.
5. At each step, a direction is randomly chosen for the walker by generating random integers `dx` and `dy` between `-1` and `1`.
6. If the next position of the walker is outside the boundaries of the world, the walker is plopped into another random location.
7. If the next position overlaps with another particle in the world, that particle is set as the new branch, and the current walker is removed.
8. If none of the above conditions is met, the walker moves to the new position `(px + dx, py + dy)`, and the `aux` function is called recursively with this new position.
9. The resulting world matrix is converted to a Portable Bitmap file format.

## Complexity Analysis
The time complexity of Brownian Tree algorithm depends mainly on the number of iterations, which is the number of particles to add to the world. The algorithm uses a recursive function, which can go up to the maximum depth of `num_particles`. The worst-case time complexity of the algorithm can be approximated by O(num_particles * (world_width * world_height)^2), but in practice, the actual time complexity is much lower. The space complexity of the algorithm is O(world_width * world_height), which is the size of the 2D world matrix.