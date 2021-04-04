+++

title = "6-Performance Chracterstics of Radix Sorts"

+++

### Performance Characteristics of Radix Sorts

 Running time of LSD Radix sort for sorting N records with `w` byte keys is proportional to `Nw` , because algorithms makes `w` passes over all N keys.

For long keys and short bytes this running time is comparable to $N\lg N$.

*Property 1 :* The worst case for radix sorting is to examine all the bytes in all keys.

*Property 2 :* Binary quicksort examines about $N\lg N$ bits, on average, when sorting keys composed of random bits.

*Property 3 :* MSD radix sort with radix `R` on a file of size `N` requires at least $2N+2R$ steps.

*Property 4 :* If the radix is always less than the file size, the number of steps taken by MSD radix sort is within a small constant factor of $N\log_R N$ on the average ( for keys compromising random bytes ), and within a small constant factor of the number of bytes in the keys in the worst case.

*Property 5 :* Three-way quicksort uses $2N \lg N$ byte comparisons, on the average, to sort N( arbitrarily long ) keys.

*Property 6 :* LSD radix sort can sort N records with w-bit keys in $\frac{w}{\lg R}$ passes, using extra space for R counters (and a buffer for rearranging the file).