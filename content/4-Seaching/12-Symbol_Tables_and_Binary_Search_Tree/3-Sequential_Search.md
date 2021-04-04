+++

title = "3-Sequential Search"

+++

### Sequential Search

when general keys are too large range for them to be used as indices one approach is to store contiguously. when new item is inserted, we put it into array by moving larger elements over one position as we did for insertion sort; when a search is to be performed we look through the array sequentially.

Since Array is ordered, we can report a search miss when we encounter a key larger than the search key.

**Array-based symbol table (ordered)**

````c++
template <class Item, class Key>
class ST
{
    private:
    	Item nullItem, *st;
    	int N;
    public:
    	ST(int maxN)
        { st = new Item[maxN+1]; N=0; }
    	int count()
        { return N; }
    	void insert(Item x)
        { int i = N++; Key v = x.key();
           while(i>0 && v<st[i-1].key())
           {st[i] = st[i-1]; i--;}
        	st[i] = x;
        }
    	Item search(Key v)
        {
            for(int i = 0; i<N; i++)
                if(!(st[i].key() < v)) break;
            if(v == st[i].key()) return st[i];
            return nullItem;
        }
    	Item select(int k)
        { return st[k];}
    	void show(ostream& os)
        {
            int i = 0;
            while(i<N) st[i++].show(os);
        }
}
````

slight improvement : we can include sentinel in search to eliminate the test for running off the end of the array in case that no item in the table has the search key. we can reserve space for it at the end of the array as the sentinel, then fill its key field with search key before a search. Then the search will always terminate with an item containing the search key, and we determine whether or not the key was in the table by checking whether that item is the sentinel.

alternatively we could accept items in any order and then do sequential search on the array. Characteristic property of this approach is insert is fast but select and sort require substantially more work.

Another straightforward option is to use Symbol Table implementation is to use a linked list. Easy to maintain order to easily sort or leave it unordered for fast insert.

Only advantage is we don't need to know the max size of table in advance, disadvantage is the need of extra space for links and we can't support select efficiently.

*Property 2:* Sequential search in a symbol table with N items uses about N/2 comparisons for search hits.

*Property 3:* Sequential search in a symbol table of N unordered items uses a constant number of steps for inserts and N comparisons for search misses (always)

*Property 4:* Sequential search in a symbol table of N ordered items uses about N/2 comparisons for insertion, search hits, and search misses (on the average).

If we have huge number of search operation in a small table, then keeping the items in order is worthwhile. If items with duplicate keys may be kept in the table, we can have constant time insert implementation with unordered table.

Unordered Table is preferred in scenario where a huge number of inserts but few search operation are there.