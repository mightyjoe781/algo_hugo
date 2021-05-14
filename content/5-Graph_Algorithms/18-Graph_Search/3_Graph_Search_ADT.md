+++

title = "3-Graph Search ADT"

+++

### Graph-Search ADT Functions

**18.2 Graph Search**

````c++
template <class Graph > class SEARCH
{
    protected:
    	const Graph &G;
    	int cnt;
    	vector<int> ord;
    	virtual void search (Edge) = 0;
    	void search(){
            for(int v = 0; v<G.V(); v++)
                if(ord[v] == -1) searchC(Edge(v,v));
        }
    public:
    	SEARCH(const Graph &G): G(G),
    	ord(G.V(),-1), cnt(0) {}
    	int operator[](int v) const {return ord[v];}
};
````

We typically use graph-search function that performs these steps until all of the vertices of the graph have been marked as having been visited:

- Find an unmarked vertex (a start vertex)
- Visit (and mark as visited) all the vertices in the connected component that contains the start vertex.

**Program 18.3 Derived class for depth-first search for computing a spanning forest from SEARCH base class of 18.2 **

private vector `st` here holds a parent-link representation of tree that we initialize in the constructor. So basically this allows anyone know the parent of any vertex easily.

````c++
template <class Graph>
class DFS : public SEARCH<Graph>
{
    vector<int> st;
    void searchC(Edge e)
    {
        int w = e.w;
        ord[w] = cnt++; st[e.w] = e.v;
        typename Graph::adjIterator A(G,w);
        for(int t = A.beg(); !A.end(); t = A.nxt())
            if(ord[t] == -1) searchC(Edge(w,t));
    }
    public :
    	DFS(const Graph &G) : SEARCH<Graph> (G),
    		st(G.V(), -1) { search(); }
    	int ST(int v) const {return st[v];}
};
````

**Property 18.2** *A graph-search function checks each edge and marks each vertex in a graph if an only if the search function that it uses marks each vertex and checks each edge in the connected component that contains the start vertex.*

*Proof  :* By induction on the number of connected components.

**Property 18.3** *DFS of a graph represented with an adjacency matrix requires time proportional to $V^2$*.

**Property 18.4** *DFS of a graph represented with adjacency lists requires time proportional to $V+E$*

18.3 and 18.4 implies that search is linear in size of data structure used to represent the graph.

If time per vertex is bounded by some function $f(V,E)$, then the time for the search is guaranteed to be proportional to $E+Vf(V,E)$.

