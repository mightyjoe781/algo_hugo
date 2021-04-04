+++

title = "2 Separate Chaining"

+++

### Separate Chaining

Second component of hashing algorithm is to decide how to handle the case when two keys hash to the same address. Most straightforward method is to build for each table address, a linked list of the items whose keys hash to that address. Rather then maintaining a single list, we maintain M lists.

This method is traditionally called *separate chaining* because items that collide are chained together in separate linked list.

Keeping chains in order is preference according to need but here we only consider the quick insertion.

````c++
private:
	link* heads;
	int N,M;
public:
	ST(int maxN)
    {
        N = 0; M = maxN/5;
        heads = new link[M];
        for(int i = 0; i<M; i++) heads[i] = 0;
    }
	Item search(Key v)
    { return searchR(heads[hash(v,M)],v);}
	void insert(Item item)
    { int i = hash(item.key(),M);
      heads[i] = new node(item,heads[i]); N++; }
````

*Property 1:* Separate chaining reduces the number of comparisons for sequential search by a factor of M (on the average), using extra space for M links.

Mostly we use unordered list for separate chaining which give constant times insert and search misses are proportional to $\frac{N}{M}$ .

If huge numbers of search misses are expected, we can speed up the misses are expected, we speed up the misses by a factor of 2 by keeping the lists ordered at the cost of slower insert.

*Property 2:* In a separate-chaining hash table with M lists and N keys, the probability that the number of keys in each list is within a small constant factor of $\frac N M$ is extremely close to 1.

The foregoing analysis is an example of a classical *occupancy problem*, where we consider N balls thrown randomly into one of M urns, and analyze how the balls are distributed among the urns. For example, the Poisson approximation tells us that the number of empty lists is about $e^{-a}$. A more interesting result tells us that the average number of items inserted before the first collision occurs is about $\sqrt{\pi M /2} \approx 1.25\sqrt M$ . This result is solution to classical birthday problem. Which tells us that for M = 365 , that the average number of people we need to check before finding two with same birthday is about 24.

A second classical result tells us that the average number of items inserted before each list has at least one item is about $MH_M$. This is the solution of classical coupon collector problem. For example the same analysis tells us, for M =1280, that we would expect to collect 9898 baseball cards(coupons) before getting one for each of 40 players of 32 team in a series.

In separate chaining we typically choose M to be small enough that we are not wasting huge area of contiguous memory with empty links, but large enough that sequential search is the most efficient methods for the lists.

Hybrid methods like Binary search are not worth the trouble. and as rule of thumb we can take about one-fifth/tenth the number of keys expected be in the table, so that the lists are expected to contain about five/10 keys each.

