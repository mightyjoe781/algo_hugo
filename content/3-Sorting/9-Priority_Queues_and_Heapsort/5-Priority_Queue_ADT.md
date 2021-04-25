+++

title = "5-Priority Queue ADT"

+++

### Priority Queue ADT

**Full priority-queue ADT**

This interface allows client programs to delete items and to change priorities (using handles provided by the implementation  ) and to merge priority queues together.

````c++
template <class Item>
class PQ
{
    private :
		//Implementation-dependent code
    pubilc :
    	//Implementation-dependent handle definition
    	PQ(int);
    	int empty() cons;
    	handle insert(Item);
    	Item getmax();
    	void change(handle,Item);
    	void remove(handle);
    	void join(PQ<Item>&);
};
````

This arrangement restricts both client program and implementation that client program is not given a way to access information through handles except through this interface. So responsibility falls to use handles properly ( checking illegal actions).

For its part  the implementation cannot move around information freely because client programs have handles that they may use later.

Client program keeps the responsibility for maintaining the records and keys, and PQ routines refer to them indirectly.

***Unordered doubly linked list priority queue***

````c++
template <class Item>
    classPQ{
    private:
    	struct node
        	{ Item item; node *prev, *next;
             node(Item v) {item = v ; prev = 0 ; next = 0;
        	};
             typedef node *link;
             link head,tail;
   	public:
             typedef node* handle;
             PQ(int = 0)
             	{
                 head = new node(0); tail = new node(0);
                 head->prev - tail; head->next = tail;
                 tail->prev = head; tail->next = head;
             	}
             int empty() const
             	{ return head->next->next == head;}
             handle insert(Item v)
             	{ handle t = new node(v);
                  t->next = head->next; t->next->prev = t;
                  t->prev = head ; head->next = t;
                  return t;
             	}
             Item getmax();
             void change(handle,Item);
             void remove(handle);
             void join(PQ<Item>&);
             Item getmax()
                { Item max; link x = head->next;
                  for (link t = x; t->next != head; t = t->next)
                  if (x->item < t->item) x = t;
                  max = x->item;
                  remove(x);
                  return max;
                }
             void change(handle x, Item v)
                { x->key = v; }
             void remove(handle x)
                {
                x->next->prev = x->prev;
                x->prev->next = x->next;
                delete x;
                }
             void join(PQ<Item>& p)
                {
                tail->prev->next = p.head->next;
                p.head->next->prev = tail->prev;
                head->prev = p.tail;
                p.tail->next = head;
                delete tail; delete p.head;
                tail = p.tail;
                }
};
````

