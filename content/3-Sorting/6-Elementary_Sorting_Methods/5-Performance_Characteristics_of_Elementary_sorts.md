+++

title = "5-Performance Characteristics of Elementary sorts"

+++

#### Performance Characteristics of Elementary Sorts

Selection sort, Insertion sort, and bubble sort are all quadratic time algorithms both in worst and in average case, and none requires extra memory.

Insertion sort never looks ahead and Selection sort never looks back.

Changin the scan through the array to alternate between beginning to end and end to beginning gives a version of bubble sort called ***Shaker sort*** which finishes more quickly.

**Definition**

- An *inversion* is a pair of keys that are out of order in the file

**Properties**

- Selection sort uses $\frac{N(N-1)}{2}$ comparisions and N exchanges.
- Insertion sort uses about $\frac{N^2}{4}$ comparisions and $\frac{N^2}{4} $ half-exchanges (moves) on the average, and twice that many at worst.
- Bubble Sort uses about $\frac{N^2}{2}$ comparision and $\frac{N^2}{2}$ exchanges on the average and in worst case.
- Insertion Sort and Bubble sort use a linear number of comparisons and exchanges for files with at most a constant number of inversions corresponding to each element.
  - Given file is partially sorted Insertion and Bubble performs the sorting in linear time almost but Selection sort still takes quadratic time.
- Insertion Sort uses a linear number of comparisons and exchanges for files with at most a constant number of elements having more than a constant number of corresponding inversions.
  - Given sorted file and we append some keys at the end then Insertion sort performs in linear time but Bubble sort not.
- Selection Sort runs in linear time for files with large item and small keys.



**Important Facts**

​	Insertion sort and bubble sort performs same in case of file with identical keys.

​	