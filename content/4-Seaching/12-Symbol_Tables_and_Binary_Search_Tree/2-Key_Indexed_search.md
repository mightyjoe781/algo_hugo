+++

title = "2-Key Indexed Search"

+++

## Key-Indexed Search

suppose key values are distinct small numbers, then simplest search algorithm is based on storing the items in array, indexed by the keys.

Property 1: If key values are positive integers less than M and items have distinct keys, then the symbol-table data type can be implemented with key-indexed arrays of items such that *insert,search, and remove* require constant time; and *initialize,select and sort* require time proportional to M, whenever any of the operation are performed on an N-item table.

**Key-indexed-array-based symbol table**

````c++
template <class Item,class Key>
class ST
{
    private:
    	Item nullItem, *st;
    	int M;
    public:
    	ST(int maxN)
        { M = nullItem.key(); st = new Item[M];}
    	int count()
        {int N=0;
        for(int i=0;i<M;i++)
        if(!st[i].null()) N++;
        return N;
        }
    	void insert(Item x)
        {st[x.key()] = x;}
    	Item search(Key v)
        {return st[v];}
    	Item remove(Item x)
        { st[x.key()] = nullItem;}
    	Item select(int k)
        { for(int i = 0; i<M ; i++)
        	if(!st[i].null())
                if(!st[i].null())
                    if(k--=0) return st[i];
         return nullItem;
        }
    	void show(ostream& os)
        {for(int i=0;i<M;i++)
        if(!st[i].null()) st[i].show(os);}
}
````

This implementation doesn't handle duplicates well.

