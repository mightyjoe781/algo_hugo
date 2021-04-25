+++

title = "3-External Sorting"

+++

### External Sorting

Another abstract problem is when file to sorted is much too large to fit in the RAM of computer. This situation is known as *external sorting*.

Consider two atomic operations

- read data from external storage into main memory
- write data from main memory into external storages

Cost of these two operations is very large than cost of primitive computational operation so we ignore them.

i.e. we ignore cost of sorting of main memory

read and write operations in most memory is generally done in large continuous blocks of data and also peak performance is achieved when we access the blocks in a sequential manner.

First step is to make a copy of the file.

Second step might be to implement a program to reverse the order of file.

The sorting methods we have seen till now are organized as a number of passes over all the data, and we usually measure of cost of an external sorting by simply counting these passes.

Since data is huge removing even one pass improves efficiency significantly.

Basic assumption is run time is dominated by input and output so run time of external sort is multiplication of no. of passes by the time required to read and write whole file.

- N records to be sorted on external device
- space in main memory to hold M records
- 2P external devices for use during sort

General strategy is make a first pass thru file to be sorted, breaking it up into blocks about the size of internal memory and sort these blocks. Then, merge the sorted blocks together, if necessary by making several passes through the file, creating successively larger blocks. This is called as *sort-merge*.

Simplest sort-merge strategy, which is called as *multiway merging*.

*Property 4* : With $2P$ external devices and internal memory sufficient to hold M records, a sort-merge that is based on a P-way balanced merge takes about $1+\lceil log_p(N/M) \rceil$ passes.

