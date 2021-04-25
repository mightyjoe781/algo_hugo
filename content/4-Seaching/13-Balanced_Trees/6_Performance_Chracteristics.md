+++

title = "6 Performance Chracteristics"

+++

### Performance Characteristics

How to choose among randomized BSTs, splay BTSs, red-black BTSs, and skip lists for a particular application.

implementation of rotation along the search path is an essential ingredient of most balanced tree algorithms.

Randomized BSTs are the simplest to implement with a prime requirements of having a confidence in the random-number generator and to avoid spending too much time generating the random bits. Splay BSTs are slightly more complicated but are a straightforward extension to standard root insertion algorithm. RB BSTs involve slightly more code still, to check and manipulate the color bits.

One advantage of RB Trees over the other two is that color bits can be used for consistency check for debugging, and for a guarantee of a quick search at any time during the lifetime of the tree.

Skip lists are easy to implement and are particularly attractive if a full range of symbol-table operation is to be supported, because search, insert, remove, join, select and sort all have natural implementation that are easy to form.

Splay BSTs require no extra space for balance information, RB BSTs require 1 extra bit, and randomized BSTs require count field.

