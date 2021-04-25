+++

title = "4-Binary search"

+++

### Binary Search

divide the set of items into two parts, determine to which of the two parts the search key belongs, then concentrate on that part. A reasonable way to divide set of items is to keep items sorted, then to use indices into the sorted array to delimit the part of the array being worked on.

This is *Binary Search*

*Property 5:* Binary search never uses more than $\lfloor \lg N \rfloor +1$ comparisons for a search ( hit or miss)

**Binary Search(for array-based symbol)**

````c++
private:
	Item searchR(int l,int r, Key v){
        if(l>r) return nullItem;
        int m = (l+r)/2;
        if(v==st[m].key()) return st[m];
        if(l==r) return nullItem;
        if(v<st[m].key())
            return searchR(l,m-1,v);
        else return searchR(m+1,r,v);
    }
public:
	Item search(Key v)
    { return searchR(0,N-1,v);}
````

If we need to insert new items dynamically, it seems that we need a linked structure, but a singly linked list does not lead to efficient implementation, because efficiency of binary search lies in the fact that we can access the array indices quite fast.

for getting both benefit we might require a complicated data structure.

If duplicate keys are present in the table, then we can extend Binary search to support symbol-table operation for counting the number of items with a given key or returning them as a group.

One direct improvement in binary search is called as interpolation search. In this instead of blindly selecting mid element we make a informed decision like finding a name in dictionary that starts with a,z then we should most likely see at the start and end of dictionary.

we replace the statement `m=(l+r)/2` with

`m = l+(v-a[l].key())*(r-l)/(a[r].key()-a[l].key());`

For random keys, it is possible to show this uses fewer comparisons of around $\lg \lg N+1$ for a search(hit or miss).

However its performance is based on assumption that keys are well distributed over the interval. Its quite a good improvement as compared to Binary search say for a 1 billion uniformly distributed data $\lg\lg N+1 < 5$ .

So interpolation is method of choice when size of file is very big or interpolation search certainly a choice.

