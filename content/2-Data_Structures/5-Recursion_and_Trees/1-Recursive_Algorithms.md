+++

title = "1-Recursive Algorithms"

+++

## Recursive Algorithms

Concept of recursion is Fundamental in mathematics. A recursion program is one that calls itself with a *termination condition*.

- *Trees* are data structures which are recursively defined.
- **Definition-** A *recursive algorithm* is one that solves a program by solving one or more smaller instances of the same problem.

***Factorial function (recursive implementation***

````c++
int factorial(int N){
    if (N == 0) return 1;
    return N*factorial(N-1);
}
````

- <u>Its always possible to convert a recursive program into a non-recursive one.</u>

- A care should be taken while writing programs related to recursion is that
  - they must explicitly solve a basis case
  - Each recursive call must involve smaller values of the arguments

***A questionable recursive program***

````c++
int puzzle(int N)
{
    if (N == 1) return 1;
    if (N % 2 == 0)
        return puzzle(N/2);
    else return puzzle(3 * (N+1));
}
````

We can clearly see there is no bound on N;

***Euclid's algorithm***

````c++
int gcd(int m,int n ){
    if(n==0) return m;
    return gcd(n,m%n);
}
````

***Recursive program to evaluate prefix expressions***

````c++
char *a; int i;
int eval(){
    int x=0;
    while(a[i] == " ") i++;
    if(a[i] == "+")
    {	i++; return eval() + eval();}
    if(a[i] == "*")
    {	i++; return eval() * eval();}
    while((a[i]>="0")&&( a[i]<="9"))
        x=10*x+(a[i++]-'0');
    return x;
}
````

***Examples of recursive functions for linked lists***

- ​	`count` - It counts number of nodes on the list.
- `traverse` - calls `visit` for each node on the list from beginning to end.
- `traverseR` - It calls `visit` for every node but in reverse order.
- `remove` - Removes all nodes from a given item value from the list.

````c++
int count(link x)
{
    if(x==0) return 0;
    return 1 + count(x->next);
}
void traverse(link h, void visit(link)){
    if(h==0) return ;
    visit(h);
    traverse(h->next,visit);
}
void traverseR(link h, void visit(link)){
    if(h==0) return;
    traverseR(h->next,visit);
    visit(h);
}
void remove(link& x,Item v)
{
    while(x!=0 && x->item == v)
    {	link t = x ; x = x->next ; delete t;}
    if (x!=0) remove(x->next,v);
}
````

