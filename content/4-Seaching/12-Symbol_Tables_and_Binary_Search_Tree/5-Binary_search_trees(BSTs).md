+++

title = "5-Binary search trees(BSTs)"

+++

### Binary Search Trees (BSTs)

To overcome problem of expensive insertion we use an explicit tree structure as basis of Symbol-Table implementations.

*Definition 2 :* A binary search tree (BST) is a binary tree that has a key associated with each of its internal nodes, with the additional property that the key in any node is larger than (or equal to ) the keys in all nodes in that node's left subtree and smaller than (or equal to) the keys in all nodes in that node's right subtree.

**BST based symbol table**

````c++
template <class Item, class Key>
class ST
{
    private :
    	struct node
        { Item item; node *l,*r;
          node(Item x)
          { item = x; l=0; r=0;}
        };
    	typedef node *link;
    	link head;
    	Item nullItem;
    	Item searchR(link h,Key v)
        {
            if(h==0) return nullItem;
            Key t = h->item.key();
            if(v == t)return h->item;
            if(v<t) return searchR(h->l,v);
            else return searchR(h->r, v);
        }
    	void insertR(link& h, Item x)
        {
            if(h==0) { h=new node(x); return;}
            if(x.key()< h->item.key())
                insertR(h->l,x);
            else insertR(h->r,x);
        }
    public:
    	ST(int maxN)
        { head = 0; }
    	Item search(Key v)
        { return searchR(head,v);}
    	void insert(Item x)
        {insertR(head,x);}
}
````

This search procedure stops either when search hit or miss (subtree becomes empty)

sort function for symbol tables is available with little extra work when BSTs are used. Constructing a binary search tree amounts to sorting items, since a binary search tree represents a sorted file when we look at it the right. Simple inorder traversal does the job.

**Sorting with a BST**

````c++
private:
	void showR(link h, ostream& os)
    {
        if(h==0) return;
        showR(h->l, os);
        h->item.show(os);
        showR(h->r, os);
    }
public:
	void show(ostream& os)
    { showR(head,os);}
````

**Insertion in BSTs (nonrecursive approach)**

````c++
void insert(Item x)
{	Key v = x.key();
	if(head==0)
    {	head = new node(x); return; }
 	link p = head;
 	for(link q = p; q!=0; p=q?q:p)
        q = (v<q->item.key()) ? q->l ; q->r;
 	if(v<p->item.key()) p->l = new node(x);
 				else 	p->r = new node(x);
}
````

There is no check for duplicates here and whenever they appear they appear scattered throughout the tree.

BSTs are dual to quicksort. The node at the root of tree corresponds to the partitioning element in quicksort( no keys to the left are larger, and no keys to right are smaller)

