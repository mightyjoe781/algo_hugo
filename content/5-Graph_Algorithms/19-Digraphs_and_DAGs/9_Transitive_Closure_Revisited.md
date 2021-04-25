+++

title = "9-Transitive Closure Revisited"
weight = 9

+++

### Transitive Closure Revisited

So we can solve abstract-transitive-closure problem for digraphs that- although it offers no improvement over a DFS based solution in the worst case- will provide an optimal solution in many situations.

The algorithm is based on preprocessing the digraph to build the latter's kernel DAG. The algorithm is efficient if the kernel DAG is small relative to the size of the original digraph. If the digraph is a DAG or if it has just few small cycles, we will not observe any significant savings. however is digraph has large cycles we can develop optimal/near-optimal algorithms. For clarity, we assume the kernel DAG to be sufficiently small that we can afford the adjacency matrix representation

To implement abstract transitive closure, we process the digraph as

- find strong components
- build its kernel DAG
- compute the transitive closure of kernel DAG

We can used any of three algorithms to find SC; A single pass through edges to make kernel DAG; and DFS to compute its transitive closure.

The vertices of DAG are the component numbers in the digraph for each edge $s-t$ in the original digraph, we simply set `D-> adj[sc[s]][sc[t]]` to 1. We would have to cope with duplicate edges in the kernel DAG if we are using an adjacency lists representation- in adjacency matrix duplicated edges simple correspond to setting to 1 a matrix entry that already has been set to 1. This point is significant because the number of duplicated edges is potentially huge.

**Property 19.17** *Given two vertices s and t in a digraph D, let $sc ( s )$ and $sc( t )$ , respectively, be their corresponding vertices in D’s kernel DAG K. Then, t is reachable from s in D if and only if $sc ( t )$ is reachable from $sc ( s
)$ in K.*

**Program 19.13 Strong Component based transitive closure**

````c++
template <class Graph> class TC
{
    const Graph &G; DenseGRAPH *K;
    dagTC<DenseGRAPH, Graph> *Ktc;
    SC<Graph> *Gsc;
    public :
    	TC(const Graph &G) : G(G)
        {
            Gsc = new SC<Graph> (G);
            K = new DenseGRAPH(Gsc->count(), true);
            for(int v = 0; v < G.V(); v++)
            {
                typename Graph::adjIterator A(G,v);
                for(int t = A.beg(); !A.end(); t = A.nxt())
                    K->insert(Edge(Gsc->ID(v), Gsc->ID(t)));
            }
            Ktc = new dagTC<DenseGRAPH, Graph> (*K);
        }
    ~TC() { delete K; delete Ktc; delete Gsc; }
    bool reachable (int v, int w)
    { return Ktc->reachable(Gsc->ID(v), Gsc->ID(w));}
};
````



**Property 19.18**  *We can support constant query time for the abstract transitive closure of a digraph with space proportional to $V + V^2$ and time proportional to $E + V^2 + vx$ for preprocessing (computing the transitive closure), where $v$ is the number of vertices in the kernel DAG and $x$ is the number of cross edges in its DFS forest.*

**Property 19.19** *We can support constant query time for the abstract transitive closure of any digraph whose kernel DAG has less than $^3\sqrt V $ vertices with space proportional to $V$ and time proportional to $E + V$ for preprocessing.*