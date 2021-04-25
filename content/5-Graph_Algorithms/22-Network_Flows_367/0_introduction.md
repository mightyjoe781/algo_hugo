+++

title = "0-introduction"

+++

### Network Flow

We will extend the network problem-solving model to encompass a flowing through the network, with different costs attached to different routes. These extensions allow us to tackle a surprisingly broad variety of problems with a long list of application.

These problems and application can be handled within a few natural models that can be inter-related using reduction.

Network-flow models encompass huge range of problems that fall into general categories known as *distribution* problems, *matching* problems, and *cut* problems.

*Distribution* problems deal with moving objects from one place to another within a network and typify the challenges involved in managing a large and complex operation.

**Merchandise distribution**

**Communication**

**Traffic Flow**

*Matching* problems, the network is all possible ways to connect a pair of vertices, and our goal is to choose connection without including any vertex twice.

**Job Placement**

**Minimum-distance point matching**

*Cut* problems we are required to cut network into two or more pieces and are fundamentally related to graph connectivity problems.

**Network Reliability**

**Cutting supply lines**

We will study to important particular problems within the *network-flow* model the *maxflow* and *mincost-flow* problems.

In basic properties of *flow network*, where we interpret a network's edge weights as capacities and consider properties of *flows*, which are a second set of edge weights that satisfy certain natural constraints. Next, we consider *maxflow*  problem, which is to compute a flow that is best in specific technical sense.

Maxflow algorithms and implementation prepare for us to discuss even more important and general model *mincost-flow problem*, where there is cost ( another set of edge weights) and define flow costs, so we do a maxflow problem that is of minimal cost.

We do a classical generic solution to mincost-flow problem known as *cycle-canceling* algorithm; then we give particular implementation of cycle canceling algorithm known as *network simplex.*

