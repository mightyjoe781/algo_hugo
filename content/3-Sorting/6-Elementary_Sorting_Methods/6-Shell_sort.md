+++

title = "6-Shell_sort"

+++

#### Shellsort

Shellsort is a simple <u>extension</u> of insertion sort that allows exchanges of elements that are far apart.

Idea is to rearrange the file to give it the property that takes every $h^{th}$ element yields a sorted file. File is called as a *h-sorted* file.

*h-sorted file is h independent sorted files, interleaved together.*

One way to implement shellsort would be, for each h, to use insertion sort independently on each of the h sub files.

Now Big question is how to decide increment value $h$.

There no definitive answer to that but no provably best sequence has been found. In practice we generally we use sequences that decrease geometrically so number of increments is logarithmic in the size of file.

The increment sequence 1,4,13,40,121,364,1093,3280,etc this rule of one-thirds was given by Knuth.

````c++
template <class Item>
void shellsort(Item a[],int l , int r){
    int h;
    for(h = 1; h <= (r-1)/9 ; h = 3*h +1); // this loop to reach end of h max in multiples of 3
    for(; h > 0 ; h /= 3)
        for(int i = l+h ; i<=r ; i++){
            int j = i; Item v = a[i];
            while(j>= l+h && v < a[j-h]){
                a[j] = a[j-h]; j-=h;
            }
            a[j] = v;
        }
}
````

Some properties

- The result  of h-sorting a file that is k-ordered is a file that is both h- and k-ordered.
- Shellsort does less than $\frac{N(h-1)(k-1)}{g} $ comparisons to g-sort a file that is h-and k-ordered, provided that $h$ and $k$ are relatively prime
- Shellsort does less than $ O (N^{\frac{3}{2}})$ comparisons for the increments 1 4 13 40 121 364 1093 3280 9841 ...
- Shellsort does less than $ O (N^{\frac{4}{3}})$ comparisons for the increments 1 8 23 77 281 1073 4193 16577 ...
- Shellsort does less than $O(N(log N)^2)$  comparisons for the increments 1 2 3 4 6 9 8 12 18 27 16 24 36 54 81 ... (Pratt's method of triangle increments)

![image-20201015082022311](/6-Shell_sort.assets/image-20201015082022311.png)

Shell sort is not stable.

