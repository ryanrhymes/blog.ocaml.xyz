---
layout: post
title:  "LU Decomposition"
date:   2023-11-22 04:00:00 +0200
categories: algorithm
---

## Introduction  
LU decomposition is a numerical method for solving systems of linear equations. It decomposes a square matrix into two triangular matrices, one lower triangular matrix (L) and one upper triangular matrix (U). This decomposition can be used to solve linear equations in the form Ax = b, where A is the matrix, x is the vector of unknowns, and b is the vector of constants.   
  
## Implementation  
The following implementation of LU decomposition is written in OCaml. It takes a matrix `a` as input and returns the L and U matrices, as well as a row permutation vector. The `_lu_base` function performs the decomposition and returns the L and U matrices and the permutation vector. The `lu` function calls `_lu_base` and then formats the output into the L and U matrices. The `_lu_solve_vec` function uses the output of `_lu_base` to solve a system of linear equations.  
   
```ocaml 
(* LU decomposition.
 * Input matrix: a[n][n]; return L/U in one matrix, and the row permutation vector.
 * Test: https://github.com/scipy/scipy/blob/master/scipy/linalg/tests/test_decomp.py
 *)
let _lu_base a =
  let k = M.kind a in
  let _abs = Owl_base_dense_common._abs_elt k in
  let _mul = Owl_base_dense_common._mul_elt k in
  let _div = Owl_base_dense_common._div_elt k in
  let _sub = Owl_base_dense_common._sub_elt k in
  let _flt = Owl_base_dense_common._float_typ_elt k in
  let _zero = Owl_const.zero k in
  let _one = Owl_const.one k in
  let lu = M.copy a in
  let n = (M.shape a).(0) in
  let m = (M.shape a).(1) in
  assert (n = m);
  let indx = Array.make n 0 in
  (* implicit scaling of each row *)
  let vv = Array.make n _zero in
  let tiny = _flt 1.0e-40 in
  let big = ref _zero in
  let temp = ref _zero in
  (* flag of row exchange *)
  let d = ref 1.0 in
  let imax = ref 0 in
  (* loop over rows to get the implicit scaling information *)
  for i = 0 to n - 1 do
    big := _zero;
    for j = 0 to n - 1 do
      temp := M.get lu [| i; j |] |> _abs;
      if !temp > !big then big := !temp
    done;
    if !big = _zero then raise Owl_exception.SINGULAR;
    vv.(i) <- _div _one !big
  done;
  for k = 0 to n - 1 do
    big := _zero;
    (* choose suitable pivot *)
    for i = k to n - 1 do
      temp := _mul (M.get lu [| i; k |] |> _abs) vv.(i);
      if !temp > !big
      then (
        big := !temp;
        imax := i)
    done;
    (* interchange rows *)
    if k <> !imax
    then (
      for j = 0 to n - 1 do
        temp := M.get lu [| !imax; j |];
        let tmp = M.get lu [| k; j |] in
        M.set lu [| !imax; j |] tmp;
        M.set lu [| k; j |] !temp
      done;
      d := !d *. -1.;
      vv.(!imax) <- vv.(k));
    indx.(k) <- !imax;
    if M.get lu [| k; k |] = _zero then M.set lu [| k; k |] tiny;
    for i = k + 1 to n - 1 do
      let tmp0 = M.get lu [| i; k |] in
      let tmp1 = M.get lu [| k; k |] in
      temp := _div tmp0 tmp1;
      M.set lu [| i; k |] !temp;
      for j = k + 1 to n - 1 do
        let prev = M.get lu [| i; j |] in
        M.set lu [| i; j |] (_sub prev (_mul !temp (M.get lu [| k; j |])))
      done
    done
  done;
  lu, indx, !d


(* LU decomposition, return L, U, and permutation vector *)
let lu a =
  let k = M.kind a in
  let _zero = Owl_const.zero k in
  let lu, indx, _ = _lu_base a in
  let n = (M.shape lu).(0) in
  let m = (M.shape lu).(1) in
  assert (n = m && n >= 2);
  let l = M.eye k n in
  for r = 1 to n - 1 do
    for c = 0 to r - 1 do
      let v = M.get lu [| r; c |] in
      M.set l [| r; c |] v;
      M.set lu [| r; c |] _zero
    done
  done;
  l, lu, indx


let _lu_solve_vec a b =
  let _k = M.kind a in
  let _mul = Owl_base_dense_common._mul_elt _k in
  let _div = Owl_base_dense_common._div_elt _k in
  let _sub = Owl_base_dense_common._sub_elt _k in
  let _zero = Owl_const.zero _k in
  assert (Array.length (M.shape b) = 1);
  let n = (M.shape a).(0) in
  if (M.shape b).(0) <> n then failwith "LUdcmp::solve bad sizes";
  let ii = ref 0 in
  let sum = ref _zero in
  let x = M.copy b in
  let lu, indx, _ = _lu_base a in
  for i = 0 to n - 1 do
    let ip = indx.(i) in
    sum := M.get x [| ip |];
    M.set x [| ip |] (M.get x [| i |]);
    if !ii <> 0
    then
      for j = !ii - 1 to i - 1 do
        sum := _sub !sum (_mul (M.get lu [| i; j |]) (M.get x [| j |]))
      done
    else if !sum <> _zero
    then ii := !ii + 1;
    M.set x [| i |] !sum
  done;
  for i = n - 1 downto 0 do
    sum := M.get x [| i |];
    for j = i + 1 to n - 1 do
      sum := _sub !sum (_mul (M.get lu [| i; j |]) (M.get x [| j |]))
    done;
    M.set x [| i |] (_div !sum (M.get lu [| i; i |]))
  done;
  x
```

## Step-by-step Explanation  
1. The `_lu_base` function takes a square matrix `a` as input and returns the L and U matrices, as well as a row permutation vector.   
  
2. The function first makes a copy of the input matrix `a` into a new matrix `lu`.  
   
3. It initializes some variables, including the row permutation vector `indx`, the scaling vector `vv`, a small value `tiny`, and a flag `d`.  
   
4. The function then calculates the implicit scaling of each row by finding the maximum absolute value in each row and storing the reciprocal of that value in the `vv` vector.  
   
5. It then performs the LU decomposition using Gaussian elimination with partial pivoting. The function loops over each column `k` and selects the pivot element as the one with the largest scaled value in the column. If necessary, it exchanges rows to bring the pivot element to the diagonal. It stores the row index of each pivot element in the `indx` vector.  
   
6. During the elimination process, the function stores the multipliers used to eliminate the elements below the pivot element in the `lu` matrix.  
   
7. The function also keeps track of the sign of the row exchanges in the `d` flag.  
   
8. After the elimination process is complete, the function returns the `lu` matrix, the `indx` vector, and the `d` flag.  
   
9. The `lu` function calls `_lu_base` and then formats the output into the L and U matrices. It first calls `_lu_base` to get the `lu` matrix, `indx` vector, and `d` flag. It then creates an identity matrix `l` with the same size as `lu`.  
   
10. The function sets the elements of `l` to the corresponding elements of `lu` below the diagonal.  
   
11. The function returns the `l` and `lu` matrices, as well as the `indx` vector.  
   
12. The `_lu_solve_vec` function uses the output of `_lu_base` to solve a system of linear equations. It takes a matrix `a` and a vector `b` as input and returns the solution vector `x`.  
   
13. The function first checks that the dimensions of `a` and `b` are compatible.  
   
14. It then calls `_lu_base` to get the `lu` matrix, `indx` vector, and `d` flag.  
   
15. The function initializes some variables and loops over each row of the `lu` matrix to solve for the intermediate solution vector `y`.  
   
16. The function then loops over each row of the `lu` matrix again to solve for the final solution vector `x`.  
   
17. The function returns the solution vector `x`.  

## Complexity Analysis  
The time complexity of LU decomposition is O(n^3), where n is the size of the input matrix. This is because the algorithm involves performing Gaussian elimination on the matrix, which requires O(n^3) operations. The space complexity of the algorithm is also O(n^2), since it requires storing the L and U matrices, as well as the row permutation vector. However, the algorithm is numerically stable and is widely used in practice for solving systems of linear equations.