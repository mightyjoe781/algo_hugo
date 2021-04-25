+++

title = "2-Heap Data Structure"

+++

## 2. Heap Data Structure

*Definition:* A tree is heap-ordered if the key in each node is larger than or equal to the keys in all of that node's children (if any).

Equivalently, the key in each node of a heap-ordered tree is smaller than or equal to the key in that node's parent (if any).

***Property : 1*** Key of the Root Node is largest.

*<u>definition</u>* : A heap is a set of nodes with keys arranged in a complete heap-ordered binary tree, represented as an array.

Complete trees provide opportunity to use a compact array representation where we can easily get from node to its parent and children without maintaining explicit links.

The parent of the node in $i$ position is $ \lfloor i/2 \rfloor $ and its two children node are $2i$ and $2i+1$.

All paths in a complete tree are $lg N$ nodes on them so we could use linked list if we wanted a efficient implementation of Join Operation of PQ.



