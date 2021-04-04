+++

title = "8-First Class ADTs"
weight = 8
+++

## First-Class ADTs

*A first class data type is one which we can use in our programs in the same way as we use built-in data types.*

***Complex numbers driver(roots of unity)***

````c++
#include <iostream.h>
#include <stdlib.h>
#include <math.h>
#include "COMPLEX.cxx"
int main(int argc, char *argv[]){
    int N = atoi(argv[1]);
    cout<< N << " complex roots of unity " << endl;
    for(int k =0; k <N; k++){
        float theta = 2.0*3.14159* k/N;
        Complex t(cos(theta), sin(theta)), x=t;
        cout<< K << ": " << t << " ";
        for(int j=0; j<N-1 ; j++) x*=t;
        cout<< x << endl;
    }
}
````

We want ADT that can perform algebra of complex numbers. For example adding two complex numbers by a certain rule and taking multiplication

***First-Class ADT interface for complex numbers***

```c++
class Complex{
    private:
    	//IMplementation-dependant code
    public:
    	Complex(float,float);
    	float Re() const;
    	float Im() const; Complex& operator*=(Complex&);
    	//Last part make availble *= operation for complex numbers
}
```

***First-class ADT for complex numbers***

Note that definition for the overloaded operator << doesn't format output.

````c++
#include <iostream>
class Complex{
    private:
    	float re,im;
    Public:
    	Complex(float x,float y)
        { re =x ; im =y;}
    	float Re() const
        { return re;}
    	float Im() const
        { return im;}
    	Complex& operator*= (const Complex& rhs)
        { 	float t = Re();
        	re = Re()*rhs.Re() - Im() * rhs.Im();
         	im = t*rhs.Im() + Im()*rhs.Re();
         	return *this;
        }
}
ostream& operator<<(ostream& t , const Complex& c)
{ t<<c.Re() << " " << c.Im(); return t;}
````

- any class with property that <u>none of its data member is a pointer</u> is a first-class data type in C++
  - when an object is copied,each member is copied;
  - when something is assigned into an object, each member is overwritten
  - when object goes out of scope its storage is reclaimed
  - system has default mechanisms for these situations
    - copy constructor
    - assignment operator
    - destructor
- when data member is a pointer
  - for assignment operation default copy constructor makes copy(*we won't be interested in that*)
- storage and copy constructor become crucial when using ADT's in software engineering.

## Queue Simulation