+++

title = "3-All pair Shortest Paths"

+++

### All Pair Shortest Paths

In this section we consider two classes that solve all-pair shortest-paths problem. The algorithm that we implement directly generalize two basic algorithms that we considered for the transitive closure problem.

First one in to run Dijkstra's algorithm from each vertex to get shortest paths from that vertex to each of the others. Second method which allows us to solve problem directly in time proportional to $V^3$, is extension of Warshall's algorithm that is *Floyd's algorithm*

Both of these implement ADT interface for finding shortest distances and path.

Here ADT preprocesses the data to provide constant time query at the cost of increased memory size.

**Program 21.2 All pair shortest-paths ADT**

````c++
template <class Graph, class Edge> class SPall{
    public:
    	SPall(const Graph &);
    	Edge *path(int,int) const;
    	Edge *pathR(int,int) const;
    	double dist(int,int) const;
};
````

The first all-pairs shortest paths ADT function implementation that we consider solves the problem by using Dijkstra's algorithm to solve the single-source problem for each vertex. In C++, we can express method directly.

**Program 21.3 Computing the diameter of a network**

````c++
template <class Graph, class Edge>
double diameter(Graph &G)
{ 	int vmax = 0, wmax = 0;
    allSP<Graph, Edge> all(G);
	for (int v = 0; v < G.V(); v++)
	for (int w = 0; w < G.V(); w++)
		if (all.path(v, w))
			if (all.dist(v, w) > all.dist(vmax, wmax))
			{ vmax = v; wmax = w; }
	int v = vmax; cout << v;
	while (v != wmax)
		{ v = all.path(v, wmax)->w(); cout << ”-” << v; }
	return all.dist(vmax, wmax);
}
````

**Property 21.7** *With Dijkstra's algorithm, we can find all shortest path in a network that has nonnegative weights in time proportional to $VE \log_d V$  where d = 2 if E <2V, and d = E/V otherwise.*

**Program 21.4 Dijkstra's algorithm for all shortest paths**

````c++
#include “SPT.cc”
template <class Graph, class Edge> class allSP
{ 	const Graph &G;
	vector< SPT<Graph, Edge> *> A;
public:
	allSP(const Graph &G) : G(G), A(G.V())
		{ for (int s = 0; s < G.V(); s++)
		A[s] = new SPT<Graph, Edge>(G, s); }
	Edge *pathR(int s, int t) const
		{ return A[s]->pathR(t); }
	double dist(int s, int t) const
		{ return A[s]->dist(t); }
};
````

For dense graphs, we could use an adjacency matrix representation and avoid computing the reverse graph by implicitly transposing the matrix. Developing an implementation along these lines is interesting exercise however next method is more compact.

The method of choice for dense graph is Floyd's Algorithm. Instead of using logical or operation to keep track of existence of paths, it check distances for each edge to determine whether that edge is part of a new shorter path. Indeed, as we have noted, Floyd's and Warshall's algorithm are identical in proper abstract setting.

**Program 21.5 Floyd's algorithm for all shortest paths**

````c++
template <class Graph, class Edge> class allSP
{
    const Graph &G;
    vector<vector <Edge *>> p;
    vector<vector <double>> d;
public:
    allSP(const Graph &G) : G(G), p(G.V()), d(G.V())
    {
        int V = G.V();
        for(int i = 0; i < V; i++)
        { p[i].assign(V,0); d[i].assign(V,V);}
        for(int s = 0; s<V; s++)
            for(int t = 0; t <V; t++)
                if(G.edge(s,t))
                {
                    p[s][t] = G.edge(s,t);
                    d[s][t] = G.edge(s,t)->wt();
                }
        for(int s =0; s<V; s++) d[s][s] =0;
        for(int i = 0; i<V; i++)
            for(int s = 0; s<V; s++)
                if(p[s][i])
                    for(int t = 0; t <V; t++)
                        if(s!=t)
                            if(d[s][t] > d[s][i]+d[i]d[t])
                            {
                                p[s][t] = p[s][i];
                                d[s][t] = d[s][i] + d[i][t];
                            }
    }
    Edge *path(int s, int t) const {return p[s][t]; }
    double dist(int s,int t) const { return d[s][t]}
};
````

**Property 21.8** With Floyd's algorithm, we can find all shortest paths in a network in time proportional to $V^3$.

Running Dijkstra's algorithm on each vertex is clearly a method of choice for sparse networks, because the running time is close to VE. As density increases, Floyd's algorithm- which always takes time proportional to $V^3$ becomes competitive and it is widely used because its so simple to implement.

Floyd's algorithm is effective in even those networks with negative weights given no negative cycles.

