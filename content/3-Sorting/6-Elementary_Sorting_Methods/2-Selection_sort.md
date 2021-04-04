+++

title = "2-Selection_sort"

+++

#### Selection Sort

we repeatedly select smallest remaining element and swap it with the current one.

\# of exchanges = N-1 ( no need to check last element)

\# of comparison = $N^2$

So comparisons dominate the running time.

````c++
void selection(Item a[], int l , int r){
    for(int i = l ; i <r ; i++){
        int min  = i ;
        for(int j = i+1 ; j <=r ; j++)
            if(a[j]<a[min]) min = j ;
        exch(a[i],a[min]);
    }
}
````

It takes about the same time for a already sorted file or file with same data , as it does for a randomly ordered file.

Selection Sort outperforms more sophisticated methods in one important application; it is the method for sorting files with huge item and small keys. `(cost of moving > cost of making comparisons)`

It is not stable.

