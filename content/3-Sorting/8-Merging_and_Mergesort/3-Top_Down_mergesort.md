+++

title = "3-Top Down mergesort"

+++

### Top-Down Mergesort

Mergesort is important because it is a straightforward optimal sorting method 9 it runs in time proportional to $n\log n $ that can be implemented in a stable manner.

It is classic example of the divide and conquer paradigm.

````c++
template <class Item>
void mergesort(Item a[],int l , int r)
{
    if(r<= l ) return ;
    int m = (r+l)/2 ;
    mergesort(a,l,m);
    mergesort(a,m+1,r);
    merge(a,l,m,r);
}
````

- Mergesort requires about $n \log n $ comparisons to sort any file of $N$ elements
- Mergesort uses extra space proportional to $N$.
- Mergesort is stable, if underlying merge is stable.
- The resource requirements of mergesort are insensitive to the initial order of its input.

