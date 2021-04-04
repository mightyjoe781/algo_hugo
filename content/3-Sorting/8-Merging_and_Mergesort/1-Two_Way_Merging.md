+++

title = "1-Two Way Merging"

+++

### Two way Merging

given two ordered input file, we can combine them into one ordered file by simply keeping track of smallest element in each file and entering a loop where the smaller of the two elements that are smallest in their files are moved to output.

````c++
template <class Item>
void mergeAB(Item c[],Item a[],int N,Item b[], int M){
    for(int i = 0, j= 0,k=0;k<(M+N);k++){
        if(i == N){ c[k] = b[j++]; continue;}
        if(j == M){c[k] = a[i++]; continue;}
        c[k] = (a[i]<b[j])? a[i++] : b[j++];
    }
}
````

Time complexity : $O(n)$

