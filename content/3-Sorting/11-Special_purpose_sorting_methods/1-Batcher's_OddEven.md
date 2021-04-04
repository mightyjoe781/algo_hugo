+++

title = "1-Batcher's OddEven"

+++

### Batcher's Odd-Even Mergesort

To begin lets consider two abstract operation, the compare-exchange operation and the perfect shuffle operation (along with its inverse, the perfect unshuffle)

developed by Batcher in 1968.

More challenging to understand is how and why algorithm works rather than understanding how these operation are done.

*Definition 1 :* A nonadaptive sorting algorithm is one where the sequence of operations performed depends on only the number of inputs, rather than on the values of keys.

***Perfect shuffle and perfect unshuffle***

The *shuffle* function rearranges subarray by splitting the subarray in half, then alternating elements from each half : Elements in the first half go in the even-numbered position in the result, and elements in the second half go in the odd-numbered position in the result.

The *unshuffle* does exact opposite

````c++
template <class Item>
void shuffle(Item a[], int l,int r){
    int  i,j,m=(l+r)/2;
    static Item aux[maxN];
    for(i = l, j=0; i<=r; i+=2,j++)
    {aux[i] = a[l+j]; aux[i+1]=a[m+1+j];}
    for(i=l;i<=r;i++) a[i]=aux[i];
}
void unshuffle(Item a[], int l,int r){
    int  i,j,m=(l+r)/2;
    static Item aux[maxN];
    for(i = l, j=0; i<=r; i+=2,j++)
    {aux[l+j] = a[i]; aux[m+1+j]=a[i+1];}
    for(i=l;i<=r;i++) a[i]=aux[i];
}
````

***Batcher's odd-even merge(recursive version)***

````c++
template <class Item>
void merge(Item a[], int l, int m ,int r){
    if(r==l+1) compexch(a[l],a[r]);
    if(r<l+2) return;
    unshuffle(a,l,r);
    merge(a,l,(l+m)/2,m);
    merge(a,m+1,(m+l+r)/2,r);
    shuffle(a,l,r);
    for(int i = l+1; i<r ; i+=2)
        compexch(a[i],a[i+1]);
}
````

*Property 1 :* (0-1 principle ) If a nonadaptive program produces sorted output when the inputs are either 0 or 1, then it does so when inputs are arbitrary keys.

*Property 2 :* Batcher's odd-even merge is valid merging method.

