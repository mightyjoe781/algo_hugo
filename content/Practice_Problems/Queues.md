+++

title = "Queues"
date = 2021-04-15T00:01:25+05:30
weight = 7

+++

Application is usage of FIFO property.

There are 3 types of queues.

- Single Ended Queues 
- Double Ended Queues
- Priority Queues

[Implement Queue using stacks](https://leetcode.com/problems/implement-queue-using-stacks/) : Implementation problem using 2 stacks

O(n) push : Use two stacks with invariant , with s1 contains elements in LIFO order , s2 is empty

O(n) pop : Transfer all s1 content to s2 and remove the top (first) then retransfer to s1

O(1) empty: check s1.empty();

<u>Improvements</u>

Push : push always in s1

Pop : check S2 is empty ? transfer everything from s1 to s2, pop from s2 : pop from s2

Peek : just don't pop otherwise same as Pop

Empty : both s1 and s2 are empty

[Design a Circular Queue](https://leetcode.com/problems/design-circular-queue/) : Use array implementation

Declare array of size k , and maintain 2 pointer marking first and last pointer.

enqueue : first will stay while last will keep moving ahead.

dequeue  : delete first and move it ahead

````c++
class MyCircularQueue {
public:
    vector<int> c_queue;
    int sz;
    int first; // first points to the next position after the front.
    int last;  // last points to the next position  last occupied / filled position.
    int k;
    MyCircularQueue(int k) {
       c_queue.resize(k);
        sz = 0;
        first = last = 0;
        this->k = k;
    }
    bool enQueue(int value) {
        if(isFull())
            return false;
        c_queue[last] = value;
        last = (last+1)%k;
        sz++;
        return true;
    }
    bool deQueue() {
       if(isEmpty())
           return false;
        first = (first+1)%k;
        sz--;
        return true;
    }
    int Front() {
        if(isEmpty())
            return -1;
        return c_queue[first]; 
    }
    int Rear() {
       if(isEmpty())
           return -1;
        return c_queue[(last-1+k)%k];
    }
    bool isEmpty() {
        return sz == 0; 
    }
    bool isFull() {
        return sz == k;
    }
};
````

[Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/) 

We can use heap to maintain k window.

But we want to use Monotonic Queue , what if we keep pushing the running max in the queue.

use same concept as in monotonic stack : here constraint queue always contains increasing queue

keep processing the array windows and put the item on the queue now if item is greater then the item at front of queue we keep popping it until our max is at head of queue.

````c++
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        // keep the index
        deque<int> dq;
        int active_window = 0;
        int i, n = nums.size();
        vector<int> res;
        for(i=0 ;i < n; i++) {
     	// remove smaller numbers from the back as long its not empty
            while(!dq.empty() && nums[dq.back()] <= nums[i])
                dq.pop_back();
            
            // remove the satle indices from front
            while(!dq.empty() && dq.front() < active_window)
                dq.pop_front();
            dq.push_back(i);
            // printing when we have enough items for window
            if( i >= k-1 ){
                res.push_back(nums[dq.front()]);
                active_window++;
            }
                
        }
        return res;
            
    }
````





