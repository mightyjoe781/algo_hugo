+++

title = "1-Building Blocks"

+++


- Choice of Algorithm and Data Structure are closely intertwined.
- A Data Structure is not a passive object.(We must consider Ops to be performed on them).

## Building Blocks

- *Types* define how we will use particular set of bits.

- *Functions* allow us to specify operation that will be performed on data.

- *C++ Structures* to group heterogenous data together.

- *Pointers* to refer to information indirectly.

- Basic Data types are

  - Integers (**int**S)
  - Floating-point Numbers (**float**S)
  - Characters (**char**S)

- We can trade space for accuracy and **use int , long int, short int** for integers and from **float, double** for floating-point numbers.

- Ops are associated with types not other way around. Keep track of whether operand and result are of correct type. In some situation C++ performs *implicit type conversion* in other cases we perform casts.

- Functions have a list of arguments and possibly a return type. First line includes the library which contains cout and endl.

  ````c++
  #include <iostream.h>
  int lg(int);
  int main(){
      for( int N =1000;N<=1000000;N*=10)
          cout<<lg(N) <<" "<<N<<endl;
  }
  int lg(int N){
      for(int i=0;N>0;i++,N/=2);
      return i;
  }
  ````

- By not specifying the type of the numbers that the program processes, we extend its potential utility, portability.

- Best Practice is to split file in 3 files.

  - **Interface**-defines the data structure and declares the function to be used to manipulate the data structure
  - **Implementation** of function declared in the interface.
  - **Client Program** that uses the functions declared in the interface to work at a higher level of abstraction.

- Reusing function names or operators in different data types is called **overloading.**

- Simplest mechanism to group data in C++ is arrays and structures.

- Structures are aggregate types that we use to define collections of data such that we can manipulate an entire collection as a unit, but still can refer back to components of a given datum by name.

  ##### interface for pairs of floating point numbers

  ````c++
  struct point {float x; float y;};
  float distance(point,point);
  ````

  so we can now declare the variables using this new data type.

  `struct point a,b;` declared two variables of this type.

  also note that we can pass this data type as arguments to the functions.

  `a.x = 1.0; a.y = 1.0` This can be considered as initialization of variables.

  ##### implementation

  ````C++
  #include <math.h>
  #include "Point.h" //interface file
  float distance(point a,point b)
  {
      float dx = a.x - b.x , dy = a.y-b.y;
      return sqrt(dx*dx+dy*dy);
  }
  ````

- Now pointer gives us ability to manipulate our data indirectly. <u>A pointer is a reference to an object in memory</u>.

- `int *a`  Here variable a is a pointer to integer.(Declaration)

-  `*a` refers to that integer. (Accessing)

- Unary Operator `&` gives machine address of an object and useful in initializing pointers. For e. g. `*&a` is same as `a`.

  ````c++
  polar(float x, float y, float *r, float *theta)
  { *r = sqrt(x*x + y*y); *theta = atan2(y, x); }
  ````

  This snippet actually creates a function that return multiple values using pointers with calling function as-

  `polar(1.0, 1.0, &a, &b)` //note arguments are **passed by value.**

- Same effect can be achieved by **reference parameters**

  ````c++
  polar(float x, float y, float& r, float& theta)
  { r = sqrt(x*x + y*y); theta = atan2(y, x); }
  ````

  - Notation `float&` means "reference to a `float`".
  - We may think of reference as built in pointers that are automatically followed.
  - Now calling function `polar(1.0, 1.0, a ,b)` will set a = 1.4142... and b =0.7853....