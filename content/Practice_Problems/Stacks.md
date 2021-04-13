+++

title = "Stacks"
date = 2021-04-13T12:01:25+05:30
weight = 6

+++

##### Review :

Dynamic Data Structure , O(1) insert, remove latest insertion and lookup top.

Application for practice.

- Expression Evaluation 
- Monotonic Stack
- Generic Problems

USP (Unique Selling Point xD) of stack.

- LIFO property

#### Problems on Implementations and Designs

[Min Stack](https://leetcode.com/problems/min-stack/) : Implementation problem. Use 2 stack. One for keeping track of stack ops and one stack for maintaining minimum so far.

[Custom Stack](https://leetcode.com/problems/design-a-stack-with-increment-operation/) : Use a vector to implement stack and desired function. (gives AC) (naïve approach not desired).

Interesting Implementation :

lazy increment take O(k) to implement but remember function doesn't return anything and doing this operation can be costly if done frequently.

Don't increment at the time increment is called rather while popping ensure its popped with correct value.

Keep the stack with pair `<int , inc >` now while incrementing just to the change at the kth element. and while popping keep transferring the previous increments all the way to the bottom of stack.

this reduces the cost to O(1)

````c++
class CustomStack {
public:
    vector<int> custom_stack_; // keeps track of stack
    vector<int> inc_val_;	   // keeps track of increment value
    int size_;
    CustomStack(int maxSize) {
        size_ = maxSize;
    }
    
    void push(int x) {
        if(custom_stack_.size() == size_)
            return;
        custom_stack_.push_back(x);
        inc_val_.push_back(0);
    }
    
    int pop() {
        if(custom_stack_.empty())
            return -1;
        
        int last = custom_stack_.back() + inc_val_.back();
        // transfer before removing
        if(custom_stack_.size() > 1){ 
            inc_val_[inc_val_.size()-2] += inc_val_.back();
        }
        custom_stack_.pop_back();
        inc_val_.pop_back();
        
        return last;
    }
    
    void increment(int k, int val) {
        int n = custom_stack_.size();
        if(n == 0) return; // edge case
        inc_val_[min(n,k)-1] += val; // adding on increments.
    }
};
````

[Maximum Frequency](https://leetcode.com/problems/maximum-frequency-stack/) : A useful and more precise approach would be to maintain a map of frequency which is of type stack.

so for freq 1 we have a stack

​      for freq 2 we have a stack

if we want to insert say `push(5)` we insert in freq1 stack once and push once in freq2 stack

`push(5), push(7) ,push(5)`

​	1 --	|	| => stack => 5,7

​	2 --	|	| => stack => 5

Now its quite easy to infer 5 is most frequent and we have to pop it. 

It solves two problems : popping the max frequency as well in the case of tie we pop most recent insertion.

We will need another integer DAT for storing frequency.

````c++
class FreqStack{
    public:
    int max_freq;
    unordered_map<int,int> counter; // DAT 
    unordered_map<int,stack<int>> rev_freq;// the table shown in digram

    FreqStack(){
    }
    void push(int x){
        //increase the counter
        counter[x]++;
        int count = counter[x];
        rev_freq[count].push(x);
        // update the max_freq
        max_freq = max(max_freq,count);
    }
    int pop(){
        //remove the most frequent
        int last = rev_freq[max_freq].top();
        
        rev_freq[max_freq].pop();
        //update the count
        counter[last]--;
        //update the max_freq
        if(rev_freq[max_freq].empty()){
            max_freq--;
        }
        return last;
    }
};

````

#### Problems on Expression Evaluation ( Monotonic Stacks)

_fix is defined on operator

`a b +` Postfix

`a + b` Infix

`+ a b` Prefix

[Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/) : Simple standard question.



