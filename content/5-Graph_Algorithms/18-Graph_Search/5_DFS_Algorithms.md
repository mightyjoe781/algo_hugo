+++

title = "5-DFS Algorithms"

+++

### DFS Algorithms

**Cycle Detection** Does a given graph have an cycles ? Is the graph a forest ?

This problem is easy to solve using DFS because any back edge in a DFS tree belongs to cycle consisting of edge plus the tree path connecting nodes.

For testing this condition we add else clause to program 18.1's if condition to test whether t is equal to v.

If it is, we have just encountered the parent link $w-v$. If not, w-t completes a cycle with edges from $t$ down to $w$ in the DFS tree.

Any graph with V or more edges must have cycle so it takes V visits at most to find out the cycle.

**Simple Path** Given two vertices, is there a path in the graph that connects them ?

**Simple Connectivity** We can check connectivity in linear time. Graph-search strategy is dependent on upon calling a search function for each connected component. In a DFS, the graph is connected if and only if the graph-search function calls the recursive DFS function just once. The number of connected components in the graph is precisely the number of times that recursive function is called from GRAPHSearch, so we can find number of connective components by keeping track of such calls.

**Program 18.4 Graph Connectivity**

CC constructor computes, in linear time, the number of connected components in a graph and stores a component index associated index associated with each vertex in the private vertex-indexed vector id. Clients can use a CC object to find the number of connected components (count ) or test whether any pair of vertices are connected (connect), in constant time.

````c++
template <class Graph> class CC
{
    const Graph &G;
    int ccnt;
    vector <int> id;
    void ccR(int w)
    {
        id[w] = ccnt;
        typename Graph::adjIterator A(G,w);
        for(int v = A.beg(); !A.end(); v = A.nxt())
            if(id[v] == -1) ccR(v);
    }
    public:
    	CC(const Graph &G)  : G(G), ccnt(0), id(G.V(),-1)
        {
            for(int v = 0; v <G.V() ; v++)
                if(id[v] == -1) { ccR(v); ccnt++;}
        }
    	int count() const {return ccnt;}
    	bool connect(int s, int t) const
        { return id[s] == id[t] ; }
};
````

**Two-way Euler tour ** is a DFS-based class for finding a path that uses all the edges in a graph exactly twice-once in each direction.

The path corresponds to Tremaux exploration.

**program 18.5 Two-way Euler tour**

````c++
template <class Graph>
class EULER : public SEARCH<Graph>
{
    void searchC(Edge e){
        int v = e.v, w = e.w;
        ord[w] = cnt++;
        cout << "-" << w;
        typename Graph::adjIterator A(G,w);
        for(int t = A.beg(); !A.end(); A.nxt())
            if(ord[t] == -1) searchC(Edge(w,t));
        	else if(ord[t] < ord[v])
                cout<<"-" << t << "-" << w;
        if(v!=w) cout<<"-"<<v; else cout << endl;
    }
    public:
    	EULER(const Graph &G) : SEARCH<Graph>(G)
        { search();}
};
````

**Spanning Forest**

Given a connected graph with V vertices, find a set of $V-1$ edges that connects the vertices. If graph has $C$ connected components, find a spanning forest (with $V-C$ edges). We have already seen solution in 18.3 program :)

**Vertex Search**

How many vertices are in the same connected component as a given vertex ? We can solve this problem easily by starting a DFS at the vertex and counting the number of vertices marked. In a dense graph, we can speed up the process considerably by stopping the DFS after we have marked $V$ vertices at that point, we know that no edge will take us to vertex we have not yet seen, so we will be ignoring all the rest of edges.

**Two-colorability, bipartiteness, odd cycle**

Is there a way to assign one of two colors to each vertex of a graph such that no edge connects two vertices of the same color ? Is graph bipartite? Does graph have a cycle of odd length.

**Program 18.6 ** Two colorability (bipartiteness)

````c++
template <class Graph> class BI
{
    const Graph &G;
    bool OK;
    vector <int> vc;
    bool dfsR(int v, int c)
    {
        vc[v] = (c+1)%2;
        typename Graph::adjIterator A(G,v);
        for(int t=A.beg(); !A.end(); t = A.nxt())
            if(vc[t] == -1)
            { if(!dfsR(t,vc[v])) return false;}
        	else if(vc[t] != c) return false;
        return true;
    }
    public:
    	BI(const Graph &G) : G(G),OK(true), vc(G.V(),-1)
        {
            for(int v = 0 ; v<G.V(); v++)
                if(vc[v] == -1)
                    if(!dfsR(v,0)) { OK = false ; return;}
        }
    		bool bipartite() const {return OK; }
    		int color(int v) const { return vc[v]; }
};
````

