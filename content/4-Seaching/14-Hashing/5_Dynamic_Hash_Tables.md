+++

title = "5-Dynamic Hash Tables"

+++

### Dynamic Hash Tables

As the number of keys in a hash table increases, search performance degrades.

With separate chaining, the search time increases gradually-when the number of keys in the table doubles, the search time doubles. The same is true of open addressing methods linear probing and double hashing for sparse tables but cost increases as the table fills up and worse we reach a point where no more keys can be inserted at all While in Trees this cost increases slightly whenever the number of nodes in the tree doubles.

One way is to double the size of hash table as it starts to fill up. Doubling is and expensive operation because everything need to reinserted but it is an infrequent operation.

**Dynamic Hash insertion ( for linear probing )**

````c++
private:
	void expand()
    {
        Item *t = st;
        init(M+M);
        for (int i = 0; i< M/2 ; i++)
            if(!t[i].null()) insert(t[i]);
        delete t;
    }
public:
	ST(int maxN)
    { init(4);}
	void insert(Item item)
    { int i = hash(item.key(), M);
      while(!st[i].null()) i = (i+1)%M;
     st[i] = item;
     if(N++ >= M/2) expand();
    }
````

*Property :* A sequence of `t` search, insert, and delete symbol-table operations can be executed in time proportional to `t` and with memory usage always within a constant factor of the number of keys in the table.

