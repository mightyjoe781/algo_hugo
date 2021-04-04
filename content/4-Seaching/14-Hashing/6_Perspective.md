+++

title = "6-Perspective"

+++

### Perspective

All methods can reduce the symbol table search and insert function to constant-time operations and all are useful for a broad variety of application.

Roughly, we ca characterize the three major methods (linear probing, double hashing , and separate chaining):

Linear probing is fastest of three, double hashing makes most efficient use of memory and separate chaining is easiest to implement and deploy.

Double hashing is slower than separate chaining and linear probing for sparse tables but is much faster than linear probing when the table fills up.

The choice between linear probing and double hashing depends primarily on the cost of computing the hash function and on the load factor of the table. for sparse tables ( small $\alpha$), both methods use only a few probes but double hashing could take more time because  it has to compute two hash function for long keys.

as $\alpha \rightarrow 1$ double hashing far outperforms linear probing.

