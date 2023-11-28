---
layout: post
title:  "Gaussian Kernel Density Estimation"
date:   2022-09-13 17:07:14 +0200
categories: algorithm
---

## Introduction
Kernel density estimation is a non-parametric method used to estimate the probability density function of a random variable. The goal of KDE is to find a function that best represents the underlying probability density function (pdf) of a given set of data points, without making any assumptions about the distribution of the data. This algorithm works by drawing a kernel around each data point in the given set, and estimating the height of the curve at each point. The sum of these curves then represents the overall pdf.

## Implementation

```ocaml

(* Gaussian Kernel-Density Estimate *)

let build_kernel = function
  | `Gaussian ->
    fun h p v ->
      let u = (v -. p) /. h in
      1. /. Owl_const.(sqrt2 *. sqrtpi) *. exp (-.Owl_base_maths.sqr u /. 2.)


let build_points n_points h kernel vs =
  let min, max = minmax vs in
  let a, b =
    match kernel with
    | `Gaussian -> min -. (3. *. h), max +. (3. *. h)
  in
  let points = Array.make n_points 0. in
  let step = (b -. a) /. float_of_int n_points in
  for i = 0 to n_points - 1 do
    Array.unsafe_set points i (a +. (float_of_int i *. step))
  done;
  points


let gaussian_kde ?(bandwidth = `Scott) ?(n_points = 512) vs =
  if Array.length vs < 2
  then invalid_arg "estimate_pdf: sample should have multiple elements";
  let n = float_of_int (Array.length vs) in
  let s = min [| std vs; interquartile vs /. 0.34 |] in
  let h =
    match bandwidth with
    | `Silverman -> 0.90 *. s *. (n ** -0.2)
    | `Scott     -> 1.06 *. s *. (n ** -0.2)
  in
  let kernel = `Gaussian in
  let points = build_points n_points h kernel vs in
  let k = build_kernel kernel in
  let f = 1. /. (h *. n) in
  let pdf = Array.make n_points 0. in
  for i = 0 to n_points - 1 do
    let p = Array.unsafe_get points i in
    Array.unsafe_set pdf i (f *. Array.fold_left (fun acc v -> acc +. k h p v) 0. vs)
  done;
  points, pdf

```

This implementation of kernel density estimation uses the Gaussian kernel function to estimate the pdf. It takes in a set of data points, and an optional bandwidth parameter, as well as an optional number of points to use in the output pdf estimate. The algorithm iterates through the set of data points, drawing a Gaussian kernel around each point with height proportional to the kernel function and bandwidth proportional to the standard deviation of the data. The algorithm then evaluates the sum of all kernel curves at each point in an evenly spaced grid of points across the data range, and returns an array of x-axis points and y-axis probabilities representing the pdf estimate.

## Step-by-step Explanation:
1. Check if the input array of data points has more than 1 element. If not, throw an error message.
2. Calculate the number of data points and the standard deviation of the input values. Calculate the bandwidth of the kernel function using the standard deviation and the optional bandwidth parameter.
3. Create a set of evenly spaced points along the range of the input data set.
4. Define the kernel function, which is Gaussian in this case, and calculate the height of the kernel density curve for each data point and grid point.
5. Multiply the kernel function by a scaling factor, 1 / (bandwidth * n_points), and calculate the area under the curve for each grid point.
6. Return the set of grid points and their corresponding probability density values as an array.

## Complexity Analysis:
The time complexity of kernel density estimation depends on the number of data points and the number of points used in the output pdf estimate. The time complexity of the algorithm is O(n_points * n), where n is the number of data points, since the algorithm must iterate through each data point for each grid point in the pdf estimate. The space complexity of the algorithm is O(n_points), since it creates an array of evenly spaced points along the range of the data. The Gaussian kernel function used in this algorithm is a continuous function, but is evaluated at discrete points, which creates an approximation error that decreases with increasing number of grid points. The bandwidth parameter controls the amount of smoothing in the pdf estimate, and finding an optimal value can be done using cross-validation or other methods.