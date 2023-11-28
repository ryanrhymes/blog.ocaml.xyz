---
layout: post
title:  "Tukey Fences"
date:   2021-11-19 14:05:50 +0200
categories: algorithm
---

## Introduction

Tukey fences is an algorithm used in exploratory data analysis to detect outliers in a dataset. Outliers are data points that are significantly different from other observations in the dataset and can often bias statistical analysis results. Tukey fences method provides a quick and easy way to identify potential outlier data points in numerical datasets.

## Implementation

```ocaml

(* Tukey Fences *)

let tukey_fences ?(k = 1.5) arr =
  let first_quartile = first_quartile arr in
  let third_quartile = third_quartile arr in
  let offset = k *. (third_quartile -. first_quartile) in
  first_quartile -. offset, third_quartile +. offset

```


Tukey fences algorithm uses quartiles of a dataset to define inner and outer fences. The inner fence range includes all observations within 1.5 times of the interquartile range on either side of the median. The outer fence range includes all observations within 3 times of the interquartile range on either side of the median.

## Step-by-step Explanation

1. Calculate the first quartile (Q1) and third quartile (Q3) of the input dataset.
2. Calculate the interquartile range (IQR) as Q3 - Q1.
3. Calculate the Tukey fence offset as k times IQR, where k is a constant multiplier (default value is 1.5).
4. Calculate the inner fence as Q1 - offset and Q3 + offset. All data points within this range are not detected as outliers.
5. Calculate the outer fence as Q1 - 3*offset and Q3 + 3*offset. Any data point that falls outside this range is considered an outlier.

## Complexity Analysis

The time complexity of Tukey fences algorithm is O(nlogn) where n is the size of the input dataset. This is because the algorithm requires sorting the dataset to determine the order of the observations and calculate the quartiles. The space complexity is O(n) as the function requires creating a copy of the input dataset in memory.

In terms of performance, the time complexity can be reduced to O(n) by using an algorithm like Quickselect to select the kth order statistic instead of sorting the entire dataset.