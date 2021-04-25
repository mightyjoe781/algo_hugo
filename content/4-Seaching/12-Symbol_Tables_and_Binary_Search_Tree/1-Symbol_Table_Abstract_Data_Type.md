+++

title = "1-Symbol Table Abstract Data Type"

+++

### Introduction

*A symbol table is a data structure of <u>items with keys</u> that supports two basic operations*

- *Insert a new item*
- *Return an item with a given key*

Also known as dictionaries sometimes.

### Symbol-Table Abstract Data Type

Operations of interest in the implementations are

- *insert*
- search for a item given the key
- *remove* a item
- *sort* the symbol table
- *join* two symbol table

Additionally we might also want a standard construct,test if empty , destroy and copy. for example, a search-and-insert operation is often attractive in some implementation.

Symbol Tables are so important to so many computer application that they are available as high-level abstraction in many environments. The C library has `bsearch`, an implementation of binary search, C++ STL offers a large variety of symbol tables, called "associative containers".

Instead of talking about specific container we will use items and keys and overload `==` and `<` to compare the keys

**Sample implementation for item ADT**

````c++
#include <stdlib.h>
#include <iostream.h>
static int maxKey = 1000;
typedef int Key;
class Item
{
    private:
    	Key keyval;
    	float info;
    public:
    	Item()
        {keyval = maxKey;}
    	Key key()
        {return keyval;}
    	int null()
        {return keyval == maxKey;}
    	void rand()
        {keybal = 1000*::rand()[/]RAND_MAX;
         info = 1.0*::rand()[/]RAND_MAX; }
    	int scan(istream& is = cin)
        {return (is>>keyval>>info)!=0;}
    	void show(ostream& os = cout)
        {os<<keyval<<" "<<info <<endl;}
}
ostream& operator<<(ostream& os, Item& x)
{x.show(os); return os;}
````

**Symbol-table abstract data type**

````c++
template <class Item, class Key>
class ST
{
    private:
    	//Implemenatation-dependent code
    public:
    	ST(int);
    	int count();
    	Item search(Key);
    	void insert(Item);
    	void remove(Item);
    	Item select(Item);
    	void show(ostream&);
};
````

**Example of a symbol-table client**

````c++
#include <iostream.h>
#include <stdlib.h>
#include "Item.cxx"
#include "ST.cxx"
int main (int argc, char *argv[])
{
    int N, maxN=atoi(argv[1]),sw = atoi(argv[2]);
    ST<Item, Key> st(maxN);
    for( N =0 ; N<maxN; N++){
        Item v;
        if(sw) v.rand(); else if (!v.scan()) break;
        if(!(st.search(v.key())).null()) continue;
        st.insert(v);
    }
    st.show(cout); cout<<endl;
    cout<<N<<" keys"<<endl;
    cout<<st.count() <<" distinct keys"<<endl;
}
````

