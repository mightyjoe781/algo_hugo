+++

title = "3-Insertion_sort"

+++

### Insertion Sort

we insert each element one at a time at its proper place among those already considered ( keeping them sorted ).

In selection sort elements to left are in sorted order and same happens in insertion sort but in insertion sort it is not the final order of elements.

This naïve implementation is shown in 6.1. Below is improved implementation.

Improvements in above schemes

1. we can stop doing `compexch` ops when we encounter a key that is not larger than the key in the item being inserted, because the subarray to the left is sorted and especially we can break out of the inner `for` loop in `sort` in program when condition `a[j-1] < a[j]` is true. 

   This changes the implementation into an adaptive sort, and speeds up the program by factor of 2 for randomly ordered keys.

   We have 2 conditionals that terminate the loop we can recode it again as `while` loop to reflect that fact explicitly.

2. A commonly used alternative is to keep the keys to be sorted in a[1] to a[N], and to put a <u>*sentinel*</u> key in a[0] , making it at least as small as the smallest key in the array. Then the test whether a smaller key has been encountered simultaneously test both conditions of interest, making inner loop smaller and the program faster.

````c++
void insertion(Item a[], int l, int r){
    int i ;
    for(i = r; i>l ; i--) compexch(a[i-1],a[i]); //this is explicit step to put the smallest key at the start of the array so that sentinel can be implemented.
    for( i = l+2 ; i <= r ; i++){
        int j = i ; Item v = a[i];
        while(v< a[j-1])
        	{ a[j] = a[j-1] ; j--;}
        a[j] = v;
    }
}
````

 3. The third one is consider removing extraneous instructions from the inner loop. It follows from noting that successive exchanges involving the same element are inefficient.

    eg we have 

    `t = a[j]; a[j] = a[j-1] ; a[j-1] = t;`

    followed by 

    `t = a[j-1] ; a[j-1] = a[j-2] ; a[j-2] = t;`

    the value of t doesn't even change , and we waste time storing it and then reloading it for next exchange.

    Above implementation avoids this and moves larger elements one position to the right instead of using exchanges, and thus avoid wasting time in this way.

***Insertion sort is stable.***