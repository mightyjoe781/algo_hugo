+++

title = "Sorting"
date = 2021-04-12T16:01:25+05:30
weight = 4

+++

#### Counting Inversion 

See UVa Problems 11495, 13212, 10810 ( all based on counting the inversions)

Inversion are basically swaps. So we can just add `cnt++` to any standard sorting algorithm to get number of inversions.

Secondly most efficient way to do it $nlogn$ just like most efficient sorting possible.

Sometimes questions might ask you to give inversion if only alternate swaps are allowed. (one possible direction might indicate bubble sort) but still correct choice is to use modified mergesort

````c++
typedef long long ll;
const maxN = 1000000;
//merging step
ll merge(ll a[],ll lo,ll mid, ll hi){

    ll inv = 0;
    static ll aux[maxN];
        // copy to aux[]
        for (ll k = lo; k <= hi; k++) {
            aux[k] = a[k];
        }

        // merge back to a[]
        ll i = lo, j = mid+1;
        for (ll k = lo; k <= hi; k++) {
            if      (i > mid)           a[k] = aux[j++];
            else if (j > hi)            a[k] = aux[i++];
            else if (aux[j] < aux[i]) { a[k] = aux[j++]; inv += (mid - i + 1); }
            else                        a[k] = aux[i++];
        }
    return inv;
}
// actual sort
ll mergesort(ll a[],ll l, ll r){
    ll inv = 0;
    if(r<=l) return 0;
    ll m = l+(r-l)/2;
    inv += mergesort(a,l,m);
    inv += mergesort(a,m+1,r);
    inv += merge(a,l,m,r);
    return inv;
}
````

 Used `long long` because mostly if $ n \log n $ is wanted complexity in question then to make upper bounds tight on the normal sorting methods , input will most probably be of order of `long long`.

