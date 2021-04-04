+++

title = "3-Stack Size"

+++

### Stack Size

for a random file, max size of stack can be proportional to $ log N $ but stack can grow up to $N$ for a degenerate case.

Order in which sub files are processed doesn't affect the correct operation of the algorithm, or the time taken, but might affect the size of pushdown stack.

The policy of putting the larger of the smaller sub files on the stack ensures that each entry on the stack is no more than one-half of the size of the one below it, so stack needs to make a room for only $\lg N $ entries.

Stack is maximum when partition always falls at the center of the file.

***If the smaller of the two subfiles is sorted first, then the stack never has more than $ \lg N $ entries when quicksort is used to sort N elements.***

**Quicksort Partitioning Tree**

Collapsing the partitioning diagram by connecting each partitioning element to the partitioning element used in its two sub files we get a static representation of t he partitioning process.