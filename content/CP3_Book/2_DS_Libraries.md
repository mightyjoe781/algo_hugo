+++

Title = "Data Structures and Libraries"
weight = 2

+++

### Linear DS with Built-in Libraries

- #### Static Array

  - used for sequential collections of data
  - generally should be declared with some extra buffer than mentioned in problem

- #### Dynamically-Resizable Array

  - C++ STL `vector` or Java `ArrayList`
  - same as array but with additional ability of runtime resizability
  - Typical Operations
    - `push_back()` , `at()`, `assign()` , `erase()`

- #### Array of Booleans: 

  - `C++ STL bitset` (`Java BitSet`)

- #### Bitmasks

  - lightweight, small set of Booleans
  - using integers to represent Boolean values.
  - Some common operations
    - f

- #### Linked-List

  - `C++ STL list` or `Java LinkedList`
  - usually to be avoided in contest due to slow access

- #### Stack

  - `C++ STL stack` or `Java Stack`
  - Used in
    - Postfix calculator and Infix to Postfix conversion
    - Strongly connected components
    - Graham's Scan
  - Constant time insertion(at front) as well as deletions (at front)
  - Typical Operation
    - `push(<val>)` , `pop()` , `top()`, `empty()`
  - *LIFO*

- #### Queue

  - `C++ STL queue` or `Java Queue`
  - Used in
    - BFS 
  - Constant time insertion(at back) as well deletions (at front)
  - Typical Operation
    - `push(<val>)` , `pop()` , `front()`, `back()`,`empty()`
  - *FIFO*

- #### Double-Ended Queues

  - `C++ STL deque` or `Java Deque`
  - similar to resizable array and queue above
  - fast insertion and deletion at both ends
  - Uses
    - Sliding window algorithm
  - Typical Operation
    - `push_back()`, `pop_front()` ,`push_front` , `pop_back()` and other normal queue operations

### Non-Linear DS with Built-in Libraries

- #### Balance Binary Search Tree (BST)

  - `C++ STL map/set` or Java `TreeMap/ TreeSet`
  - at each node items on the left are smaller than the node and items on the right side are greater than the node. Essentially this is strategy of Divide and Conquer Algorithm
  - Typical Operations
    - `search(key)` , `insert(key)`, `findMin() / findMax()`,`successor(key) `, `predecessor(key) `, `delete(key)`
  - $ log(n)$ time in worst case of search and insert and deletions given that tree is balanced
  - these are usually implementation of RB trees.
  - C++ STL map stores (key-> data) but C++ STL set only stores key

- #### Heap

  - `C++ STL priority_queue ` or `Java Priority Queue`
  - Heap is also like BST but it must be complete
  - can be efficiently store in array off size n+1 which is explicit representation of graph using array
  - we can reach to left/right child using index jump 2n/2n+1 and parent by n/2
  - faster way of doing above tasks is i >>1 `i/2` , i<<1 `2n` or (i<<1)+1 `2n+1`
  - it enforces heap property so that the element above a node is always maximum but element below is always small
  - there is no notion of search in heap instead use for fast extraction of Maximum item `ExtractMax()` and insertion `Insert(v)`
  - Uses
    - Prim's ( and Kruskal's) algorithm for MST,
    - Dijkstra's algorithm for Single-Source Shortest Paths
    - A* Search algorithm

- #### Hash Table

  - `C++ STL unordered_map` or `Java HashMap`
  - do not use generally in cp
  - designing it is very tricky
  - maps and sets are fast enough for 1M input
  - However a simple form of Hash Tables can be used in programming contest.
  - DATs - Direct Access Tables

### Data Structures with Our Own Libraries

​	These are not in built in into the STL so should be prepared beforehand

- #### Graph

  - `G = (V,E)` is just set of edges and vertices
  - Three ways to represent them 
    - Adjacency Matrix (2D Array) -Not good for sparse graph as space is O(n^2^)
    - Adjacency list- usually in form of a vector of vector of pairs
      - `typedef pair<int,int> ii;` //pairs then `typedef vector <ii> vii;` //vector of pairs then simply declare  `vector<vii> AdjList;`
      - In Java `Vector< Vector < IntegerPair > > AdjList`
    - Edge Lists
      - C++ STL: `vector< pair<int, ii> > EdgeList`
      - Java API: `Vector< IntegerTriple > EdgeList`
      - This representation is useful for Kruskal's algorithm for MST
      - Useful in storing sorted Edges
  - Some graphs that do not need to be stored in graph structures is called implicit graph and they come in two flavors 
    - The edges can be determined easily
    - Edges can be determined according to some rule

- #### Union-Find Disjoint Sets

  - Used to solve connected components in undirected graphs

  - efficient (in O(1)) determine whether item belongs to a set and to unite two disjoint sets in one larger set.

  - we use roots as representatives

  - so we store index of parent item and its rank in two vectors of integers `vi p and vi rank` and design s.t. representative element is a self loop with `p[i] = i ` and `rank[i]` yields the height of the tree rooted at item i.

  - disjoint set with higher rank to be the new parent of the disjoint set with lower rank using `unionSet(i, j)`

  - so function `findSet(i)` simply calls `findset(p[i])` recursively so to improve this we use union by rank heuristics.

  - `unionSet(4, 3)`, we have `rank[findSet(4)] = rank[4] = 0` which is smaller than `rank[findSet(3)] = rank[3] = `1, so we set `p[4]= 3` without changing the height of the resulting tree

    ````c++
    class UnionFind { // OOP style
    private: vi p, rank; // remember: vi is vector<int>
    public:
        UnionFind(int N) { rank.assign(N, 0);
        	p.assign(N, 0); for (int i = 0; i < N; i++) p[i] = i; }
        int findSet(int i) { return (p[i] == i) ? i : (p[i] = findSet(p[i])); }
        bool isSameSet(int i, int j) { return findSet(i) == findSet(j); }
        void unionSet(int i, int j) {
        	if (!isSameSet(i, j)) { // if from different set
        	int x = findSet(i), y = findSet(j);
        	if (rank[x] > rank[y]) p[y] = x; // rank keeps the tree short
        	else { p[x] = y;
        				if (rank[x] == rank[y]) rank[y]++; }
    } } };
    ````

    

- #### Segment Tree

  - this is structure is efficient for *dynamic range queries* 

- #### Binary Indexed (Fenwick Tree)

### Some Problems