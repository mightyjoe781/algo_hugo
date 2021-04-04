+++

title = "8-perspective"

+++

### Perspective

Our study of graph algorithm appropriately culminates in the study of network flow algorithms for four reason

- network flow model validates the practical utility of graph abstraction in countless application
- the maxflow and mincost-flow algorithms that we have examined are natural extension of graph algorithm that we studied for simpler problems.
- implementation exemplify the important role of fundamental algorithms and the data structures in achieving good performance
- the maxflow and mincost-flow models illustrate utility of approach of developing increasingly general problem-solving models and using them to solve broad classes of problems.

**Maximum matching** In a graph with edge weights, find a subset of edges in which no vertex appears more than once and whose total weight is such that no other such set of edges has a higher total weight. We can reduce the *maximum-cardinality matching* problem in unweighted graphs immediately to this problem by setting all edge weights to 1.

Assignment problem and maximum cardinality bipartite matching problem reduce to maximum matching for general graphs.

**Multicommodity flow** Suppose that we need to compute a second flow such that the sum of an edge’s two flows is limited by that edge’s capacity, both flows are in equilibrium, and the total cost is minimized.

**Convex and nonlinear costs** The simple cost function that we have been considering are linear combination of variables, and our algorithms for solving them depend in an essential way on the simple mathematical structure underlying these functions.

**Scheduling** They are barely representative of hundred of different scheduling problems that have been posed. Many scheduling problems reduce to mincost-flow model.

Scope of combinatorial computing is vase indeed, and study of problems of this sort is certain to occupy researchers for many years to come.

The broad reach of network-flow algorithms and our extensive use of reductions to extend this reach makes this section an appropriate place to consider some implications of the concept of reduction. For a large class of combinatorial algorithms, these problems represent a watershed in our studies of algorithms, where we stand between the study of efficient algorithms for particular problems and the study of general problem-solving models. There are important forces pulling in both directions.

