+++

title = "7-Index implementations with Symbol Tables"

+++

### Index Implementations with Symbol Tables

For some applications we want a search structure simply to find items, and not to move them around.

We can adapt BSTs to build indices in precisely the same manner as we provided indirection for sorting in Section 6.8 and for heaps : Use an `Index` wrapper to define items for the BST, and arrange for keys to be extracted from items via the `key` member functions, as usual.

Moreover we could use parallel arrays for the links, as we did for the linked lists. We could use three arrays, one each for the items, left links, and right links.

So we use `x = l[x]` instead of `x=x->l`.

This approach avoids **cost of dynamic memory allocation** for each node - the items occupy an array without regard to the search function, and we preallocate tow integers per item to hold the tree links, recognising that we will at least this amount of space when all the items are in the search structure.

Another advantage is that it allows extra arrays to be added without the tree-manipulation code being changed at all. When the search routine return the index for an item, it gives a way to access immediately all the information associated with that item, by using the index to access an appropriate array.

This way is useful sometimes because we don't have to copy items into the internal representation of the ADT, and overhead of allocation and construction by `new`. Though this strategy is not useful when space is premium or when symbol table grows and shrinks markedly.

An important application is to provide keyword searching in a string of text. It read a text string from an external file. Then , considering each position in the text string to define a string key starting at that position and going to the end of the string, it inserts all the keys into a symbol table, using string pointers. This use for string keys differ from a string-item type definition because no storage allocation is involved.

The string keys that we use are arbitrarily long, but we maintain only the pointers to them and we look at only enough characters to decide which one of the two strings should appear first. No two string are equal , but if we modify `==` to consider two strings to be equal if one is prefix of the other, we can use the symbol table to find whether a given query string is in the test simply by calling `search`.

