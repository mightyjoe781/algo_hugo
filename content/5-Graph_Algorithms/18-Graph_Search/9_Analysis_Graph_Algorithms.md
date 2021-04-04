+++

title = "9-Analysis Graph Algorithms"

+++

### Analysis of Graph Algorithms

we seek ideally natural input models that have 3 critical properties

- They reflect reality to a sufficient extent that we can use them to predict performance.
- They are sufficiently simple that they are amenable to mathematical analysis
- We can write generators that provide problem instances that we can use to test our algorithms.

When the running time of an algorithms depends on the structure of input graph, prediction are much harder to come by.

**Property 18.13** * If $ E > \frac 1 2 V \ln V +\mu V$ (with $\mu$ positive), a random graph with $V$ vertices and $E$ edges consists of a single connected component and an average of less than $e^{-2\mu}$ isolated vertices, with probability approaching $1$ as $V$ approaches infinity*

