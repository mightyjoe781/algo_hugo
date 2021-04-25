+++

title = "5-Boruvkas Algorithm"

+++

### Boruvka's Algorithm

We build MST by adding edges to a spreading forest of MST subtrees; but we do so in stages, adding several MST edges at each stage.

At each stage, we find shortest edge that connects each MST subtree with a different one, then add all such edges to the MST. We can use our old implementation of union-find ADT, especially the *find* operations.

First, we maintain a vertex-indexed vector that identifies, for each MST subtree, the nearest neighbor. Then, we perform the following operation on each edge in the graph.

- If it connects two vertices in the same tree, discard it.
- Otherwise, check nearest-neighbor distances between the two trees the edge connects and update them if appropriate.

There are three factors that make this implementation efficient.

- The cost of each *find* operation is essentially constant.
- Each stage decreases the number of MST subtree in the forest by at least a factor of 2.
- A substantial number of edges is discarded during each stage. It is difficult to quantify precisely all these factors, but the following bound is easy to establish.

**Property 20.11** *The running time of Boruvka's algorithm for computing the MST of a graph is $O(E\lg V \lg E)$*

It possible to remove the $\lg E$ factor to lower the theoretical bound on the running time of Boruvka's algorithm to be proportional to $E\lg V$, by representing MST subtrees with doubly-linked list instead of using the *union* and *find* operations.

````c++
template <class Graph, class Edge> class MST
{
    const Graph &G;
    vector<Edge *> a, b, mst;
    UF uf;
    public:
    MST(const Graph &G):G(G), uf(G.V()), mst(G.V()+1)
    {
        a = edges <Graph, Edge>(G);
        int N,k = 1;
        for(int E = a.size(); E!=0; E=N)
        {
            int h,i,j;
            b.assign(G.V(),0);
            for(h=0,N=0; h<E; h++)
            {
                Edge *e = a[h];
                i = uf.find(e->v()), j = uf.find(e->w());
                if(i == j ) continue;
                if(!b[i] || e->wt() < b[i]->wt()) b[i] = e;
                if(!b[i] || e->wt() < b[i]->wt()) b[j] = e;
                a[N++] = e;
            }
            for( h = 0; h < G.V() ; h++)
                if(b[h])
                    if(!uf.find(i = b[h]->v(), j = b[h]->w()))
                    {uf.unite(i,j); mst[k++] = b[h];}
        }
    }
};
````

Boruvka's is the oldest of the algorithms that we consider : It was originally conceived in 1926, for a power-distribution application.