+++

title = "4-Adjacency Lists Representation"

+++

#### Adjacency Lists Representation

Preferred when graphs are not dense, where we keep track of all vertices connected to each vertex on a linked list that is associated with that vertex. We maintain a vector of lists so that, given a vertex, we can immediately access its list; we use linked lists so that we can add new edges in constant time.

**Graph ADT implementation (adjacency lists)**

````c++
class SparseMultiGRAPH
{
    int Vcnt, Ecnt; bool digraph;
    struct node
    {
        int v; node* next;
        node(int x, node* t){ v = x; next = t; }
    };
    typedef node* link;
    vector<link> adj;
    public:
     SparseMultiGRAPH(int V, bool digraph = false):
         adj(V), Vcnt(V), Ecnt(0), digraph(digraph)
     	{ adj.assign(V,0);}
     int V() const { return Vcnt; }
     int E() const { return Ecnt; }
     bool directed() const { return digraph; }
     void insert(Edge e)
     {
         int v = e.v , w = e.w;
         adj[v] = new node(w, adj[v]);
         if(!digraph) adj[w] = new node(v,adj[w]);
     }
    void remove(Edge e);
    bool edge(int v,int w) const;
    class adjIterator;
    friend class adjIterator;
};
````

**Iterator for adjacency-lists representation**

````c++
class SparseMultiGRAPH::adjIterator
{
    const SparseMultiGRAPH &G;
    int v;
    link t;
    public:
    	adjIterator(const SparseMultiGRAPH &G, int v):
    		G(G), v(v) { t= 0; }
    	int begin()
        { t= G.adj[v]; return t? t->v : -1;}
    	int nxt()
        {if(t) t = t->next; return t ? t->v : -1;}
    	bool end()
        { return t == 0; }
};
````

It builds multigraph. Checking duplicate edges in the adjacency-lists structure would necessitate searching through the lists and could take time proportional to $V$ same can be said about *remove edge function.*

This graph is undesirable if there is heavy use of *remove edge* or of existence tests.

Isomorphism problem still persists. Adjacency-list structure determines how our various algorithms see the graph. Although a algorithm should produce correct answers no matter how the edges are ordered on adjacency list.

Primary advantage is space-efficient structure while disadvantage is time proportional to $V$ for removing a specific edges.