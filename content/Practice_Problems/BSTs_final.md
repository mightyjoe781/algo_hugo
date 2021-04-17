+++

title = "BST Review"
date = 2021-04-17T09:01:25+05:30
weight = 10

+++

#### Review

Ordered Search Structures  we use whenever there is a need for ordering. Great examples are BSTs and we used lower_bound and upper_bound

set,map => sorted order.

Lets review some of the basics of BSTs. // don't ask me why its not on top.

Date Models : usually the most of data we encounter is multidimensional so for such a multidimensional data we use identifiers ( like primary keys).

so its exhibits a key , value model

key - integer and value can be anything from primitive data to struct.

all maps and sets are identical (internal implementation) and analogues with each other except in their use case , so maps and sets are implemented using BSTs.

There is a important point of difference that is how they differentiate items

- Maps use *key* for item comparisons ( ordering )
- Sets use *data* for comparison ( ordering )

Similarly unordered_maps and unordered_sets

- unordered_maps use *key* for hashing
- unordered_sets use entire data for hashing

#### BSTs (Ordered Search Structures)

- Insertion ($O (\log n)$)
- Deletion ($O (\log n)$)
- Search ($O (\log n)$)
- lower_bound ($O (\log n)$)
- upper_bound ($O (\log n)$)



BST have a property that the root node is always less than the all the element of tree rooted at the left children of the root and greater than equal to all the elements of tree rooted at the right children of the root.



Search  : exact simulation of binary search

Upper Bound : do exact same step as execute search and for each node that you visit and keep checking

````c++
if(n.value > I){
    if(n.value - I < min-diff){
        min_diff = n.val-I;
        ans = n.val;
    }
}
````

Basically we are trying to find the minimum difference at each node to take branching decision.

Terminology .

Path (s,e) : paths that start for the root and end at a leaf node.

Height of a Tree : length of longest path.

