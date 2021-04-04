+++

title = "9-Application Based ADTs"
weight = 9
+++

## Application-Based ADT Example

*We drive polynomial ADT from symbolic mathematics*

*aim is to be solve*

$ \left(  1-x+\frac{x^2}{2}-\frac{x^3}{6} \right) = 1 + \frac{x^2}{2} +\frac{x^3}{3} -\frac{2x^4}{3}+\frac{x^5}{3}+...$

we also want to evaluate this polynomial at some specific value of x

***Polynomial Client(Binomial Coefficients)***

````c++
#include <iostream.h>
#include <stdlib.h>
#inlcude "POLY.cxx"
int mian(int argc , char *argv[]){
    int N= atoi(argv[1]); float p = atof(argv[2]);
    cout<< "Binomial coefficients " <<endl;
    POLY<int> x(1,1), one(1,0), t = x + one, y=t;
    for (int i=0; i<N; i++){
        y = y*t; cout<<y << endl;
    }
    cout<< y.eval(p) << endl;
}
````

***ADT interface for polynomials***

````c++
template <class Number>
class POLY
{
    private:
        // Implementation-dependent code
    public:
        POLY<Number>(Number, int);
        float eval(float) const;
        friend POLY operator+(POLY &, POLY &);
        friend POLY operator*(POLY &, POLY &);
};
````

***Array implementation of polynomial ADT***

````c++
template <class Number>
class POLY
    {
    private:
        int n; Number *a;
    public:
        POLY<Number>(Number c, int N)
            { a = new Number[N+1]; n = N+1; a[N] = c;
            for (int i = 0; i < N; i++) a[i] = 0;
            }
        float eval(float x) const
            { 	double t = 0.0;
            	for (int i = n-1; i >= 0; i--)
            		t = t*x + a[i];
            	return t;
            }
        friend POLY operator+(POLY &p, POLY &q)
            { POLY t(0, p.n>q.n ? p.n-1 : q.n-1);
             	for (int i = 0; i < p.n; i++)
            		t.a[i] += p.a[i];
            	for (int j = 0; j < q.n; j++) t.a[j] += q.a[j];
            	return t;
            }
        friend POLY operator*(POLY &p, POLY &q)
            { 	POLY t(0, (p.n-1)+(q.n-1));
            	for (int i = 0; i < p.n; i++)
                	for (int j = 0; j < q.n; j++)
                		t.a[i+j] += p.a[i]*q.a[j];
            		return t;
            }
};
````

- one  thing to notice we redefined the arithmetic operators by using overloading.
- and this method is based on Horner's algorithm because naive approach takes N^2^ time. Another approach to store the coefficients in different array costs extra space
- Other thing to notice is that if multiplication on two polynomial takes quadratic time later we will see we can improve it to $ N\log N $ using *FFT*.