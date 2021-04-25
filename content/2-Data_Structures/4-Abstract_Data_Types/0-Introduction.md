+++

title = "0-Introduction"

weight = -1

+++

## Introduction
- An ***abstract data type*** (ADT) is a data type (a set of values and a collection of operations on those values) that is accessed only through an *interface*. We refer to a program that uses an ADT as a *client*, and a program that specifies the data type as an *implementation*.

- We say that the interface is *opaque*: the client cannot see the implementation
  through the interface.

- **Point Class Implementation**

  - ***Class*** defines a data type consisting of the set of values "pair of  floating point numbers" along with *two* member functions that are defined for all POINTs.

    - *function POINT()* is constructor that initializes the co-ordinates to a random number.
    - *function distance(POINT)* computes the distance to another point.

    ````c++
    #include <math.h>
    class POINT
    {private:
    	float x,y;
     public:
     	POINT()
        { x=1.0 * rand() / RAND_MAX;
          y=1.0 * rand() / RAND_MAX;}
     	float distance(POINT a)
        {float dx = x-a.x,dy=y-a.y;
        return sqrt(dx*dx + dy *dy);}
    };
    ````

  - data representation is private and only can be  modified using member functions, and the member functions are public and can be used by any client.

  - `x,y` are often referred as data members of the class.

  - Client Program can call `p.distance(q)` to compute distances.

- *Structures* can have associated function is C++, too(Not in C)

  - Difference between structure and class lies in access to information using `private` and `public`.
  - So in above program we cannot access `a.x` and `a.y` as they are private.
  - But member function have direct access so we can access them using member function in a limited manner
  - We can say `x` in `x-a.x` belongs to `p` and `a.x` belongs to `q`. To remove ambiguity we can alternatively write `dx = this->x - a.x`

- ***static*** keyword attached to data members ensures that there is only one copy of that variable.

  - So in above implementation we attached function to object rather we should take two points as arguments and create a static member function

    ```c++
    static float distance(POINT a, POINT b)
    {
        float dx = a.x - b.y, dy = a.y - b.y;
        return sqrt(dx*dx + dy*dy);
    }
    ```

  - A ***static member function*** is one that may access members of a class, but does not need to be invoked for a particular object.

- Another Approach can be defining function as free function outside the declaration of class POINT.

  - But this version needs to access private data members of POINT, so we should make it friend of POINT class.

    ````c++
    friend float distance(POINT, POINT);
    ````

  - A friend is a function that, while not a member of a class, has access to its private members.

- for Client to read the data values we want to provide basic functions to provide reading facility.

  ````c++
  float X() const { return x; } //const so that they are not modified
  float Y() const { return y; }
  ````

- For many application our main aim to build a class according to needs of application. Sometime we want to use this class in same way like built in types.

- then *Operator Overloading* is a good way

  For example, suppose that we wanted to consider two points as identical if the distance between them is less than .001. By adding the code

  ````c++
  friend int operator==(POINT a, POINT b)
  { return distance(a, b) < .001; }
  ````

  Now this gives us ability to check equality of two Points using `==`

- Other examples are 

  - `<<` Operator from class `ostream`

  - But this operator can't print POINT class we may need to overload it.

    ````c++
    ostream& operator<<(ostream& t, POINT p)
    {
        cout << "(" << p.X() << "," << p.Y() << ")";
        return t;
    }
    ````

  - One thing to notice is that it is neither a member function, nor a friend but it uses public member functions `X()` and `Y()` to access the data.

- How does the C++ class concepts relate to the client-interface-implementation paradigm??

  - We use a convention

    - *the public function declaration* in class comprise its interface
      - We data hidden from client programs
      - Client can only know about public member function

    Why we did this all ??

    Because we can now test programs freely without changing the client programs at all.

- ***POINT ADT interface***

  ADT implementation done by removing private parts and replacing function implementation by declaration. because for many application ability to change implementation is required.

  ````c++
  class POINT
  	{
  	private:
  		// Implementation-dependent code
  	public:
  		POINT();
      	float distance(POINT) const;
  };
  ````

  