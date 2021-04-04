+++

title = "4-Kruskal's Algorithm"

+++

### Kruskal's Algorithm

Kruskal's algorithm also builds MST one edge at a time but it find an edge that connects two tree in a spreading forest of growing MST subtree.

disconnected forest of MST subtrees evolves gradually into a tree. Edges are added to the MST in order of their length, so the forests comprise vertices that are connected to one another by relatively short edges. At any point during execution of algorithm, each vertex is closer to some vertex in its subtree than to any vertex not in its subtree.

Note that there are 2 ways Kruskal's algo stops

If we find V-1 edges, then we have the spanning tree and stop. If we examine all the edges that is graph without finding $v-1$ tree edges, then we have determined that the graph is not connected.

**Property 20.9 ** *Kruskal's algorithm computes MST of a graph in time proportional $E\lg E$*

**Program 20.8 Kruskal's algorithm**

````c++
template <class Graph, class Edge, class EdgePtr>
class MST {
    const Graph &G;
    vector<EdgePtr> a,mst;
    UF uf;
    public :
    	MST(Graph &G):G(G), uf(G.V()), mst(G.V())
        {
            int V = G.V(), E = G.E();
            a = edge<Graph,Edge,EdgePtr> (G);
            sort<EdgePtr>(a,0,E-1);
            for(int i = 0; k = 1; i < E && k <V; i++)
                if(!uf.find(a[i]->v, a[i]->w))
                { uf.unite(a[i]->v, a[i]->w);
                 mst[k++] = a[i];

        }
};
````

Typically cost of finding MST with Kruskal's algorithm is even lower than the cost of processing all edges because MST is complete well before a substantial fraction of long graph edges is ever considered. We can use this fact and reduce run time in many applications.

**Property 20.10** *A priority-queue-based version of Kruskal's algorithm computes the MST of a graph in time proportional to $E+X\lg V $*, where $X$ is the number of graph edges not longer than the longest edge in MST.

