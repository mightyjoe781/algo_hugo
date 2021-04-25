+++

title = "2-Tries"

+++

### Tries

It is based on the same search methods that is driven by bits of keys but it also keeps keys in the tree in order. So we can support recursive implementation of sort and other symbol-table function as we did for BSTs.

The idea is to store keys only at the bottom of the tree, in leaf nodes. The resulting data structure has number of useful properties and serves as the basis for several effective search algorithms.

It was discovered by *de la Briandais* in 1959, and, because it is useful for retrieval, and given nae trie by *Fredkin* in 1960.

We use tries for keys that are fixed number of bits/var-length bitstrings. To simplify the discussion, we start by assuming that no search key is the prefix of another.

This condition is satisfied when the keys are of fixed length and are distinct.

*Definition 1* A *trie* is a binary tree that has keys associated with each of its leaves, defined recursively as follows : The trie for an empty set of keys is a null link; the trie for a single key is a leaf containing that key; and the trie for a set of keys of carnality greater that one is an internal node with left link referring to the trie for the keys whose initial bit is 0 and right link referring to the trie for the keys whose initial bit is 1, with leading bit considered to be removed for the purpose of constructing the subtrees.

To insert a key into a trie, we first perform a search, as usual. If the search ends on a null link, we replace that link with a link to a new leaf containing key, as usual. But if search ends on a leaf, we need to continue down the trie, adding an internal node for every bit where the search key and the key that we found agree, ending with both keys in leaves as children of internal node corresponding to the first bit position where they differ.

**Trie search**

````c++
private:
	Item searchR(link h, Key v, int d)
    { if(h==0) return nullItem;
      if(h->l == 0 && h->r == 0)
      	{ Key w = h->item.key();
          return (v==w) ? h->item: nullItem;}
     	if(digit(v,d) == 0)
            return searchR(h->l, v, d+1);
     	else return searchR(h->r, v, d+1);
    }
public:
	Item search(Key v)
    { return searchR(head, v, 0); }
````

**Trie Insertion**

he we distinguish to cases of search miss.

1.  if the miss was not on leaf, then we replace the null link that caused us to detect the miss with a link to a new node.

2. if miss was on leaf, then we use a function `split` to make one new internal node for each bit position where the search key and the key found agree, finishing with one internal node for the leftmost bit position where the keys differ.

   The `switch` statement in `split` converts the two bits thats testing into a number to handle the four possible cases. If the bits are the same( `00` or `11` ) then we continue splitting otherwise we stop.

````c++
private:
	link split(link p, link q, int d)
    {
        link t = new node(nullItem); t->N = 2;
        Key v = p->item.key(); Key w = q->item.key();
        switch(digit(v, d)*2 + digit(w, d))
        {
            case 0: t->l = split(p,q,d+1); break;
            case 1: t->l = p; t->r = q; break;
            case 2: t->r = p; t->l = q; break;
            case 3: t->r = split(p,q,d+1); break;
        }
        return t;
    }
	void insertR(link& h,Item x, int d)
    {
        if(h==0) { h = new node(x); return;}
        if(h->l == 0 && h->r == 0)
        	{ h = split(new node(x), h, d); return;}
        if(digit(x.key(),d) == 0)
            insertR(h->l, x, d+1);
        else insertR(h->r, x, d+1);
    }
public:
	ST(int maxN)
    { head = 0; }
	void insert(Item item)
    { insertR(head, item, 0); }
````

*Property 2:* The structure of a trie is independent of the key insertion order: There is unique trie for any given set of distinct keys.

*Property 3:* Insertion or search for a random key in a trie built form N random (distinct) bitstrings requires about $\lg N$ bit comparisons on the average. The worst-case number of bit comparisons is bounded only by the number of bits in the search key.

*Property 4:* A trie built from N random $w$ - bit keys has about $\frac{N}{\ln 2} \approx 1.44 N$ nodes on average.

