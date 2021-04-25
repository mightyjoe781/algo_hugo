+++

title = "6-Separability and Biconnectivity"

+++

### Separability and Biconnectivity

Given two vertices, are there two different paths connecting them.

If it is important that graph *be* connected in some situation, then we might want it to *stay* connected even if an edge or a vertex is removed.

**Definition** A **bridge** in a graph is an edge that, if removed, would separate a connected graph into two disjoint subgraphs. A graph that has no bridges is said to be **edge-connected**.

We refer to a graph that is not edge-connected as *edge-separable* graph, and we call bridges *separation edges*.

**Property 18.5** *In any DFS tree, a tree edge $v-w$* is a bridge if and only if there are no back edges that connect a descendant of $w$ to an ancestor of $w$.

**Property 18.6 ** *We can find graph's bridges in linear time*.
**Program 18.7 Edge connectivity**

`low` vector keeps track of lowest preorder number that can be reached from each vertex by a sequence of tree edges followed by one back edge.

````c++
template <class Graph>
class EC : public SEARCH<Graph>
{
    int best;
    vector <int> low;
    void searchC(Edge e)
    {
        int w = e.w;
        ord[w] = cnt++; low[w] = ord[w];
        typename Graph::adjIterator &(G,w);
        for(int t = A.beg(); !A.end(); t = A.nxt())
            if(ord[t] == -1)
            {
                searchC(Edge(w,t));
                if(low[w] > low[t]) low[w] = low[t];
                if(low[t] == ord[t])
                    best++;
            }
        	else if (t!=e.w)
            	if(low[w] > ord[t]) low[w] = ord[t];
    }
    public:
    EC(const Graph &G) : SEARCH<Graph> (G), bcnt(0), low(G.V, -1){
        search();
    }
    int count() const {return best+1;}
};
````

**Definition 18.2** *An **articulation point** in a graph is vertex that, if removed, would separate a connected graph into at least two disjoint subgraphs*.

**Definition 18.3** *A graph is said to be **biconnected** if every pair of vertices connected by two disjoint paths.*

This requirement that the paths be disjoint is what distinguishes biconnectivity by two disjoint paths.

**Property 18.7** *A graph is biconnected if and only if it has no separation vertices ( articulation points )*

![image-20210113083919689](/6_Separability_and_Biconnectivity.assets/image-20210113083919689.png)

**Property 18.8** *We can find a graph's articulation points and biconnected components in linear time.*

**Definition 18.4** *A graph is **k-connected** if there are at least $k$* vertex-disjoint paths connecting every pair of vertices in the graph. The **vertex connectivity** of a graph is minimum number of vertices that need to be removed to separate it into two pieces.

**Definition 18.5** *A graph is **k-edge connected** if there are at least $k$* edge-disjoint paths connecting every pair of vertices in the graph that need to be removed to separate it into two pieces.

