+++

title = "2-Performance Characterstics of Quicksort"

+++

### Performance Characteristics of Quicksort

- Quicksort uses about $\frac{N^2}{2}$ comparisons in the worst case.

  - Worst case is when array is already sorted.

- Above case is also worst in space as it will take $N$ recursions.

- Best case is when quicksort is when each partitioning stage divides the file exactly in half.

  -  $C_N = 2 C_{N/2} + N $
  - $C_N \approx  N \lg N $

- Quicksort uses about $2N \ln N$ comparisons on the average.

- Dynamic Characteristics of quicksort on various types of files in file dependent and order of partitioning.

  - Nearly ordered files perform worst.
  - Because they have many partitions.

