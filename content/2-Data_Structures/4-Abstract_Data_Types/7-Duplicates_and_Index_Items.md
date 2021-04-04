+++

title = "7-Duplicates and Index Items"
weight = 7
+++

## Duplicate and Index Items

- In general to remove duplicates we have two possibility
  - assume when client pushes duplicates we ignore them
  - or remove old item and add new item there
- first one is difficult to implement than the second because it requires that we modify the data structures.
- This kind of implementation accounts for using the symbol table ADT for quick search.
- Special case for which we have a quick solution; say items are integers in range 0 to M-1.
  - we can maintain an another as a hashing table for fast access and this method can used with both implementations
- This case appears quite commonly and is very efficient way to implement unique pushdown stack.

