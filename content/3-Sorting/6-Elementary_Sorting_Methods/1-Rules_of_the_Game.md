+++

title = "1-Rules of the Game"

+++

#### Introduction

Why study these elementary techniques of sorting

- provide context - learning terminology and basic mechanisms
- sometimes more effective than the more powerful general purpose methods
- extend to better general purpose methods

### Rules of the Game

Abstract notion of <u>putting keys and associated information in order</u> is what characterizes the *sorting* problem.

- Internal Sorting : file to be sorted fits in memory
- External Sorting : file from tape or disk

Some Conventional Code :

*Note  : template allows for sorting method to be used for any type of Data Type*

````c++
#include <iostream.h>
#include <stdlib.h>
template <class Item>
   void exch(Item &A, Item &B)
	{Item t = A; A = B ; B = t;}
template <class Item>
    void compexch(Item &A,Item &B)
	{if (B<A) exch(A,B);}
template <class Item>
    void sort(Item a[],int l , int r)
	{ for(int i = l+1; i <= r ; i++)
        for(int j = i ; j> l ; j--)
            compexch(a[j-1],a[j]);
	}
int main(itn argc,char *argv[]){
    int i, N = atoi(argv[1]), sw = atoi(argv[2]);
    int *a = new int[N];
    if(sw)
        for(i = 0 ; i < N ; i++)
            a[i] = 1000*(1.0 * rand()/RAND_MAX);
    else
    	{ N =  0; while(cin>>a[N]) ; N++;}
    sort(a,0,N-1); //A variant of Insertion Sort
    for(int i = 0 ; i <N ; i++) cout<<a[i]<<" ";
    cout<<endl;
}
````

- Non-Adaptive Sorts : based on compare-exchange operation only ( Better suited for hardware implementation)
- Adaptive Sorts : performs different sequences of operation, depending on the outcomes of comparisons (invocation of `operator<`)

*A sorting method is said to be <u>stable</u> if it preserves the relative order of items with duplicated keys in the file*.

If items are large then rather than moving items around do an indirect sort.(sort an array of pointers)