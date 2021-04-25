+++

title = "1_Randomized BSTs"

+++

### Randomized BSTs

To analyze the average-case performance costs for binary search trees, we made the assumption that the items are inserted in random order. It is also possible to introduce randomness into the algorithm so that the property holds without any assumptions about the order in which the items are inserted.

The idea is simple : When we insert a new node into a tree of `N` nodes the new node should appear at the root with probability $\frac{1}{(N+1)}$ , so we randomized decision to use root insertion with that probability. Otherwise, we recursively use the method to insert the new record into the left subtree if the record's key is less than the new record's key is greater.

Viewed non recursively, doing randomized insertion is equivalent to performing a standard search for the key making a randomized decision at every step whether to continue the search or to terminate it and do root insertion. Thus, the new node could be inserted anywhere on its search path.

*Property 13.1* Building a randomized BST is equivalent to building a standard BST from a random initial permutation of the keys. We use about $2N\ln N$ comparisons to construct a randomized BST with $N$ items (no matter in what order the items are presented for insertion), and about $2\lg N$ comparisons for searches in such a tree.

**Randomized BST Insertion**

````c++
private:
	void insertR(link& h,Item x)
    {
        if(h==0) { h = new node(x); return;}
        if(rand()<RAND_MAX/(h->N+1))
        	{ insertT(h,x); return;}
        if(x.key() < h->item.key())
            insertR(h->l,x);
        else insertR(h->r,x);
        h->N++;
    }
public:
	void insert(Item x)
    { insertR(head,x);}
````

*Property 13.2* The probability that the construction cost of a randomized BST is more than a factor of a times the average is leass that $e^{-a}$

**Randomized BST combination**

````c++
private:
	link joinR(link a, link b)
    {
        if (a==0) return b;
        if (b==0) return a;
        insertR(b,a->item);
        b->l = joinR(a->l,b->l);
        b->r = joinR(a->r,b->r);
        delete a[;;] fixN(b); return b;
    }
public:
	void join(ST<Item, Key>& b)
    {
        int N = head->N;
        if(rand()/(RAND_MAX/(N+b.head->N)+1)<N)
            head = joinR(head, b.head);
        else head = joinR(b.head, head);
    }
````

**Deletion in a randomized BST**

````c++
link joinLR(link a, link b)
{
    if(a == 0) return b;
    if(b == 0) return a;
    if(rand()/(RAND_MAX/(a->N+b->N)+1) < a->N)
    	{ a->r = joinLR(a->r,b); return a;}
    else {b->l = joinLR(a,b->l); return b;}
}
````

Flaws that might be present in the randomized BSTs is that the inbuilt pseudo-number generator present in the system might actually spend a more of its time in generating the more random number that is not required here for the application of insertion.

second potential drawback of randomized BSTs is that they need to have a field in each node for the number of nodes in that node's subtree. Space might be at premium in some application so it might become liability.

*Property 13.3* Making a tree with an arbitrary sequence of randomized insert, remove, and join operations is equivalent to building a standard BST form a random permutation of the keys in the trees.