+++

title = "3-Examples of Stack-ADT Clients"
weight = 3
+++

## Examples

**Postfix and Infix order**

- Any general arithmetic expression can be written using brackets.

  $ 5 * (((9+8) * (4*6)) + 7 ) $

  Above expression is called infix order and its Postfix version is

   $  5 9 8 + 4 6 * * 7 + * $

- The reverse of postfix is called as prefix, or *Polish Notation*

- We can use stacks to achieve this : -

  - Moving from left to right, we interpret each operand as the command to "push operand onto in stack" and each operator as the commands  to "*pop the two operands from the stack*"

    ````c++
    #include <iostream.h>
    #include <string.h>
    #include "STACK.cxx"

    int main(int argc,  char *argv[])
    {
        char *a = argv[1]; int N = strlen(a);
        STACK<int> save(N);
        for (int i=0;i<N;i++)
        {
        	if(a[i]== '+')
                save.push(save.pop()+save.pop());
            if(a[i] == '*')
                save.push(save.pop()*save.pop());
            if ((a[i] >= '0') && (a[i] <=[']9[']))
                save.push(0);
            while((a[i] >= '0') && (a[i] <=[']9[']))
                save.push(10 * save.pop()+ (a[i++]-'0'));
        }
        cout<< save.pop() << endl;
    }
    ````

  - final if and while loops perform a calculation similar to C++ `atoi` function and converts integers from ASCII strings to integers for calculation.

  - There is a language that works in similar manner called as POSTSCRIPT

- Converting a `Infix` to `Postfix`

  ````c++
  #include <iostream.h>
  #include <string.h>
  #include "STACK.h"
  int main(int argc, char *argv[])
      { char *a = argv[1]; int N = strlen(a);
          STACK<char> ops(N);
          for (int i = 0; i < N; i++)
          	{
         		 if (a[i] == ')')
          		cout << ops.pop() << " ";
          	 if ((a[i] == '+') || (a[i] == '*'))
         			ops.push(a[i]);
         		 if ((a[i] >= '0') && (a[i] <= '9'))
          		cout << a[i] << " ";
      }
      cout << endl;
  }
  ````

