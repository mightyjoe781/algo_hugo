+++

title = "5-TextStrings and Index algorithms"

+++

### Text-String-Index Algorithms

The purpose of search here is to determine whether or not a search key is a prefix of one of the keys in the index, which is same as finding whether search key appears somewhere in the text strings.

A search tree that is built from keys defined by string pointers into a text string is called a suffix tree. We can use any algorithm that allows variable key indexing. Trie methods are particularly suitable because their run time doesn't depend on the key length, but rather depends on only the number of digits required to distinguish among the keys.

The characteristics lies in direct contrast to, for example hashing algorithms, which do not apply immediately to this problem because their run time is proportional to key length.

 for a typical text, standard BSTs would be the first implementation that we might choose,because they are simple to implement. interdependence of key might particularly when we are building a string index for each character position- is that the worst case for BSTs is not a particular position- is that the worst case for BSTs is not a particular concern for huge text, since unbalanced BSTs occur with only bizarre construction.

Patricia was designed for this purpose and in practice height of tree will be logarithmic and provides fast search implementation for misses because we do not need to examine all the bytes of the key.

TSTs afford several of the performance advantages of patricia, are simple to implement and take advantage of built-in byte-access operation that are typically found on modern machines. They also are amenable to simple implementations, that solve search problems more complicated than fully matching a search key. We can remove the code that handles the ends of keys in data structure because we are guaranteed that no string is a prefix of another, and thus we never will be comparing strings to their ends. one change that instead of keeping characters in nodes we keep pointer to strings in nodes so that every node refers to a position in the text string.



