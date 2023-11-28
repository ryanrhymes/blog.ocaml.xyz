---
layout: post
title:  "Quick Sort in C++"
date:   2022-01-23 03:49:56 +0200
categories: algorithm cpp
---

## Introduction

QuickSort is one of the most popular algorithms to sort an array. It is based on the divide-and-conquer strategy and provides an efficient algorithm to sort large datasets. This algorithm was initially developed by Tony Hoare in 1959 and it is easier to implement in comparison to other sorting algorithms. Its time complexity is `O(n log n)`, which makes it faster than Bubble sort and Selection sort.

QuickSort can be used in various applications like sorting database records, searching and sorting files, sorting network traffic, etc.

## Implementation

```ocaml

// Quick sort in C++

#include <iostream>
using namespace std;

// function to swap elements
void swap(int *a, int *b) {
  int t = *a;
  *a = *b;
  *b = t;
}

// function to print the array
void printArray(int array[], int size) {
  int i;
  for (i = 0; i < size; i++)
    cout << array[i] << " ";
  cout << endl;
}

// function to rearrange array (find the partition point)
int partition(int array[], int low, int high) {
    
  // select the rightmost element as pivot
  int pivot = array[high];
  
  // pointer for greater element
  int i = (low - 1);

  // traverse each element of the array
  // compare them with the pivot
  for (int j = low; j < high; j++) {
    if (array[j] <= pivot) {
        
      // if element smaller than pivot is found
      // swap it with the greater element pointed by i
      i++;
      
      // swap element at i with element at j
      swap(&array[i], &array[j]);
    }
  }
  
  // swap pivot with the greater element at i
  swap(&array[i + 1], &array[high]);
  
  // return the partition point
  return (i + 1);
}

void quickSort(int array[], int low, int high) {
  if (low < high) {
      
    // find the pivot element such that
    // elements smaller than pivot are on left of pivot
    // elements greater than pivot are on righ of pivot
    int pi = partition(array, low, high);

    // recursive call on the left of pivot
    quickSort(array, low, pi - 1);

    // recursive call on the right of pivot
    quickSort(array, pi + 1, high);
  }
}

// Driver code
int main() {
  int data[] = {8, 7, 6, 1, 0, 9, 2};
  int n = sizeof(data) / sizeof(data[0]);
  
  cout << "Unsorted Array: 
";
  printArray(data, n);
  
  // perform quicksort on data
  quickSort(data, 0, n - 1);
  
  cout << "Sorted array in ascending order: 
";
  printArray(data, n);
}

```


QuickSort is a divide-and-conquer algorithm to sort an array. It selects an element as a pivot and partitions the array around this pivot. All elements smaller than the pivot are placed on the left side of the pivot, and all the greater elements are placed on the right side of the pivot. This process is repeated recursively for the left and right sub-arrays until each element is at its sorted position. 

## Step-by-step Explanation

1. We first select the last element of the array as our pivot.
2. We set a pointer `i` to the left of the array and traverse each element of the array from left to right.
3. We compare each element with the pivot and if it is smaller, we swap the element with the element at `i+1`.
4. We then increase the value of `i` by 1, and repeat the process.
5. Once all the elements are compared, we swap the pivot element with the element at `i+1`.
6. We then have two sub-arrays, one with elements smaller than the pivot and the other with elements greater than the pivot.
7. We partition these sub-arrays and recursively repeat these steps for each sub-array.
8. Once the sub-arrays have been sorted, the entire array is now sorted.

## Complexity Analysis

QuickSort has an average case time complexity of `O(n log n)`. However, in the worst-case scenario where the chosen pivot is always the smallest or largest element in the array, the algorithm will have a time complexity of `O(n^2)`. On the other hand, the best-case time complexity is `O(n log n)`. 

The algorithm sort the array in place, which means that the space complexity is `O(1)`.

The worst-case time complexity can be avoided by choosing the pivot element carefully. There are different methods to choose the pivot element, including selecting the median, random, or first element. The pivot element can also be chosen adaptively by using the median-of-medians algorithm, which gives a deterministic algorithm with a guaranteed worst-case time complexity of `O(n log n)`.