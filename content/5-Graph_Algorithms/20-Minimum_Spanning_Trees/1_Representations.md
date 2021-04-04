+++

title = "1-Representations"

+++

### Minimum Spanning Tree

Graph models where we associate weights or costs with each edge are called for in many applications.

Questions that entail cost minimization naturally arise for such situations. We examine algorithms for such problems : (i) find the lowest-cost way to connect all of the points, and (ii) find the lowest-cost path between two given points.

First type of algorithm which is useful for undirected graphs that represent objects such as circuits, finds a *minimum spanning tree*

The second type of algorithm which is useful for digraphs that represent objects such as airline routes map, finds the *shortest paths*

Sometimes we represent edge sizes to represent the weight but when edges really represent distance we implement geometric properties in algorithms to gain efficiency.

**Definition 20.1** *A **minimum spanning tree (MST)** of a weighted graph is a spanning tree whose weight (the sum of weights of its edges) is no larger than the weight of any other spanning tree.*

The possibility of equal weights also complicated the descriptions and correctness proof of some of our algorithms.

Not only might there be more than one MST, but nomenclature does not capture precisely the concept that we are minimizing the weight rather than tree itself. So proper word we should use is minimal spanning tree. But to avoid confusion in min flow and max flow we consider using it.

We work exclusively on undirected graphs in this chapter.

### Representation

Now we make changes to previous graph ADT to store information about weight of edges instead of boolean values. And this data about weight might be of unknown complexity. So better approach is for leaving the client to define the Edge data type and for implementations to manipulate *pointers* to edges.



**Program 20.1 ADT interface for graph with weight edges**

````c++
class Edge
{
    public :
    EDGE(int,int,double);
    int v() const;
    int w() const;
    double wt() const;
    bool from(int) const;
    int other(int) const;
};
template<class Edge> class GRAPH
{
    public:
        GRAPH(int,bool);
        ~GRAPH();
        int V() const;
        int E() const;
        bool directed() const;
        int insert(Edge *);
        int remove(Edge *);
        Edge *edge(int,int);
    class adjIterator
    {
        public:
        adjIterator(const GRAPH &,int);
        Edge *beg();
        Edge *nxt();
        bool end();
    };
};
````

**Program 20.2 Examples of a graph-processing client function**

````c++
template <class Graph, class Edge>
vector<Edge *> edges(const Graph &G){
    int E = 0;
    vector<Edge*> a(G.E());
    for(int v = 0; v< G.V(); v++)
    {
        typename Graph::adjIterator A(G,v);
        for(Edge *e = A.beg(); !A.end(); e = A.nxt())
            if(e->from(v)) a[E++] = e;
    }
    return a;
}
````

For testing algorithms and basic application we use a class EDGE, which has two ints and a double as private data member taht are initialized from constructor arguments and are return values of v(),w(), and wt() member function respectively.

**Program 20.3 Weighted-graph class (adjacency matrix)**

````c++
template <class Edge> class DenseGRAPH{
    int Vcnt,Ecnt; bool digraph;
    vector<vector<Edge *> adj;
    public:
    	DenseGRAPH(int V, bool digraph = false):
    		adj(V), Vcnt(V),Ecnt(0), digraph(digraph){
                for(int i = 0; i < V; i++)
                    adj[i].assign(V,0);
            }
    	int V() const {return Vcnt;}
    	int E() const {return Ecnt;}
    	bool directed() const {return digraph;}
    	void insert(Edge *e)
        {
            int v = e->v(), w = e->w();
            if(adj[v][w] == 0) Ecnt++;
            adj[v][w] = e;
            if(!digraph) adj[w][v] = e;
        }
    	void remove(Edge *e){
            int v = e->v(), w = e->w();
            if(adj[v][w] != 0) Ecnt--;
            if(!digraph) adj[w][v] = 0;
        }
    	Edge* edge(int v, int w) const
        { return adj[v][w]; }
    	class adjIterator;
    	friend class adjIterator;
};
````

**Program 20.4 Iterator class for adjacency-matrix representation**

````c++
template<class Edge>
class DenseGRAPH<Edge>::adjIterator
{
    const DenseGRAPH<Edge> &G;
    int i,v;
    public:
    	adjIterator(const DenseGRAPH<Edge> &G, int v):
    	G(G),v(v), i(0){}
    	Edge *beg()
        { i = -1; return nxt();}
    	Edge *nxt()
        {
            for(i++;i<G.V(); i++)
                if(G.edge(v,i)) return G.adj[v][i];
            return 0;
        }
    	bool end() const
        {return i >= G.V();}
};
````

**Program 20.5 Weighted-graph class (adjacency lists)**

````c++
template <class Edge> class SparseMultiGRAPH{
    int Vcnt,Ecnt; bool digraph;
    struct node{
        Edge *e; node* next;
        node(Edge* e, node* next) : e(e),next(next) {}
    };
    typedef node* link;
    vector<link> adj;
    public:
    	SparseMultiGRAPH(int V, bool digraph = false):
    		adj(V), Vcnt(V),Ecnt(0), digraph(digraph){}
    	int V() const {return Vcnt;}
    	int E() const {return Ecnt;}
    	bool directed() const {return digraph;}
    	void insert(Edge *e)
        {
			adj[e->v()] = new node(e,adj[e->v()]);
            if(!digraph)
                adj[e->w()] = new node(e, adj[e->w()]);
            Ecnt++;
        }
    	class adjIterator;
    	friend class adjIterator;
};
````

As with our undirected-graph implementation, we don't explicitly test for parallel edges in  either implementation.

How should we represent the MST itself ? MST of a graph G is a subgraph of G that is also a tree, so we have many options

We can easily convert MST implementation into each other; :)

