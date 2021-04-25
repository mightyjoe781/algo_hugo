+++

title = "0-Introduction"

+++

### Radix Sorting

This chapter focuses on different abstractions for sort keys.

We often don't need to compare all words we achieve this efficiency in sorting algorithms, we shift from the abstract operation where we compare keys to an abstraction where we decompose keys into a sequence of fixed sized pieces, or bytes.

Binary numbers : sequence of bits , Strings : sequences of digits, and many other types of keys can be viewed in this way.

Sorting methods build on processing numbers one piece at a time are called radix sorts. These methods do not just compare keys : They process and compare pieces of keys.

Radix sorting algorithms treat the keys as numbers represented in a base-R number system, for various values of R( the radix ), and work with individual digits of numbers.

There are two, fundamentally different , basic approaches to radix sorting.

1. Algorithms that examine the digits in the keys in a left-to-right order working with most significant digits first referred as **MSD Radix sort.**

   They are attractive since they examine the minimum amount of information necessary to get a sorting job done.

   It kind of generalize the quicksort, because it works by partitioning the file to be sorted according to leading digits of keys and recursively applying the same method.

2. Algorithms based on right to left order, working with least significant-digit referred as **LSD Radix sort**

   they are somewhat counterintuitive since they spend most of time in processing digits that don't affect result. Still its a method of choice in many applications.

    