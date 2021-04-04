+++

title = "6-Priority Queues for Index Items"

+++

### Priority Queues for Index Items

Suppose that records to be processed in a PQ are in array already. So we can develop PQ routines handle data using indexes. Moreover handle here becomes array index.



***Priority queue ADT interface for index items***

````c++
template <class Index>
class PQ
	{
    private :
    	//Implemetation-dependent code
    public :
    	PQ(int);
    	int empty() const;
    	void insert(Index);
    	Index getmax();
    	void change(Index);
    	void remove(Index);
	};
````

***Index heap based priority queues***

````c++
template <class Index>
class PQ
{
    private:
    	int N; Index* pq; int* qp;
    	void exch(Index i, Index j)
        {
            int t;
            t=qp[i];qp[i] = qp[j];qp[j]= t;
            pq[qp[i]] = i; pq[pq[j]] = t;
        }
    	void fixUp(Index a[],int k);
    	void fixDown(Index a[],int k , int N);
    public:
    	PQ(int maxN)
        	{ pq = new Index[maxN+1];
              qp = new int[maxN+1]; N = 0;
            }
    	int empty() cont
        { return N == 0;}
    	void insert(Index v)
        { pq[++N] = v ; qp[v] = N; fixUp(pq,N);}
    	Index getmax()
        {
            exch(pq[1],pq[N]);
            fixDown(pq,1,N-1);
            return pq[N--];
        }
    	void change(Index k)
        	{ fixUp(pq,qp[k]); fixDown(pq,qp[k],N); }
};
````

