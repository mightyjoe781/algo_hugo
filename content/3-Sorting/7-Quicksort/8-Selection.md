+++

title = "8-Selection"

+++

### Selection

Finding the median of a set doesn't require us to completely sort the array.

Operation of finding the median is a special case of the operation of selection : finding the `kth` smallest of a set of numbers. Since we cannot say that a item is `kth` smallest without examining `k-1` elements that are smaller and `N-k` elements that are larger, most selection algorithms can return all the `k` smallest elements of a file without a great deal of extra calculations.

````c++
template <class Item>
void select(Item a[], int l, int r ,int k ){
    if (r<= l) return;
    int i = partition(a,l,r);
    if(i>k) select(a,l,i-1,k);
    if(i<k) select(a,i+1,r,k);
}
````



**Non-Recursive Selection**

````c++
template <class Item>
void select(Item a[],int l,int r , int k){
    while(r>l){
        int i = partition(a,l,r);
        if(i >= k) r = i - 1;
        if(i <= k) l = i + 1;
    }
}
````



- *Quicksort-based selection is linear time on the average*