+++

title = "5-Median of three partitioning"

+++

### Median of Three Partitioning

choosing a partitioning element that is more likely to divide the file near the middle improves the performance.

There are several possibilities for choosing partitioning element

- a safe choice is to choose random element from array
  - then worst case will happen with small probability
  - Example of a *Probabilistic Algorithm*
  - but using a full random-number generator might be overkill for this algorithm
- Another well-known way to find is to take sample of three elements form the file, then use the median of the three for the partitioning element. By choosing the three elements from the left, middle , and right of the array, we can incorporate sentinels into this scheme as well.
- Then exchange the one in the middle `a[r-l]`, and then run the partitioning algorithm `a[l+1],...,a[r-2]` . This improvement is called `median-of-three` method.

Improvements this method introduces to quicksort is

1. Makes worst case more unlikely to occur in any actual sort.
2. Eliminates the need for a sentinel key for partitioning
3. reduces the total average running time of the algorithm by about 5%

**Improved quicksort**

````c++
static const int M = 10;
template <class Item>
void quicksort(Item a[], int l, int r){
    if(r-l <= M) return;
    exch(a[(l+r)/2],a[r-l]);
    compexch(a[l],a[r-1]);
    compexch(a[l],a[r]);
    compexch(a[r-1],a[r]);
    int i = partition(a,l+1,r-1);
    quicksort(a,l,i-1);
    quicksort(a,i+1,r);
}
template <class Item>
void hybridsort(Item a[],int l ,int r)
	{quicksort(a,l,r); insertion(a,l,r);}
````

