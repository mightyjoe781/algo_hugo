+++

title = "7-Sublinear Time Sort"

+++

### Sublinear-Time Sorts

from previous section we can conclude that the running time of radix sorts can be sublinear in the total amount of information in the keys.

LSD radix sort implementation makes `bytesword` passes through the file. By making `R` large, we get an efficient sorting method, as long as N is also large and we have space for R counters.

Reasonable choice is to make $\lg R$ about one-quarter of the word size, so that the radix sort is four key-indexed counting passes. Each byte of each key is examined, but there are only four digits per key.

***Dynamic characteristics of LSD radix sort on MSD bits***

When keys are random bits, sorting the file on the leading bits of keys brings it nearly into order.

For some file sizes, it might make sense to use the extra space that would otherwise be used for the auxiliary array to try to get by with just one key-indexed-counting pass, doing the rearrangement in place.

LSD approach to radix sorting is widely used, because it involves extremely simple control structures and its basic operation are suitable for machine language implementation, which can directly adapt to special purpose high-performance hardware.