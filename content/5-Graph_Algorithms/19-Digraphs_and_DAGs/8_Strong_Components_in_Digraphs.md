+++

title = "8-Strong Components in Digraphs"
weight = 8

+++

### Strong Components in Digraphs

Undirected graphs and DAGs are both simpler structure than digraphs because of structural symmetry that characterizes the reachability relationships among the vertices;

To understand structure of digraphs, we consider *strong connectivity*.

If $s$ and $t$ are strongly connected (each reachable from the other) the by definition so are $t$ and $s$.

The goal of our algorithm is to assign component numbers to each vertex in a vertex indexed vector using labels 0, 1, $\rhd$ , for strong components. The highest number assigned is one less than number of strong components, and we can use component numbers to provide a constant-time test of whether two vertices are in the same strong component.

A brute force method is by checking each pair of $s$ and $t$ to see whether t is reachable for s and s is reachable from t. Define an undirected graph with edges with such pairs. The connected components of the graph is strong components of digraph.

***Kosaraju's Algorithm***

Kosaraju's method is simple to explain and implement. To find the strong components of a graph, first run DFS on its reverse, computing the permutation of vertices defined by postorder numbering. (This is like topological sort if digraph is a DAG.) Then, run DFS again on graph, but to find the next vertex to search, *use the unvisited vertex with highest postorder number.*

Magic of this method is that, when the unvisited vertices are checked according to topological sort in this way, the tree in the DFS forest defines the strong components just as tree in a DFS forest define connected components in undirected graph.

Two vertices are connected if and only if they are in same tree in this forest.

**Program 19.10 Strong components (Kosaraju's Algorithm)**

````c++
template <class Graph> class SC
{
    const Graph &G;
    int cnt, scnt;
    vector<int> postI,postR, id;
    void dfsR(const Graph&G, int w)
    {
        id[w] = scnt;
        typename Graph::adjIterator A(G,w);
        for(int t = A.beg(); t!= A.end(); t = A.nxt())
            if(id[t] == -1) dfsR(G,t);
        postI[cnt++] = w;
    }
    public :
    	SC(const Graph &G) : G(G), cnt(0), scnt(0),
    		postI(G.V()), postR(G.V()), id(G.V(),-1)
            {
                Graph R(G.V(), true);
                reverse(G,R);
                for(int v = 0; v<R.V(); v++)
                    if(id[v] == -1) dfsR(R,v);
                postR = postI; cnt = scnt = 0;
                id.assign(G.V(), -1);
                for(int v = G.V()-1; v>=0; v--)
                    if(id[postR[v] == -1)
                      { dfsR(G,postR[v]); scnt++;}
            }
			int count() const { return scnt; }
			const stronglyreachable(int v, int w) const
            {return id[v] == id[w]; }
};
````

**Property 19.14** *Kosaraju's method finds the strong components of a graph in linear time and space.*

We don't even need to reverse the graph when using the adjacency matrix representation. :)

Now lets move on to Tarjan's Algorithm and Gabow's algorithm - ingenious methods that requires less changes  to the basic DFS procedure.

It is same to the program for finding bridges we read :). The method is based on 2 observation.

1. consider vertices in reverse topological order so that when we reach the end of recursive function for a vertex we know we will not encounter any more vertices in the same strong component.
2. The back links in the tree provide a second path from one vertex to another and bind together the strong components.

The recursive DFS in program 18.7 uses the computation to find the highest vertex reachable (via a back edge) from any descendants of each vertex and use search-indexed vector to keep track of the strong components and a stack to keep track of current search path. Pushing the vertex names onto a stack on entry to recursive function, then pops them and assigns components number after visiting the final member of each strong component.

**Property 19.15** *Tarjan's algorithm finds the strong components of a digraph in linear time.*

**Program 19.13 Strong Components (Tarjan's Algorithm)**

````c++
#include <stack>
template <class Graph> class SC
{
    const Graph &G;
    stack<int> S;
    int cnt,scnt;
    vector<int> pre,low,id;
    void scR(int w)
    {
        int t;
        int min = low[w] = pre[w] = cnt++;
        S.push(w);
        typename Graph::adjIterator A(G,w);
        for(t = A.beg(); !A.end(); t = A.nxt())
        {
            if(pre[t] == -1) scR(t);
            if(low[t] < min) min = low[t];
        }
        if(min < low[w]) {low[w] = min; return;}
        do
        { id[t=S.pop()] = scnt; low[t] = G.V();}
        while(t!=v)
            scnt++;
    }
    public:
    	SC(const Graph &G) : G(G), cnt(0), scnt(0), pre(G.V(),-1), low(G.V()), id(G.V())
        { for(int v = 0; v < G.V(); v++)
        	if(pre[v] == -1) scR(v);}
    	int count() const {return scnt;}
    	bool stronglyreachable(int v, int w) const
        { return id[v] == id[w];}
};
````

 In 1999 Gabow discovered the version of Tarjan's algorithm in Program 19.12 The algorithm maintains the same stack of vertices in the same way as does Tarjan's algorithm but uses a second stack to decide when to pop all the vertices in each strong component from the main stack. The second stack contains vertices on the search path.

**Property 19.16** *Gabow's algorithm finds the strong components of a digraph in linear time*

**Program 19.12 Strong Components ( Gabow's algorithm )**

````c++
void scR(int w)
{
    int v;
    pre[w] = cnt++;
    S.push(w); path.push(w);
    typename Graph::adjIterator A(G,w);
    for(int t = A.beg(); !A.end(); t = A.nxt())
        if(pre[t] == -1) scR(t);
    	else if (id[t] == -1)
            while(pre[path.top()] > pre[t]) path.pop();
    if(path.top() == w) path.pop(); else return;
    do {id[v=S.pop()] = scnt; } while(v!=w);
    scnt++;
}
````

