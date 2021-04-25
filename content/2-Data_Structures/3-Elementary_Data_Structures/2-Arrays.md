+++

title = "2-Arrays"

+++

## Arrays

- Most primitive and fundamental data structures.

- An array is a fixed collection of same-type data that are stored contiguously and that are accessible by an index.

- One of the distinctive features of C++ is that an array name generates a pointer to the first element of the array (the one with index 0). So `*(a+i)`is same as `a[i]`.

- ### Sieve of Eratosthenes (nlogn)

  ````c++
  #include <iostream>
  using namespace std;
  static const int N = 1000;
  int main(){
      int i, a[N];
      for(int i=2;i<N;i++) a[i]=1; //sets entire array
      for(int i=2;i<N;i++) 
          if(a[i])	//takes jumps of j to remove every multiple
              for(int j=i;j*i<N;j++) a[i*j]=0;
      for(i=2;i<N;i++)		//printing utility
          if(a[i]) cout <<" "<<i;
      cout<<endl;
  }
  ````

- So we can pass a pointer to array in function to avoid creation of copy of array.

- In above program we can take input two ways

  #### Dynamic Memory Allocation

  - Using command line arguments

    ````c++
    int main(int argc, char *argv[])
    { int i, N = atoi(argv[1]);
    int *a = new int[N];
    ````

    here `new[]`, an operator that allocates the amount of memory that we need for our array at execution time and returns, for our exclusive use, a pointer to array.

  - Using `cin`

- C++ **STL** library provides *Vector*, an abstract object that we can index like an array (with optional automatic out-of-bounds checks),but can grow and shrink.

  ------

  ## Some Codes

  ### Coin Flipping Simulation$ O(n^2)$[can be improved to linear]

  If we flip a coin N times, we expect to get N/2 heads, but could get anywhere from 0 to N heads. This program runs the experiment M times, taking both N and M from the command line. It uses an array f to keep track of the frequency of occurrence of the outcome "i heads" for 0<= i <= N, then prints out a histogram of the result of the experiments, with one asterisk for each 10 occurrences.
  The operation on which this program is based—indexing an array with a computed value—is critical to the efficiency of many computational procedures.

  ````c++
  #include <iostream>
  #include <stdlib>
  int heads(){
      return rand() < RAND_MAX/2;
  }
  int main(int argc , char *argv[]){
      int i , j, cnt;
      int N = atoi(argv[1]),M = atoi(argv[2]);
      int *f = new int[N+1];
      for(j=0; j<=N; j++) f[j]=0;
      for(i=0; i<M; i++,f[cnt]++)
          for (cnt=0,j=0; j<=N ; j++)
              if( heads())cnt++;
      for(j=0;j<=N; j++)
      {
          if (f[j]==0) cout<<".";
          for(i =0;i<f[j];i+=10) cout<< "*";
          cout<<endl;
      }
  }
  ````

  ### Closest-Point computation

  This program illustrates the use of an array of structures, and is representative of the typical situation where we save items in an array to process them later, during some computation.
  It counts the number of pairs of N randomly generated points in the unit square that can be connected by a straight line of length less than d, The running time is $O(N^2)$, so this program cannot be used for huge N. Same with this as it can also be improved.

  ````c++
  #include <iostream.h>
  #include <math.h>
  #include <stdlib.h>
  #include "Point.h"
  float randFloat()
  {return 1.0 * rand()/RAND_MAX;}
  int main(int argc, char *argv[]){
      float d = atof(argv[2]);
      int i,cnt =0, N =atoi(argv[1]);
      point *a = new point[N]; //array of structs
      for( i =0; i<N;i++)
      { a[i].x =randFloat(); a[i].y=randFloat();}
      for(i =0; i<N;i++)
          for(int j=i+1;j<N;j++)
              if(distance(a[i],a[j])<d) cnt++;
      cout<< cnt << " pairs within " << d << endl;
      }
  ````

  