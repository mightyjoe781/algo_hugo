+++

title = "7-Sorting of Other type of data"

+++

### Sorting of Other type of data

In this section we talk about building implementation, interfaces, and client programs for sorting algorithms. Specifically, we consider interfaces for :

- *Items*, or generic objects to be sorted
- *Arrays* of Items

**Sort driver for arrays**

````c++
#inlcude <stdlib.h>
#include "Item.h"
#include "exch.h"
#include "Array.h"
main(int argc , char *argv[]){
    int N = atoi(argv[1]), sw = atoi(argv[2]);
    Item *a = new Item[N];
    if(sw) rand(a,N); else scan(a,N);
    sort(a,0,N-1);
    show(a,0,N-1);
}
````

This driver uses 3 explicit interfaces :

1. First for a data type that encapsulated the operations that we perform on generic items.
2. slight higher level implementation of `exch` and `compexch` for sort implementation
3. initialize and print and sort arrays

**Interface for array data type**

````c++
//Array.h interface defines high level functions for abstract items
template <class Item>
    void rand(Item a[],int N);
template <class Item>
    void scan(Item a[], int &N);
template <class Item>
    void show(Item a[], int l ,int r);
template <class Item>
    void sort(Item a[], int l, int r);
````

**Implementation of array data type**

````c++
#include <iostream.h>
#include <stdlib.h>
#include "Array.h"
template <class Item>
    void rand(Item a[],int N)
	{	for(int i= 0 ; i < N; i++) rand(a[i]);	}
template <class Item>
    void scan(Item a[],int &N){
    	for(int i = 0 ; i< N ; i++)
            if(!scan(a[i])) break;
    	N=i;
	}
template <class Item>
    void show(Item a[], int l ,int r){
    	for(int i = l; i <= r ; i++)
            show(a[i]);
    	cout<<endl;
	}
````

In similar manner we can define Item interfaces

**Interface for item data type**

````c++
typedef struct record { int key; float info ;} Item;
int operator<(const Item&, const Item&);
int scan(Item&);
void rand(Item&);
void show(const Item&);
````

**Implementation for item data type**

````c++
#include <iostream.h>
#include <stdlib.h>
#include "Item.h"
int operator<(const Item& A, const Item& B)
	{ return A.key < B.key; }
int scan(Item& x)
	{ return (cin>>x.key>>x.info)!= 0;}
void rand(Item$ x)
	{ x.key = 1000*(1.0 *rand() / RAND_MAX);
    	x.info =  1.0*rand()/RAND_MAX; }
void show(const Item& x)
	{ cout<<x.key<<" "<<x.info<<endl;}
````

