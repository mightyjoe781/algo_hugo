+++

title = "2-Pushdown Stack ADT"
weight = 2
+++

## Pushdown Stack ADT

A pushdown stack is an ADT that comprises two basic operations: insert **(push)** a new item, and remove **(pop) **the item that was most recently inserted.

- items are removed according to a last-in, first-out (***LIFO***) discipline.

- Each push increases the size of the stack by 1 and each pop decreases the size of the stack by 1.

- **Pushdown-stack ADT interface**

  ````c++
  template <class Item>
  class STACK
      {
      private:
      	// Implementation-dependent code
      public:
          STACK(int);
          int empty() const;
          void push(Item item);
          Item pop();
      };
  ````

- First line in above interface gives  a C++ template to the class.

  `STACK<int> save(N)`

  This declaration specifies that the type of items on stack named `save` is to be `int` (and max number of item in the stack might need to hold at any one time is N).