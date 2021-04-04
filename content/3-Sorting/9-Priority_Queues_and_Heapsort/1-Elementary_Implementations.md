+++

title = "1-Elementary Implementations"

+++

### Elementary Implementations

we can use ordered or unordered sequences, implemented as linked list or arrays. Tradeoff between leaving the items unordered and keeping them in order is

Ordered Sequences allows constant time *remove the maximum* and *find the max* but going through the whole list for insert. (eager approach)

Unordered Sequences allows constant time *insert* but traversing entire structure for finding the maximum or removing the maximum (lazy approach)

We can use an array or linked-list representation in either case, with the basic tradeoff that linked list allow constant-time *remove* (and, in the unordered case *join*) but requires more space for links.

**Priority-queue example ( unordered array representation )**

````c++
template <class Item>
class PQ
	{
    private:
    	Item *pq;
    	int N;
    public :
    	PQ(int maxN)
        	{ pq = new Item[maxN] ; N =0 ;}
    	int empty() const
        	{ return N == 0;}
    	void insert(Item item)
        	{ pq[N++] = item; }
    	Item getmax)()
        	{ int max = 0;
             for(int j = 1; j <N ; j++)
            	if(pq[max] < pq[j] ) max = j;
             	exch(pq[max],pq[N-1]);
             return pq[--N];
            }
	};
````

