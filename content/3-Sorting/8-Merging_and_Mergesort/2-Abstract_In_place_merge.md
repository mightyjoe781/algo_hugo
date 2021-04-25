+++

title = "2-Abstract In-place merge"

+++

### Abstract In-Place Merge

Although extra space for the auxiliary array seems to be a fixed practical cost. further improvements that allow s to avoid the extra time to copy the array.

 Second characteristic of basic merge that is worthy of note is that the inner loop includes two tests to determine whether the ends of two input arrays have been reached and most of the times there tests fail so we should sentinel keys to allow the tests to be removed.

That is, if elements with a key value larger than those of all other keys are added to the ends of the `a` and `aux` arrays, the tests can be removed, because, when the `a` (`b`) array is exhausted, the sentinel causes the next elements for the `c` array to be taken from the `b(a)` array until the merge is complete.

````c++
//merge in place without sentinels by copying the second array int oaux in reverse order back to back with the first (putting aux in bitonic order).
//acutuall merging starts in 3rd for loop
//first one and second sets i , j pointers.
template <class Item>
void merge(Item a[],int l, int m, int r){
    int i ,j ;
    static Item aux[maxN];
    for(i = m+1; i>l; i--) aux[i-1] = a[i-1];
    for(j = m; j< r ; j++) aux[r+m-j]= a[j+1];
    for(int k = l; k<= r ; k++)
        if(aux[j] < aux[i])
            a[k] = aux[j--]; else a[k] = aux[i++];
}
````

A sequence of keys that increases, then decreases or vice-versa is referred to as `bitonic` sequence.

Sorting a bitonic sequence is equivalent to merging but it is sometimes convenient to case a merging problem as a bitonic sorting problem

Above code is stable sort cause stable merging leads to stable sorting.