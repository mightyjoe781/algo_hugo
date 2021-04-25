+++

title = "2-Divide and Conquer"

+++

## Divide and Conquer

***Divide and Conquer to find the maximum***

- function divides array into two halves and then finds the maximum

````c++
Item max(Item a[],int l, int r){
    if (l==r) return a[l];
    item m = (l+r)/2;
    Item u = max(a,l,m);
    Item v = max(a,m+1,r);
    if(u>v) return u; else return v;
}
````

- *Property* - A recursive function that divides a problem of size N into two independent (non empty) parts that it solves recursively calls itself less than N times.

- If parts are one of size k and one size N-k

  $$ T_N = T_k + T_{N-k} + 1, for N \ge 1 \text{ with }T_1 =0 $$

  Solution- $ T_N= N-1$  is immediate by induction.


***Towers of Hanoi***

given 3 pegs and N disks that fit onto the pegs.  Disks differ in size and are initially arranged on one of the pegs, in order from largest(disk N) at the bottom to smallest (disk1) at the top.

The task is to move the stack of disks to the right one position(peg), while obeying the following rules

1. only one disk mat be shifted at a time
2. no disk may be places on top of a smaller one

*The recursive divide-and-conquer algorithm for the towers of Hanoi problem produces a solution that has 2^N^ -1 moves.*

$ T_N = 2 T_{N-1} + 1, \text{ for } N\ge 2 \text{ with }T_1 =1 $

***Solution to the towers of Hanoi problem***

````c++
void hanoi(int N,int d){
    if(N==0) return;
    hanoi(N-1,-d);
    shift(N,d);
    hanoi(N-1,-d);
}
````

there is a  correspondence with n-bit numbers is a simple algorithm for the task. We can move the pile one peg to right by iterating the following two steps until done:

1. Move the small disk to right if n is odd (left if n is even)
2. Make the only legal move not involving the small disk.

- Some important search and sort algorithms analysis

  ![image-20200723093046145](/2-Divide_and_Conquer.assets/image-20200723093046145.png)

