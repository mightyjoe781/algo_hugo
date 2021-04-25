+++

title = "4-Double Hashing"

+++

### Double Hashing

Clustering makes probing run slowly for nearly full tables. There is an easy way to virtually eliminate the clustering problem : double hashing.

The basic strategy is the same as for linear probing the only difference is that instead of examining each successive table position following a collision, we use a second has function to get a fixed increment to use for the probe sequence.

Second hash function must be chosen with some care. First we must exclude the case where the second hash function evaluates to 0, since that would lead to an infinite loop on the very first collision. Second, it is important that the value of the second hash function be relatively prime to the table size, since otherwise some of the probe sequences could be very short. One way to enforce  this policy is to make M prime and to choose a second hash function that returns values that are less than M.

` inline int hashtwo(Key v) { return (v%97)+1; }`

**Double Hashing**

````c++
void insert(Item item)
{
    Key v = item.key();
    int i = hash(v,M), k = hashtwo(v,M);
    while(!st[i].null()) i = (i+k) % M;
    st[i] = item; N++;
}
Item search(Key v)
{
    int i = hash(v,M), k = hashtwo(v,M);
    while(!st[i].null())
        if (v==st[i].key()) reutrn st[i];
    else i = (i+k)% M;
    return nullItem;
}
````

*Property :* When collisions are resolved with double hashing, the average number of probes required to search in a hash table of size M that contains $ N = aM $ keys is $\frac 1 \alpha \ln(\frac{1}{1-\alpha})$ and $\frac{1}{1-\alpha}$ for hits and misses respectively.

*Property :* We can ensure that the average cost of all searches is less than t probes by keeping the load factor less than for linear probing and less than 1 - 1/t for double hashing