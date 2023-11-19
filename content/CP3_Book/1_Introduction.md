+++

Title = "Introduction"
weight = 1

+++

Competitive Programming is all about "*Given well-known Computer Science (CS) problems, solve them as quickly as possible!* ".

####  Tips to be Competitive

- Type Code Faster.

  - **55-65** wpm is decent speed.
- Quickly Identify Problem Types.
- Do Algorithm Analysis.

  - Anything of order of O(n log n) is accepted solutions in most cases.
- Master Programming Languages.

  - CPP is preferred but some library of Java can be useful for e. g. [Big Int](referances.md#euclidean-gcd)
- Master the Art of Testing Code.

  - Use `diff` in Unix to check for difference in i/o.
  - for multiple case input include same i/p again to check for avoiding reset variable errors.
  - Always check corner cases, large cases.
- Practice and More Practice.

#### Anatomy of a Programming Contest

- Background Story/problem description

  - for easy problem's story appear to be deceptive to make it hard.
  - hard problems are already hard but explained carefully.

- I/O Description.

- Samples for I/O.

  

#### Typical I/O Routines.

- \# of Test Cases Based i/o

  ![image-20200705091233940](/1_Introduction.assets/image-20200705091233940.png)

````c++
int TC,a,b;
scanf("%d",&TC);
while(TC--){
    scanf("%d %d",&a,&b);
    printf("%d\n",a+b);
}
````



- Variable # of i/o

  ![image-20200705091625628](/1_Introduction.assets/image-20200705091625628.png)

  ````c
  int k, ans v;
  while (scanf("%d",&k) != EOF){
  	ans=0;
      while(k--){ scanf("%d",&v); ans +=v}
      printf("%d\n", ans);
  }
  ````

  

  


#### The Ad Hoc Problems.

These are simply problems that can't be classified anywhere easily.

But a broad categorization is:

- Games
  - Cards, Chess, Others
- Palindromes and Anagrams
- Real Life Problems
- Time Library Based Problems
- **Time Waster Problem**

#### Some Useful Links

- [UHunt](https://uhunt.onlinejudge.org/)
- [Typing Test](https://www.typingtest.com/)

------




## Starred Problems UVa

#### Super Easy Problems (7-8 min max.)

**UVa 11172** - Relational Operators * (ad hoc, very easy, one liner) 

**UVa 11498** - Division of Nlogonia * (just use if-else statements) 

**UVa 11727** - Cost Cutting * (sort the 3 numbers and get the median)



#### Easy Problems

**UVa 10114** - Loansome Car Buyer * (just simulate the process) 

**UVa 11559** - Event Planning * (one linear pass) 

**UVa 11799** - Horror Dash * (one linear scan to find the max value)

#### Medium Level Problems

**UVa 00573** - The Snail * (simulation, beware of boundary cases!) 

**UVa 10141** - Request for Proposal * (solvable with one linear scan)

**UVa 11507** - Bender B. Rodriguez ... * (simulation, if-else)



#### Card & Chess Problems

**UVa 00462** - Bridge Hand Evaluator (simulation; card) 

**UVa 10646** - What is the Card? (shuffle cards with some rule and then get certain card) 

**UVa 12247** - Jollo (interesting card game; simple, but requires good logic to get all test cases correct) 

**UVa 00278** - Chess (ad hoc, chess, closed form formula exists) 

**UVa 00696** - How Many Knights (ad hoc, chess) 

**UVa 10284** - Chessboard in FEN (FEN = Forsyth-Edwards Notation is a standard notation for describing board positions in a chess game)

#### Palindromes and Anagrams

**UVa 00156** - Ananagram  (easier with algorithm::sort) 

**UVa 00195** - Anagram (easier with algorithm::next permutation) 

**UVa 00454** - Anagrams  (anagram) 

**UVa 00401** - Palindromes  (simple palindrome check)

**UVa 10945** - Mother Bear (palindrome)

**UVa 11221** - Magic Square Palindrome (we deal with a matrix)

#### Time

**UVa 00579** - Clock Hands (ad hoc, time)

**UVa 00893** - Y3K (use Java Gregorian Calendar; similar to UVa 11356)

**UVa 11947** - Cancer or Scorpio (easier with Java Gregorian Calendar)

