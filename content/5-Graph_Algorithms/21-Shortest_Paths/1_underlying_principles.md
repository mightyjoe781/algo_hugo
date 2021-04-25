+++

title = "1-Underlying principles"

+++

### Underlying Principles

Our shortest path algorithms are all based on a simple operation know as *relaxation*. We start a shortest paths algorithm knowing only the network's edges and weights. As we proceed, we gather information about shortest paths that connect various vertices and algorithms update this info incrementally, making new conclusion based information collected.

At each step, we test whether we can find a path that is shorter than some know path.

We apply repeatedly two operations

*Edge relaxation* Test whether travelling along a given edges gives a new shortest path to its destination vertex.

*Path relaxation* Test whether traveling through a given vertex gives a new shortest path connecting two other given vertices.

Data structure we need to support this operation are straightforward. First, we have the basic requirement that we need to compute the shortest paths length from source to each of other vertices and store it in vector-indexed vector wt the lengths of shortest known path from source to each vertex. second we record the paths themselves as we move from vertex to vertex, our convention will be the same as the one that we used for other graph-search algorithm. We use a vertex-indexed vector to `spt` to record the last edge on a shortest path form the source to indexed vertex. These edges constitute tree.

In single source shortest-paths code, we use following code to relax along edges e from v to w:

````c++
if(wt [w] > wt[v] + e->wt())
{	wt[w] = wt[v] + e->wt(); spt[w] = e; }
````

**Definition 21.2**  *Given a network and a designated vertex s, a **shortest paths tree (SPT)** for s is a subnetwork containing s and all the vertices reachable from s that forms a directed tree rooted at s such that every tree path is a shortest path in the network.*

One way to get edges on the path in order from source to sink from an SPT is to use a stack.

````c++
stack <EDGE *> P; EDGE *e = spt[w];
while (e) { P.push(e); e = spt[e->v()]); }
if (P.empty()) cout << P.top()->v();
while (!P.empty())
	{ cout << ’-’ << P.top()->w(); P.pop(); }
````

In class implementation we could provide similar coded vector that contains edges of the path. There is another way to simply use reverse graph.

Next lets consider path relaxation, which is basis of some of our all pair algorithms : Does going through a given vertex lead us to a shorter path that connects two other given vertices ? For that we need to have some knowledge about the path lengths to take decision.

Path relaxation is appropriate for all-pairs solution where we maintain lengths of shortest paths that we have encountered between all pairs of vertices. Specifically, in All-pair-shortest-paths code of this kind, we maintain a vector of vectors d such that `d[s][t]` is shortest path length from s to t, we maintain a vector of vector p such that `p[s][t]` is the next vertex on a shortest path from `s` to `t`. We refer former as *distances* and *paths* matrices respectively :)

Distance matrix is prime objective of computation but paths matrix stores more clear and compact information as, the full list of paths.

**Property 21.1** *If a vertex x is on a shortest path from s to t, then that path consists of a shortest path from s to x followed by a shortest path from x to t.*

We encountered the path-relaxation operation when we discussed transitive closure algorithms. If the edge and path weight either 1 or infinite, then path relaxation is the operation that we used in Warshall's algorithm ( if we have path from s to x and a path x to t, then we have a path from s to t). If we define a path's weight to be the number of edges on that path,  then Warshall's algorithm generalizes the Floyd's algorithm for finding all shortest paths in unweighted digraphs it generalized to apply to network.

In summary, relaxation gives us the basic abstract operation that we need to build our shortest paths algorithms. The primary complication is the choice of whether to provide the first or final edge on the shortest path. For example, single-source algorithms are more naturally expressed by providing the final edge on the path so that we need only a single vertex-indexed vector to reconstruct the path, since all paths lead back to source.

This choice does not represent a fundamental difficulty because we can either use t he reverse graph as warranted or provide member functions that hide this difference from clients.

