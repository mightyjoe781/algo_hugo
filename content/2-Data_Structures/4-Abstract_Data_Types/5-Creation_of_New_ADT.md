+++

title = "5-Creation of New ADT"
weight = 5
+++

## Creation of New ADT

- Generally we freeze the interface after a detailed inspection of user needs.

- We can different client codes and implementation for the same ADT

  ````c++
  class UF
  {
      private:
      	//Implementation-dependent code
      public:
      	UF(int);
      	int find(int,int);
      	void unite(int ,int);
  };
  ````

  ***Equivalence-relations ADT interface***

- There are 3 abstract operations

  - initialize an abstract data structure to track connections
  - find whether two nodes are connected or not
  - unite two given node

- ***Equivalence-relations ADT client***

  ````c++
  #include <iostream.h>
  #include <stdlib.h>
  #include "UF.cxx"
  int main (int argc,char *argv[])
  {
      int p,q, N=atoi(argv[1]);
      UF info(N);
      while(cin>>p>>q){
          if (!info.find(p))
          {
              info.unite(p,q);
              cout<<" "<< p << " "<<q<<endl;
          }
      }
  }
  ````

- **Equivalence-relations ADT implementation**

  ````c++
  class UF
  {
      private:
          int *id, *sz;
          int find(int x)
          { while (x != id[x]) x = id[x]; return x; }
      public:
          UF(int N)
          {
              id = new int[N]; sz = new int[N];
              for (int i = 0; i < N; i++)
              { id[i] = i; sz[i] = 1; }
          }
          int find(int p, int q)
              { return (find(p) == find(q)); }
          void unite(int p, int q)
              { int i = find(p), j = find(q);
              if (i == j) return;
              if (sz[i] < sz[j])
                  { id[i] = j; sz[j] += sz[i]; }
              else { id[j] = i; sz[i] += sz[j]; }
          }
  };
  ````

  we can see above interface and implementations are mixed in above example we can separate them using 3 file strategy or below given method

- #### Abstract Class

  - a class whose members are all pure virtual functions.

  - and classes deriving the abstract  class must define all the member functions and any necessary private data members

  - we can say abstract class is an interface and any class that is derived from it is an implementations.

  - ***Abstract Class for equivalence-relations  ADT***

    ```c++
    class uf
    {
        public:
        	virtual uf(int) = 0;
        	virtual int find(int , int) = 0;
        	virtual void unite(int , int) = 0;
    }
    ```

  - so we can change above implementation by

    `class UF : public class uf`

  - this indicates UF is derived from uf

- using abstract classes incurs significant runtime cost

  - every call to a virtual function requires following the pointer thru a pointer table
  - compilers are far more restricted in their ability to produce optimized code for abstract classes

-