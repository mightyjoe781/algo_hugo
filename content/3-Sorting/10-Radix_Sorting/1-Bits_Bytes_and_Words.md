+++

title = "1-Bits Bytes and Words"

+++

### Bits, Bytes and Words

Some important points

- computers are built to process bits in groups called machine words, which is often grouped into smaller pieces call bytes
- sort keys are also are commonly organized as byte sequences
- small byte sequences can also serve as array indexes or machine addresses

***Def 1:*** A byte is  a fixed sequence of bits; a string is variable-length sequence of bytes; a word is a fixed-length sequence of bytes.

````c++
const int bitsword = 32;
const int bitsbyte = 8;
const int bytesword = bitsword/bitsbyte;
const int R = 1 << bitsbyte;
````

We can extract `Bth` byte of a binary word using this snippet

````c++
inline int digit(long A, int B)
	{ return (A>>bitsbyte*(bystesword-B-1)) & (R-1);}
````

Another way is arrange things that the radix is aligned with byte size and therefore a single access will get the right bits quickly.

````c++
inline int digit(char* A, int B)
{ return A[B];}
````

At a different level of abstraction, we can think of keys as numbers and bytes as digits. Given a (key represented as a) number, the fundamental operation needed for radix sorts is to extract a digit from the number. When we chose a radix that is a power of 2, the digits are groups of bits , which we can easily access directly using one of the macros above.

Indeed, the primary reason that we use radices that are powers of 2 is that the operation of accessing groups of bits is inexpensive.

$\lfloor \frac{a}{R^b} \rfloor \  mod\  R$

***Def 2 :*** A key is a radix- $R$ number, with digits numbered from the left (starting at 0)

