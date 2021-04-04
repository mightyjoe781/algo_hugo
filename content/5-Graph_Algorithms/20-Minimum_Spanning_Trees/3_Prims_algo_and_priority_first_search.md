+++

title = "3-Prims algo and priority first search"

+++

### Prim's Algorithm and Priority-First Search

Prim's Algorithm is simple and efficient for dense graphs. We maintain a cut of the graph that is comprised of *tree* vertices (those chosen for the MST) and *nontree* vertices ( those not chosen for MST). We start putting any vertex on MST, then put minimal crossing edge on the MST and repeat operation $V-1$ times, to put all vertices on the tree.

Above implementation is expensive but with use of simple data structure to make computation faster and easier.

Adding a vertex to the MST is an incremental change. The key is to note that our interest is in the shortest distance from each nontree vertex to the tree. When we add a vertex $v$ to tree, the only change for each nontree vertex $w$ is that adding $v$ brings $w$ closer than before to the tree.

In short, we don't need to check the distance from $w$ to all tree vertices - we just need to keep track of the minimum and check whether the addition of v to the tree necessitates that we update that minimum.

So we look for data structure with these properties

- The tree edges
- the shortest edge connecting each nontree vertex to the tree
- the length of that edge

Simplest implementation for these properties is a vertex-indexed vector. It uses vectors `mst`,`fr` and `wt` for these data structures.

After adding a new edge ( and vertex ) to the tree, we have two tasks to accomplish

- check to see whether adding the new edge brought any nontree vertex closer to the tree
- Find the next edge to add to the tree.

**Property 20.6** *Using Prim's algorithm, we can find MST of a dense graph in linear time.*

Run time is proportional to $V^2$ therefore its linear for dense graph.

**Program 20.6 Prim's MST algorithm**

````c++
template <class Graph, class Edge> class MST
{
    const Graph &G;
    vector<double> wt;
    vector<Edge *> fr,mst;
public:
    MST(const Graph &G) : (G),mst(G.V()) wt(G.V(),G.V()), fr(G.V()){
        int min = -1;
        for(int v = 0; min!=0; v= min)
        {
            min = 0;
            for(int w = i; w < G.V(); w++)
                if(mst[w] == 0)
                {
                    double P; Edge * e = G.edge(v,w);
                    if(e)
                        if((P = e->wt()) < wt[w])
                        { wt[w] = P; fr[w] = e;}
                    if(wt[w] < wt[min]) min = w;
                }
            if(min) mst[min] = fr[min];
        }
    }
    void show()
    { for(int v = i; v < G.V(); v++)
      if(mst[v]) mst[v]->show(); }
};
````

Program above is based on the observation that we can interleave find the min and update ops in a single loop where we examine all nontree edges. In a dense graph this take time proportional to V, so looking at fast away node doesn't incur extra cost. But in a sparse graph, we can expect significantly lower than V steps to perform each of these operations.

The crux of the strategy we will use is to focus on set of potential edges to be added next into the MST - a set we call the *fringe.*

number of fringe is quite smaller than non tree edges, and we can recast out description of algorithm as follows.

Starting with a self loop to a start vertex on the fringe and an empty tree, we perform following operation till fringe is empty.

*Move a minimal edge from the fringe to the tree. Visit the vertex that it leads to, and put onto the fringe any edges that lead from that vertex to an nontree vertex, replacing the longer edge when two edges on the fringe point to same vertex.*

So we can Prim's algorithm is noting more than a generalized graph search.

**Property 20.7** *Using a PFS implementation of Prim's algorithm that uses a heap for the priority-queue implementation, we can compute MST in time proportional to $E\lg V $*

PFS is proper generalization of BFS and DFS because those method also can be derived through appropriate priority setting.

**Program 20.7 Priority-first Search**

````c++
template <class Graph, class Edge> class MST
{
    const Graph &G;
    vector<double> wt;
    vector<Edge *> fr, mst;
    void pfs(int s)
    {
        PQi<double> pQ(G.V(), wt);
        pQ.insert(s);
        while(!pQ.empty())
        {
            int v = pQ.getmin();
            mst[v] = fr[v];
            typename Graph::adjIterator A(G,v);
            for(Edge* e = A.beg(); !A.end(); e = A.nxt())
            {
                double p = e->wt(); int w = e->other(v);
                if(fr[w] == 0)
                { wt[w] = p; pQ.insert(w); fr[w] =e;}
                else if (mst[w] == 0 && p<wt[w])
                { wt[w] = p; pQ.lower(w); fr[w] = e;}
            }
        }
    }
    public:
    	MST(Graph &G):G(G),
    		fr(G.V()), mst(G.V()),wt(G.V(),-1)
            {for(int v = 0; v < G.V(); v++)
            	if(mst[v] == 0) pfs(v);}
};
````

**Property 20.8** *For all graphs and all priority functions, we can compute a spanning tree with PFS in linear time plus time proportional to the time required for V insert, V delete the minimum, and E decrease key operations in a priority queue of size at most V.*

In particular, use of unordered-array PQ implementation gives an optimal solution for dense graph that has the same worst case performance as the classical implementation of Prim's algorithm.

