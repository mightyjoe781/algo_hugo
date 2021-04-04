+++

title = "3-Linear Probing"

+++

### Linear Probing

If we can estimate in advance the number of elements to be put into the hash table and have enough contiguous memory available to hold all the keys with some room to spare, then it is probably not worthwhile to use any links at all in the hash table.

Several methods have been devised that store N items in a table of size M>N, relying on empty places in the table to help with collision resolution. Such methods are called *open-addressing* hashing methods.

The simplest open-addressing method is called *linear probing* when there is a collision ( when we hash to a place in the table that is already occupied with an item whose key is not the same as the search key), then we just check the next position in the table. It is customary to refer to such a check (determining whether or not a given table position holds an item with key equal to the search key) as a *probe*.

There are only 3 possibilities for linear probing

1. search hit
2. search miss (the table position is empty)
3. search miss then you just do a linear probing until either search hit or empty table position is found.

if we want to insert an item we put it into the empty table space that terminated the search.

**Linear Probing**

for a sparse table (small a), we expect most searches to find an empty position with just a few probes. For a nearly full table ( a close to 1 ), a search could require a huge number of probes, could even fall into an infinite loop when the table is completely full.

Average cost of linear probing depends on the way in which items cluster together into contiguous groups of occupied table cells, called clusters, when they are inserted.

````c++
private:
	Item *st;
	int N, M;
	Item nullItem;
public:
	ST(int maxN)
    {
        N = 0; M = 2*maxN;
        st = new Item[M];
        for(int i = 0; i<M; i++) st[i] = nullItem;
    }
	int count() const {return N;}
	void insert(Item item)
    {
        int i = hash(item.key(),M);
        while(!st[i].null()) i = (i+1) % M;
        st[i] = item; N++;
    }
	Item search(Key v)
    {
        int i = hash(v,M);
        while(!st[i].null())
            if(v == st[i].key()) return st[i];
        else i = (i+1) % M;
        return nullItem;
    }
````

*Property :* When collisions are resolved with linear probing, the average number of probes required to search in a hash table of size $M$ that contains $N = aM$ keys is about $\frac 1 2 ( 1 + \frac {1}{(1-\alpha)})$ and $\frac 1 2 ( 1 + \frac {1}{(1-\alpha)^2})$ for hits and misses, respectively.

Search misses are always more expensive than hits, and both require only a few probes, on the average, in a table that is less than half full.

How do we delete a key from a table built with linear probing ?  we cannot remove it, because items that were inserted later might have skipped over that item, so searches for those items would terminate prematurely at the hole left by the deleted item.

One solution to this problem is to rehash all the items for which this problem could arise. those between deleted one and next unoccupied position to right.

in sparse table, this repair process will require only a few rehash operations, at most. Another way to implement deletion is to replace the deleted key with a sentinel key that can serve as a placeholder for searches but can be identified and reused for insertions.

**Removal from a linear-probing hash table**

````c++
void remove(Item x)
{
    int i = hash(x.key(),M),j;
    while(!st[i].null())
        if (x.key() == st[i].key()) break;
    	else i = (i+1)% M;
    if(st[i].null()) return;
    st[i] = nullItem; N--;
    for( j = i+1 ; !st[i].null(); j = (j+1)%M, N--)
    {Item v = st[j]; st[j] = nullItem; insert(v);}
}
````

