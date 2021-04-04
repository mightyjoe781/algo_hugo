+++

title = "0-Introduction"

+++

### Introduction

Merging can be thought of as a complementary process to the selection that we read last time.

One of the most impressive property of merge sort is that it can sort a file in $N\log N$ , independent of input. ( Heapsort also does this)

Prime disadvantage of mergesort is that it requires extra space proportional to $N$.

A guaranteed $N\log N$ running time can be a liability. For e.g. we know we can sort special cases in linear time in some other sorting schemes. That is it become significant when there is already a order in the file.

Mergesort is stable sort. Quicksort and Heapsort are not stable.

Mergesort is a method of choice for sorting Linked Lists ( why ?) {

Linked list is a linear structure and merge sort access the structure in a linear fashion.

}