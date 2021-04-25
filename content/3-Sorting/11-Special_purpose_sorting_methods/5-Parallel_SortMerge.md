+++

title = "5-Parallel SortMerge"

+++

### Parallel Sort-Merge

The abstract model for parallel processing involves following assumptions.

- N records to be sorted and
- P processors, each capable of holding N/P records.

Processors are labeled as 0,1, . . . , P-1 and assume that input is in the local memories of processors.

The goal of the sort is to rearrange the records to put smallest N/P records in processor 0's memory and so on in sorted order.

There is direct tradeoff between P and the total running time.

- assume communication between processors is more costly than references to local memory, that it is most efficiently done sequentially in large blocks.

*Definition* : A *merging* comparator takes as input two sorted files of size M, and produces as output two sorted files : one containing the M smallest of the 2M inputs, and the other containing the M largest of the 2M inputs.

***Property 7*** : We can sort a file of size N by dividing it into N/M blocks of size M, sorting each file, then using a sorting network built with merging comparators.

***Property 8*** : Block sorting on P processors, using Batcher's sort with merging comparators, can sort N  records in about $(\lg P)^2/2$ parallel steps.