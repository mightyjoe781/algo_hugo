+++

title = "0-Introduction"

+++

*A priority queue is a data structure of items with keys that supports two basic operation*

1. insert a new item
2. remove the item with the largest key

Its most important example of generalized queue ADT and in fact PQ is proper generalization of the stack and the queue because we can implement these DS using priority queues.

Applications of PQ include simulations systems ( keys ~ event times ) , job scheduling (keys ~ priorities ) , numerical computations ( keys ~ computation errors )

PQ is fundamental abstraction in understanding some basic graph-searching algorithms.

Following are the client expectations of of priority queues implementation

- *Construct* a PQ from N given Items.
- *Insert* a new item
- *remove* the maximum item
- *Change the priority* of an arbitrary specified item
- *Remove* and arbitrary specified item
- *Join* two PQ into one large one.

 **Basic priority-queue ADT**

````c++
template <class Item>
class PQ
{
    private :
    	// implementation dependent code
    public : 
    	PQ(int);
    	int empty() const;
    	void insert(Item);
    	Item getmax();
};
````

