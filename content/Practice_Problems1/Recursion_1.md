+++

title = "Recursion 1"
date = 2021-05-13T23:01:25+05:30
weight = 3

+++

### Recursion

- What / How -> First Part (Pre-read)
- Writing your own recursive solution (frameworks)

What is Recursion

- Function calling itself
- Divide and Conquer
- Mathematical Concept!

**Call by value**

Memory Layout

- Stack (local function data : arguments and inside data)
- Heap (dynamic like malloc, new)
- Global (global variable, code)

Flow of execution

- Main
- Stack is allocated for every functions

**Call by reference**

- Change the value at calls
  - Explicit return type ( use struct to create new data type :D)
  - Implicit return type
- Space Optimized
  - don't need extra memory declaration in memory stack.

##### [Subsets](https://leetcode.com/problems/subsets/)

In case of set there is no notion of order!

[2,3,5]

**Subsets** : Pick zero or more elements from above array in no order. e. g. {2}, {3,5}, {2,5} and {5,2}  are identical !

**Subarray** : A set of contiguous set from the array. e. g. {3,5} , {2,3}  are subarray while {2,5} and {5,2} are not subarray.

**Subsequences** : A set of elements of array taken in order. e. g. {2,5} is subsequence while {5,2} is not.

Given size $n$ array.

Number of Subsets   : $2^n$

Number of Subarray : $\frac{n(n-1)}{2}$ + 1 (empty :D)

Number of Subsequences : $2^n$

