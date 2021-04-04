+++

title = "3-Dynamic Programming"

+++

## Dynamic Programming

An essential characteristic of divide-and-conquer algorithms was that they partition into independent subproblems. When the subproblems are not independent, the situation is more complicated and simple algorithms can take up a lot of time to run.

Sometimes these computation can take up exponential time.

Classic Example is Fibonacci Sequence whose recursive implementation requires more time and most in efficient thing is that we are calculating already calculated recursive call again and again wasting time.

By contrast, it is easy to compute using an array in linear time.

````c++
F[0] =0; F[1] =1;
for(i=2;i<= N; i++)
    F[i]=F[i-1]+F[i-2];
````

Numbers grow exponentially, so array size is small ,say $ f_{45} $ is largest Fibonacci number that can be represented as a 32- bit integer, so array of size 46 will do.

`A recurrence is a recursive function with integer values.` We can evaluate any such function values starting at the smallest, using previously computed values at each step. This technique is known as *bottom-up dynamic programming*.

*Top-down dynamic programming* is even simpler view of the technique that allows us to execute recursive functions at same cost( or less than) bottom-up approach.

**Top to down dynamic programming is often called Memoization**

***Fibonacci numbers(recursive implementation)***

````c++
int F(int i)
{
    if(i<1) return 0;
    if (i==1) return 1;
    return F(i-1) + F(i-2);
}
````

***Knapsack Problem***

- A thief robbing a safe finds it filled with N types of item of varying size and value, but has only a small knapsack of capacity M to use to carry the goods.

  The knapsack problem is to find the combination of item which the thief should choose for the knapsack in order to maximize the total value of all the stolen items.

- In recursive solution  to the knapsack problem, each time we choose an item and assume the we have already find the optimal way to solve. For the knapsack of size `cap`, we determine, for each item `i` among the available item types, what total value we could carry by placing `i` in the knapsack.

- But this is not a feasible solution as it runs in exponential time.

- We can apply top-down DP to eliminate all recomputation.

  ***Recursive Implementation***

  This code will run forever. Don't use it.

````c++
typedef struct { int size; int val;} Item;
int knap(int cap)
{	int i, space,max,t;
	for(i =0, max=0;i<N; i++){
        if((space = cap-item[i].size)>=0)
            if((t=knap(space)+items[i].val)> max)
                max=t;
        return max;
    }
}
````

***DP Implementation***

````c++
int knap(int M)
{
    int i, space,max, maxi=0,t;
    if(maxKnown[M] != unknown) return maxKnown[M];
	for(i =0, max=0;i<N; i++){
        if((space = cap-item[i].size)>=0)
            if((t=knap(space)+items[i].val)> max)
            {max=t; maxi=i;}

    maxKnown[M] = max; itemKnown[M] = items[maxi];
        return max;
    }
}
````



