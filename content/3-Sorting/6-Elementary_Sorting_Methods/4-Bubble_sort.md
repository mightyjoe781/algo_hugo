+++

title = "4-Bubble_sort"

+++

### Bubble Sort

Most simple and worse performance than both techniques.

traverse from right to left through the file whenever min element is encountered during the first pass, we exchange it with each of the elements to its left, hoping to eventually put it to its place.

````c++
template <class Item>
void bubble(Item a[] , int l , int r){
    for(int i = l ; i<r ; i++)
        for(int j = r ; j > i; j--)
            compexch(a[j-1],a[j]);
}
````

***Stable Sort***

Thus N passes suffice, and bubble sort operates as a type of selection sort, although it does too much work to put it into its position.

