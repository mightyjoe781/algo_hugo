+++

title = "6-Strings"

+++

## Strings

- C++ inherits this data structure from C, and also includes strings as a higher-level abstraction in the STL

- Difference between string and an array of character revolves around *length*.

  - Both represents contiguous area of the memory, but the length of an array is set at the time that the array is created , whereas the length of a string may change during the execution of a program.

- *String Termination Character* - `'\0'`

- #### Elementary String-Processing  operations

  There are 2 approaches - *Pointer approach is compact* while *Indexed Array approach is natural to understand*

  - **Indexed Array Version**

    - *Compute string length* `(strlen(a))`

      `for ( i = 0; a[i] !=0 ; i++); return i;`

    - *Copy* `(strcpy(a,b))`

      `for ( i =0 ; (a[i] = b[i])!= 0 ; i++);`

    - *Compare* `(strcmp(a,b))`

      `for ( i=0 ; a[i] == b[i] ; i++)`

      ​		`if( a[i] ==0 ) return 0;`

      ​		`return a[i]- b[i] ;`

    - *Compare (prefix)* `(strncmp(a,b,n))`

       `for ( i=0 ; i<n && a[i]!=0 ; i++)`

      ​		`if( a[i] ==0 ) return a[i]-b[i];`

      ​		`return 0;`

    - *Append* `(strcat(a,b))`

      `strcpy(a+strlen(a) , b)`

  - **Equivalent pointer versions**

    - *Compute string length* `(strlen(a))`
      `b = a; while (*b++) ; return b-a-1;`

    - *Copy* `(strcpy(a, b))`
      `while (*a++ = *b++) ;`

    - Compare `(strcmp(a, b))`
      `while (*a++ == *b++)`

      ​		`if (*(a-1) == 0) return 0;`
      `return *(a-1) - *(b-1);`

- #### String Search

  ````c++
  #include <iostream.h>
  #include <string.h>
  static const int N = 10000;
  int main(int argc, char *argv[])
  	{ int i; char t;
  		char a[N], *p = argv[1];
  		for (i = 0; i < N-1; a[i] = t, i++)
  			if (!cin.get(t)) break;
  		a[i] = 0;
  		for (i = 0; a[i] != 0; i++)
  			{ int j;
  			for (j = 0; p[j] != 0; j++)
  				if (a[i+j] != p[j]) break;
  			if (p[j] == 0) cout << i << " ";
  			}
  		cout << endl;
  }
  ````

- Above code is correct but still code is bad since its run time is O(n^2^). Even searching a million words requires billion of instruction and is not efficient.

- This kind of error is called a *performance bug* because code can be verified to be correct, but it does not perform as effectively as we (implicitly) expect.

- Strings are actually pointer to chars. In some cases this realization can lead to compact code for string processing functions. For example code for string copy.