+++

title = "4-Equivalence Relations and Partial Orders"
weight = 4

+++

### Equivalence Relation and Partial Orders

Given a set, a *relation* among its objects is defined to be a set of ordered pairs of the objects.

a relation R is said *symmetric* if sRt implies that tRs for all s and t; (same as undirected graph)

it is *reflexive* if sRs for all s. (related to self loops)

relation that correspond to graphs where no vertices have self-loops are said to be *irreflexible.*

A relation R is said to be *transitive* when sRt and tRu implies that sRu for all s,t and u.

The *transitive closure* of a relation is well-defined concept but instead of redefining it in set-theoretic terms, we appeal to the definition that we gave for digraphs.

An *equivalence relation* is a transitive relation is also reflexive and symmetric. Equivalence relation divide the objects in a set into subsets known as *equivalence classes*. Examples of it

**Modular arithmetic** Any positive integer $k$ defines an equivalence relation on the set of integers, with $st(mod \ k) $ if and only if the remainder that results when we divide $s$ by $k$ is equal to remainder that results when we divide $t$ by $k$.

**Connectivity in Graph** Relation "is in the same connected components as" among vertices is an equivalence relation because it is symmetric and transitive.

A partial order $ \prec $ is a transitive relation that is also irreflexive. As a direct consequence of transitivity and irreflexivity, it is trivial to prove that partial orders are also *asymmetric* : and partial order can't contain cycles. (last property :)

**Subset inclusion** The relation "include but is not equal to" ($\subset$) among subsets of a given set is a partial order - it is certainly irreflexive, and if $s\subset t$ and $ t\subset u$, then certainly $s\subset u$.

**Paths in DAGs** The relation " can be reached by a nonempty directed path from" is partial order on vertices in DAGs with no self-loops because its transitive irreflexive.



A *total order* T is a partial order where either $sTt$ or $tTs$ for all $s \neq t$. familiar examples of total orders are the "less than" relation among integer or real numbers and lexicographic ordering among string of characteristic.

In summary the following correspondences between sets and graph models help us to understand the importance and wide applicability of fundamental graph algorithms.

- Relations and digraphs
- Symmetric relation and undirected graph
- Transitive relation and paths in graphs
- Equivalence relation and paths in undirected graphs
- Partials orders and paths in DAGs