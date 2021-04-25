+++

title = "4-shortest path in acyclic networks"

+++

#### Shortest Paths in Acyclic Networks

For shortest-paths problems, we do have algorithms for DAGs that are simpler and faster than the priority-queue-based methods that we have considered for general digraphs. Specifically, in this section we consider algorithms for acyclic networks that.

- Solve the single source problem in linear time
- solve the all-pairs problem in time proportional to VE
- solve other problems, such as finding longest paths

In the first two cases, we cut the logarithmic factor from running time that is present in our best algorithm for sparse networks; in the third case, we have simple algorithm for problems that are intractable for general networks.

Since there are no cycle at all, there are no negative cycles; so negative weights present no difficulty in shortest-paths problems on DAGs. Accordingly, we place no restriction on edge-weight values throughout this section.

The four basic ideas that we applied to derive efficient algorithms for unweighted DAGs in chap 19 are even more effective for weighted DAGs.

- Use DFS to solve single-source problem
- use a source queue to solve the single-source problem
- invoke either method, once for each vertex, to solve the all-pairs problem
- use a single DFS( with DP ) to solve the all-pair problem

The method solve the single-source problem in  time proportional to E and the all-pair problems in time proportional to VE. They are all effective because of topological ordering which allows us to compute the shortest paths for each vertex without having to revisit any decision.

**Multisource shortest paths** Given a set of start of vertices, find, for each other vertex w, a shortest path among the shortest paths from each start vertex to w.

This problem is essentially equivalent to the single-source shortest-paths problem. We can convert a multisource problem into a single-source problem by adding a dummy source vertex with zero-length edge to each source in the network.

Conversely, we can convert a single-source problem to a multisource problem by working with the induced subnetwork defined by all vertices and edges reachable from the source.

Topological sorting immediately presents a solution to multi-source shortest-path problem and to numerous other problems. we maintain a vertex-indexed vector `wt `that gives the weight of the shortest know path from any source to each vertex. To solve the multisource shortest path problem, we initialize the `wt` vector to 0 for sources and a large sentinel value for all the other vertices. Then, we process the vertices in topological order. To process the vertex v, we perform a relaxation operation for each outgoing edge v-w that update the shortest path to w if v-w gives a shorter path form source to w (through v).

**Property 21.9** We can solve the multisource shortest-paths problem and the
multisource longest-paths problem in acyclic networks in linear time.

**Program 21.6 Longest path in an acyclic network**

````c++
#include "dagTS.cc"
template <class Graph, class Edge> class LPTdag{
    const Graph &G;
    vector <double> wt;
    vector<Edge *> lpt;
public:
    LPTdag(const Graph &G):G(G),lpt(G.V()),wt(G.V(),0){
        int j, w;
        dagTS<Graph> ts(G);
        for(int v = ts[j=0]; j < G.V(); v = ts[++j])
        {
            typename Graph::adjIterator A(G,v);
            for(Edge*e = A.beg(); !A.end(); e = A.nxt())
            { wt[w] = wt[v]+e->wt(); lpt[w] = e;}
        }
    }
    Edge *pathR(int v) const { return lpt[v]; }
    double dist(int v) const { return wt[w]; }
};
````

**Program 21.7 All shortest paths in an acyclic network**

````c++
template <class Graph , class Edge> class allSPdag {
    const Graph &G;
    vector<vector<Edge *>> p;
    vector<vector<double> > d;
    void dfsR(int s)
    {
        typename Graph::adjIterator A(G,s);
        for(Edge* e = A.beg(); !A.end(); e = A.nxt())
        {
            int t = e->w(); double w = e->wt();
            if(d[s][t]> w)
            { d[s][t] = w; p[s][t] = e;}
            if(p[t][t] == 0) dfsR(t);
            for(int i = 0; i < G.V(); i++)
                if(p[t][i])
                    if(d[s][i] > w+d[t][i])
                    { d[s][i] = w + d[t][i]; p[s][i] = e;}
        }
    }
public:
    allSPdag(const Graph &G) : G(G),p(G.V()), d(G.V())
    {
        int V = G.V();
        for(int i = 0; i < V; i++)
        {p[i].assign(V,0); d[i].assign(V,V);}
        for(int s = 0; s < V; s++)
        if(p[s][s] == 0) dfsR(s);
    }
    Edge *path(int s, int t) const { return p[s][t]; }
   	double dist(int s, int t) cosnt { return d[s][t]; }
};
````

Thus for acyclic networks, topological sort allows us to avoid the cost of priority queue in Dijkstra's algorithm. Like floyd's algorithm, also solves problems more general than those solved by Dijkstra algorithm, because unlike Dijkstra the algorithm works correctly even in the presence of negative edge weights.