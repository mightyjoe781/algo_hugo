+++

title = "1-Hashing Functions"

+++

### Hashing

The search algorithms that we have been considering are based on abstract comparison operation. A significant exception to this assertion is the key-indexed search method.

A extension of that method is hashing where keys don't have the fortuitous property of small range on then. The end result is a complete different approach to search from the comparison-based methods rather than navigating through dictionary data structures by comparing search keys with keys in items, we try to reference search keys with keys in items, we try to reference items in a table directly by doing arithmetic operations to transform keys into table addresses.

Search algorithms that use hashing have two parts.

1.  first step is computing hash function that transform the key into a table address
2. Collision-resolution process that deals with keys that are hashed to same values.

Hashing is good example of a time-space trade off.

Hashing is a classical computer-science problem : The various algorithms have been studied in depth and are widely used. We will see that under some generous assumption, it is not unreasonable to expect to support the search and insert symbol-table operation in constant time, independent of the size of the table.

This expectation is theoretical optimum performance for any symbol-table implementation, but hashing is not a panacea, for two primary reasons. First, the running time does depend on the length of the key, which can be liability in practical applications with long keys. Second hashing doesn't provide efficient implementations for other symbol-table operation, such as select or sort.

### Hash Functions

If we have a table that can hold M items, then we need a function that transforms keys into integers in range [0,M-1]. An ideal hash function is easy to compute and approximates a random function : For each input, every output should be in some sense equally likely.

Hash functions depends on the key type and strictly speaking we need a different hash function for each kind of key that might be used. For efficiency, we generally want to avoid explicit type conversion. Hash functions generally are dependent on the process of transforming keys to integers, so machine independence and efficiency are sometimes difficult to achieve simultaneously in hashing implementations.

Simplest situation is when keys are floating point numbers known to be in a fixed range. for ex. keys are in range 0 and 1 then we can just multiply by M and round off to nearest integer to get an address between 0 and M-1.

If keys are w-bits integers we can convert them to floating-point numbers and divide by $2^w$ to get floating-point numbers between 0 and 1, then multiply by M as in prev. para. If floating point operation are expensive and the nubers are not so large as to cause overflow, we can accomplish same result with arithmetic operation : Multiply the key by M then shift right w bits to divide by $2^w$.

Some functions are not useful unless keys are evenly distributed in the range.

A simpler and more efficient method for w-bits integers- one that is most commonly used for hashing- is to choose the table size M to be prime, and, for any integer key $k$, to compute the remainder when dividing $k$ by $M$, or $h(k) = k \mod M$ . Such a function is called a *modular* hash function. Its very easy to compute ( `k%M` in c++), and is effective in dispersing the key values evenly among the values less than M.

We can use modular hashing for floating-point keys as well if keys are in small range we can scale to convert them to numbers between 0 and 1, multiply by $2^w$ to get $w$ - bit integer result, then use a modular hash function. Another alternative is just to use the binary representation of the key (if available) as the operand for the modular hashing function.

Modular hashing is completely trivial to implement except for the requirement that we make the table size prime. For some application we can be content with a small known prime, or we can look up a prime number close to the table size that we want in a list of known primes. for example number of the form $2^{t-1}$ are prime for $ t = 2, 5, 7,13,17,19 \ and \ 31 (\text{and no other t<31})$ These are famous *Mersenne Primes.*

Another alternative is to combine multiplicative and modular methods.

Multiply the key by a constant between 0 and 1 then reduce it modulo M. That is use $ h(k) = \lfloor ka \rfloor \mod M$.

There is interplay among values of a, M , and effective radix of the key that could possible result in anomalous behavior,

A popular choice for a is $\phi = 0.618033...$ ( *The golden ratio*)

##### How do we calculate hash of strings and possibly long strings.

given `averylongkey`, In 7-bits ASCII word corresponds to 84-bits number $97.128^{11} + 118.128^{10}+101.128^9+...$ which is too large to be represented for normal arithmetic functions in most computers.

To compute a modular hash function for long keys we transform keys piece by piece. We can take advantage of arithmetic properties of the `mod` function and use *Horner's Algorithm*

**Hash function for string Keys**

````c++
int hash (char *v,int M)
{ int h = 0, a = 127'
  for(;*v!=0;v++)
      h = (a*h+*v) % M;
 return h;
}
````

Above representation can be done this way also

![image-20201231172845960](/1_Hashing_Functions.assets/image-20201231172845960.png)

That is we can compute decimal number corresponding to character encoding of a string by proceeding left to right, multiplying the accumulated value by 128, the adding the encode value of the next character. But one thing to node we might get big number while doing this but we don't need it so we will take modulo at every step

**Universal Hash Function ( for string keys )**

````c++
int hashU(char *v, int M)
{
    int h, a = 31415, b = 27183;
    for(h = 0; *v!=0; v++, a = a*b% (M-1))
        h = (a*h + *v) % M;
    return (h<0) ? (h+M):h;
}
````

In summary to use hashing for abstract symbol-table implementation, the first step is to extend the abstract type interface to include a `hash` operation that maps keys into non-negative integers less than `M` the table size.

````c++
inline int hash(Key v, int M)
{ return (int) M*(v-s)[/](t-s);}
````

does the job for floating point numbers in between `s` and `t`. for integer keys, we return `v%M`. If M is not prime the hash function might return

````c++
(int) (0.616161* (float) v ) % M
````

A bug that typically arises in hashing implementations if for the hash function always return the same value, perhaps because an intended type conversion did not take place properly. Such a bug is called a *performance bug* because program using such a hash function is likely to run correctly, but to be extremely slow.

We can use $\chi ^2$ statistic to test the hypothesis that a hash function produces random values, but this requirement is perhaps too stringent. Indeed, we might be happy if the hash function produces each value the same number of times, which corresponds to a $\chi^2$ statistic that is equal to 0, and is decidedly not random.