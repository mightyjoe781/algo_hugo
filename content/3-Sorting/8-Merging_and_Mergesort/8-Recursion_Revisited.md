+++

title = "8-Recursion Revisited"

+++

### Recursion Revisited

Quicksort might be called a proper conquer and divide algorithm.

In a recursive implementation, most of the work for a particular activation is done before the recursive calls.

On the other hand, the recursive mergesort has more the spirit of divide and then conquer.

Quicksort starts with processing on the largest subfile, and finishes up with the small ones.

Quicksort is more naturally thought of top-down algorithm, because it does work at the top of the recursion tree, then proceeds down to finish the sort.

Quiksort and Mergesort differ in the issue of stability.

However straight forward implementation of quicksort for linked lists is, however, stable.