+++

title = "4-Improvements to basic algorithm"

+++

### Improvements to the Basic Algorithm

Just like we improved the quicksort using handling small cases differently we can improve performance of merge sort.

So recursion evidently says that for small values of recursion calls are most often so handling them with efficiency may lead to improvements.

Second improvements is consider for mergesort is to eliminate the time taken to the auxiliary array used for merging. So we can arrange recursion calls in a way that computation switches roles of input and auxiliary array at each level.

Below code is 40% more fast than the previous implementation but still 20% slower than the quicksort.

````c++
template <class Item>
void meregesortABr(Item a[],Item b[], int l, int r){
    if(r- l <= 10) {insertion(a,l,r); return ;}
    int m  = (l+r)/2;
    mergesortABr(b,a,l,m);
    mergesortABr(b,a,m+1,r);
    mergeAB(a+l,b+l,m-l+1,b+m+1,r-m);
}
template <class Item>
void mergesortAB(Item a[],int l ,int r){
    static Item aux[maxN];
    for(int i = l; i<= r ; i++) aux[i] = a[i];
    mergesortABr(a,aux,l,r);
}
````

