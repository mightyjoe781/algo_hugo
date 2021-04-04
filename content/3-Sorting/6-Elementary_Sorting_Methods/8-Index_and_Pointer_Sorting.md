+++

title = "8-Index and Pointer Sorting"

+++

### Index and Pointer Sorting

The development of a string data type implementation is of particular interest as character strings are widely used as sort keys.

**Interface for String data type**

````c++
typedef struct record { char *str;} Item; //just one line changed
int operator<(const Item&, const Item&);
int scan(Item&);
void rand(Item&);
void show(const Item&);
````

*Note: here we put pointer in a struct because C++ does not allow us to overload operator< for built in types such as pointers*

***A class(or struct) that adjusts the interface to another data type is called a wrapper class.***

**Data-type implementation for string items**

````c++
#include <iostream.h>
#include <string.h>
#include <string.h>
#include "Item.h"
static char buf[100000];
static int cnt = 0;
int operator<(const Item& a , const Item& b)
	{return strcmp(a.str,b.str) < 0;}
void show(const Item& x)
	{ cout << x.str <<" ";}
int scan(Item& x){
    int flag = (cin >> (x.str = &buf[cnt]))!=0;
    cnt+=strlen(x.str)+1;
    return flag;
}
````

To specify that our sorts will be processing array indices, not just ordinary integers. Our goal is to define a type `Index` so that we can overload `operator<` as follows:

````c++
int operator<(const Index& i , const Index& j )
	{return data[i]<data[j];}
````

If we have an array `a` of object of type `Index` , then any of our sort functions will rearrange the indices in `a` to make `a[i]` specify the number of keys that are smaller than `data[i]` (index of `a[i]` in the sorted array). To define the `Index` we use a wrapper class

````c++
struct intWrapper{
    int item;
    intWrapper(int i =0 )
    	{item = i ;}
    operator int() const
 	   {return item;}
};
typedef intWrapper Index;
````

Constructor in this `struct` converts any `int` to an `Index` and the cast ` operator int()`  converts any `Index` back to an `int` , so we can use objects of type `Index` anywhere that we can use objects of built-in type `int`.

An example of indexing , with the same items sorted by two different keys, is shown in book. One Client program can define `operator<` to use one key and another client can define `operator<` to use another key, but both can use the same sort program to produce an index array that allows them to access the items in order of their respective keys.

