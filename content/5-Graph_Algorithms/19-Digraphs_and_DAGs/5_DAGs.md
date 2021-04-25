+++

title = "5-DAGs"
weight = 5

+++

### DAGs

The prototypical application where DAGs arise directly is called *scheduling*. Generally, solving scheduling problems has to do with arranging for the completion of a set of *tasks*, under a set of *constraints*, by specifying when and how the tasks are to be performed. Most important type of constraints are *precedence constraints*.

**Scheduling** Given a set of tasks to be completed, with a partial orders that specifies that certain tasks have to be completed before certain other tasks are begun, how can we schedule the tasks such that they are all completed while still respecting the partial order ?

In this basic form this problem is known as *topological sorting*.

First task is to check whether or not a given DAG indeed has no directed cycles. we can do it in linear time.

In a sense, DAGs are part tree, part graph. We can certainly take advantage of their special structure when we process them.

**Definition 19.6** *A **binary DAG** is a directed acyclic graph with two edges leaving each node, identified as the left edge and the right edge, either or both of which may be null.*

DAG Model of Fibonacci sequence

![image-20210114224722086](/5_DAGs.assets/image-20210114224722086.png)

The distinction between binary DAG and a binary tree is that in binary DAG we can have more than one link pointing to a node. They are significant in sense they can be used to compress information represented by binary tree.

![image-20210114224912091](/5_DAGs.assets/image-20210114224912091.png)

