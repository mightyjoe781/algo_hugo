+++

title = "2-Anatomy of DFS in Digraphs"
weight = 2
+++

### Anatomy of DFS in Digraphs

Search  trees to understand operation of algorithms is quite complicated for digraphs.

We can invoke digraphs using our previous implementation of undirected graphs by passing true argument in constructor call.

![image-20210114090640108](/2_Anatomy_of_DFS_in_Digraphs.assets/image-20210114090640108.png)

In digraphs, there is a one-to-one correspondence between tree links and graph edges, and they fall into four distinct classes :

- Those representing a recursive call (tree edges)
- Those from a vertex to an ancestor in its DFS tree (back edges)
- Those from a vertex to a descendent in its DFS tree (down edges)
- Those from a vertex to another vertex that is neither an ancestor nor a descendent in its DFS tree (cross edges)

**Property 19.3** *In a DFS forest corresponding to a digraph, an edge to a visited node is a back edge if it leads to a node with a higher postorder number; otherwise, it is a cross edge if it leads to a node with a lower preorder number and a down edge if it leads to a node with a higher preorder number*

These edge types are properties of dynamics of the search rather than of only the graph. That is why DFS forests on same graph differ remarkably in character.

**Program 19.2 DFS of a digraph**

````c++
template <class Graph> class DFS
{
    const Graph &G;
    int depth,cnt,cntP;
    vector<int> pre,post;
    void show(char *s, Edge e)
    { for(int i=0 ; i < depth ; i++) cout<<" ";
    	cout<<e.v << "-" << e.w <<s<<endl;}
    void dfsR(Edge e){
        int w = e.v; show(" tree",e);
        pre[w] = cnt++; depth++;
        typename Graph::adjIterator A(G,w);
        for(int t = A.beg(); t != A.end() ; t = A.nxt())
        {
            Edge x(w,t);
            if (pre[t]==-1) dsfR(x);
            else if (post[t] == -1) show(" back",x);
            else if (pre[t]>pre[w]) show(" down",x);
            else show(" cross",x);
        }
        post[w] = cntP++; depth--;
    }
    public:
    	DFS(const Graph &G) : G(G), cnt(0), cntP(0),
    	pre(G.V(),-1), post(G.V(),-1)
        {
            for(int v = 0; v < G.V(); v++)
                if(pre[v] == -1) dfsR(Edge(v,v));
        }
};
````



**Directed Cycle Detection** : Does a given digraph have any directed cycles ? (is the digraph a DAG?) just do a dfs and if we encounter a visited vertex then that means there is a cycle.

**Property 19.4** *A digraph is a DAG if and only if we encounter no back edges when we use DFS to examine every edge*

We can convert any digraph into a DAG by doing a dfs and removing the graph edges that corresponds to back edges in DFS.

**Single Source Reachability**: Which vertices in a given digraph can be reached from a given start vertex s ? How many such vertices are there?

**Property 19.5** *With a recursive DFS starting at s, we can solve the single-source reachability problem for a vertex s in time proportional to number of edges in subgraph induced by reachable vertices.*

DFS does not give full information about reachability from any vertex other than the start node.