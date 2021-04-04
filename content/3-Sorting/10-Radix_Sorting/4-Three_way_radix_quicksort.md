+++

title = "4-Three way radix quicksort"

+++

### Three way Radix Quicksort

another way to adapt quicksort for MSD radix sorting is to use three-way portioning on the leading byte of the keys, moving to next byte on only the middle sub file.

It might be regarded as hybrid of two algorithms.

Comparing the three-way radix quicksort to standard MSD radix sort, we note that it divides the file into only three parts, so it doesn't get benefit of quick multiway partition, especially in early stages of sort but on the other hand during later stages of sort MSD radix sort involves large numbers of empty bins, whereas three-way radix sort adapts well to duplicate keys,keys that fall in short range, small files , no need of auxiliary array, and deals with different types of non randomness in different parts of the key.

***Three-way radix quicksort***

````c++
#define ch(A) digit(A,d)
template <class Item>
void quicksortX(Item a[], int l, int r, int d){
    int i,j,k,p,q; int v;
    if (r-l <= M) {insertion(a,l,r); return;}
    v = ch(a[r]); i=l-1; j = r; p =l-1; q=r;
    while(i<j){
        while(ch(a[++i])<v);
        while(v<ch(a[--j])) if(j==l) break;
        if(i>j)break;
        exch(a[i],a[j]);
        if(ch(a[i])==v) {p++; exch(a[p],a[i]);}
        if(v == ch(a[j])) { q-- ; exch(a[j],a[q]);}
    }
        if(p == q)
        	{ if(v!='\0') quicksortX(a,l,r,d+1);
             return;
        	}
        if(ch(a[i])<v) i++;
        for(k=l;k<=p;k++,j--) exch(a[k],a[j]);
        for(k=r;k>=q;k--,i++) exch(a[k],a[i]);
        quicksortX(a,l,j,d);
        if((i==r) && (ch(a[i])== v)) i++;
        if(v!='\0') quicksortX(a,j+1,i-1,d+1);
        quicksortX(a,i,r,d);
}
````

Three way algorithm avoids the scanning of all bytes of string and gains efficiency because it can partition just by looking at few mismatching bytes.

Quite performance Boost in when keys are long.