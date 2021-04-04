+++

title = "4-Stack ADT Implementation"
weight = 4
+++

## Stack ADT Implementations

- There are 2 implementations of Stack ADT

  - Arrays
  - Linked List

- #### **Array Implementation of a pushdown stack**

  - for N items it keeps in s[0],....,s[N-1]
  - top is s[N]
  - Client needs to pass the maximum # of items for stack

  ````c++
  template <class Item>
  class STACK
  {
      private:
      	Item *s; int N;
      public:
      	STACK(int maxN)
          	{ s = new Item[maxN]; N=0;}
      	int empty() const
          	{ return N==0; }
      	void push(Item item)
          	{ s[N++] = item; }
      	Item pop()
          	{ return s[--N];}
  }
  ````

  - Drawbacks of using this implementation
    - We need to know max. size beforehand

- #### Linked-list pushdown stack

  - we keep stack in reverse order from array implementation

  - To *pop* , we remove the node from front of list

  - To *push* , create a node and add to front of list

    ````c++
    template <class Item>
    class STACK
    {
        private:
        	struct node
            { Item item; node* next;
            	node(Item x, node* t)
                { item =x; next =t;}
            };
        	typedef node *link;
        	link head;
        public:
        	STACK(int)
            { head = 0;}
        	int empty() const
            { return head == 0;}
        	void push(Item x)
            { head = new node(x,head);}
        	Item pop()
            { Item v = head->item; link t = head->next;
            	delete head; head =t ; return v;}
    };
    ````

- Note both codes are not complete

  - Array Implementation doesn't check for out of bound errors.
  - Linked- list implementation doesn't check for popping from and empty node.