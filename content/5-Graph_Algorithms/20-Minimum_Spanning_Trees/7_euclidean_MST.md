+++

title = "7-Euclidean MST"

+++

### Euclidean MST

Given N points in the plane and we want to find the shortest set of lines connecting all the points. The geometric problem is called as *Euclidean MST* problem.

One way to solve it is to build a complete graph with N vertices and N(N-1) /2 edges- one edges connecting each pair of vertices weighted with the distance between the corresponding points. Then we can use Prim's algorithm to find the MST in time proportional to $N^2$.

This solution is too slow. The geometric structure makes most of edges in the complete graph irrelevant to the problem, and we don't need to add most of them to the graph before we construct the MST

**Property 20.13** *We can find Euclidean MST of N points in time proportional to $N\log N$*.

This fact is direct consequence of two basic facts. First a graph known as *Delauney triangulation* contains MST. by definition. Second, the *Delauney triangulation* is a planar graph whose number of edges is proportional to N.

So in principle we can calculate Delauney triangulation in time proportional to N log N, then run Kruskal's algorithm or priority search method to find Euclidean MST.

**Property 20.14** *Finding the Euclidean MST of N points is no easier than sorting N numbers.*