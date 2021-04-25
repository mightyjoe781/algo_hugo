+++

title = "3-MSD Radix Sort"

+++

### MSD Radix Sort

We sorted using 1 bit in radix quicksort amounts to treating keys as radix 2 (binary ) numbers and considering the most significant digits first. Generalizing on that we can develop sort for radix-R numbers by considering the array into R, rather than just two , different parts.

These partitions are referred as *Bins* or *Buckets* and think of algorithm as using a groups of R bins.

![image-20201205080145608](/3-MSD_Radix_Sort.assets/image-20201205080145608.png)

We pass through keys, distributing them among the bins, then recursively sort the bin contents on keys with 1 fewer byte.

Dynamical Characteristics : just one stage of MSD radix sort can nearly complete a sort task.

We can tune algorithm by adjusting value of R because there is a clear tradeoff: If R is too large, the cost of initializing and checking the bins dominates, if it is too small, the method does not take advantage of potential gain available by subdividing into as many pieces as possible.

Should we do the largest subfiles last to avoid excessive recursion depth ? Probably not because recursion depth is limited by the length of the keys. Should we sort small subfiles with a simple method such as insertion sort? Certainly because there are huge numbers of them.

Below program uses auxiliary array of size equal to the size of the array to be sorted. Alternatively we could have used *in-place key-indexed counting*.



````c++
#define bin(A) l+count[A]
template <class Item>
void radixMSD(Item a[],int l, int r, int d)
	{ int i,j, count[R+1];
      static Item aux[maxN];
      if(d>bytesword) return;
      if(r-l <= M) { insertion(a,l,r); return;}
      for(j=0; j<R; j++) count[j]=0;
      for(i=l; i<=r; i++)
          count[digit(a[i],d)+1]++;
      for(j=l; j<R; j++)
          count[j]+= count[j-1];
      for(i=l; i<=r; i++)
          aux[l+count[digit(a[i],d)]++] = a[i];
      for(i=l; i<=r ; i++) a[i] = aux[i];
      radixMSD(a,l,bin(0)-1,d+1);
      for(j=0;j<R-1;j++)
          radixMSD(a,bin(j),bin(j+1)-1,d+1); //recursive call for each bin
	}
````

Another way to implement a MSD radix sorting is to use linked lists. We keep one linked list for each bin : On a first pass through the item to be sorted, we insert each item into appropriate linked list according to its leading digit value. Then we sort the sub list and stitch together all the linked lists to make a sorted whole.

