+++

title = "1-Abstract Objects & Collections of Objects"
weight = 1
+++

## Abstract Objects and Collections of Objects
- Up till now we have created a lot of simple data types in order to write code that doesn't depend on object types and deciding the datatype using `typedef`

- Above approach allows to use same code for integers and floating-points and with pointers, object type can be arbitrarily complex. Moreover implementation is not also hidden from clients. ADT's are quite useful in this sense

- Later we will develop ADT's for generic data objects of a generic type Item so that we can write client programs where we use Item in same way as built-in types.

- Once developed we can use the *C++ Template* mechanism to write code that is generic with respect to types.

  For example exchange operation for generic items.

  ````c++
  template <class Item>
  	void exch(Item &x, Item &y)
  	{ Item t = x; x = y; y = t; }
  ````

  - Other operation
    - insert a new object into the collection.
    - remove an object from collection.
  - Other operations like *destructors* and *copy constructor*

- In C++, classes that implement collections of abstract objects are called `container classes.`