Link : https://leetcode.com/problems/design-a-stack-with-increment-operation/description/?envType=company&envId=salesforce&favoriteSlug=salesforce-all

Question : 

Design a stack that supports increment operations on its elements.

Implement the CustomStack class:

CustomStack(int maxSize) Initializes the object with maxSize which is the maximum number of elements in the stack.
void push(int x) Adds x to the top of the stack if the stack has not reached the maxSize.
int pop() Pops and returns the top of the stack or -1 if the stack is empty.
void inc(int k, int val) Increments the bottom k elements of the stack by val. If there are less than k elements in the stack, increment all the elements in the stack.
 

Example 1:

Input
["CustomStack","push","push","pop","push","push","push","increment","increment","pop","pop","pop","pop"]
[[3],[1],[2],[],[2],[3],[4],[5,100],[2,100],[],[],[],[]]
Output
[null,null,null,2,null,null,null,null,null,103,202,201,-1]
Explanation
CustomStack stk = new CustomStack(3); // Stack is Empty []
stk.push(1);                          // stack becomes [1]
stk.push(2);                          // stack becomes [1, 2]
stk.pop();                            // return 2 --> Return top of the stack 2, stack becomes [1]
stk.push(2);                          // stack becomes [1, 2]
stk.push(3);                          // stack becomes [1, 2, 3]
stk.push(4);                          // stack still [1, 2, 3], Do not add another elements as size is 4
stk.increment(5, 100);                // stack becomes [101, 102, 103]
stk.increment(2, 100);                // stack becomes [201, 202, 103]
stk.pop();                            // return 103 --> Return top of the stack 103, stack becomes [201, 202]
stk.pop();                            // return 202 --> Return top of the stack 202, stack becomes [201]
stk.pop();                            // return 201 --> Return top of the stack 201, stack becomes []
stk.pop();                            // return -1 --> Stack is empty return -1.
 

Constraints:

1 <= maxSize, x, k <= 1000
0 <= val <= 100
At most 1000 calls will be made to each method of increment, push and pop each separately.


Soln :- 

class CustomStack {

    int[] increment;
    Stack<Integer> st;
    int n ;
    int i = -1;

    public CustomStack(int maxSize) {
        n = maxSize;
        increment = new int[maxSize];
        Arrays.fill(increment , 0);
        st = new Stack<>();
    }
    
    void push(int x) {
        
        // normal push
        if(st.size() < n ) {
            st.push(x);
            i++;
        }
    }
    
    int pop() {
        
        if(st.size() == 0 ) {
            return -1;
        }
        int ans = st.pop() + increment[i];
        if(i > 0 ) {
            // lazy propagation
            increment[i - 1] += increment[i];
        }
        increment[i] = 0; // as element got popped    --------> this part of the code is very imp , read pdf to understand 
        // we dont want to keep the val that was added on it 
        i--;

        return ans;
    }
    
    void increment(int k, int val) {
        
        // case where if k is greater than stack size,
        // increment only the elements in the stack  
        int index = Math.min(k , st.size()) - 1;

        if(index >= 0 ) {
            increment[index] += val;
        }
        
    }
};

/**
 * Your CustomStack object will be instantiated and called as such:
 * CustomStack* obj = new CustomStack(maxSize);
 * obj->push(x);
 * int param_2 = obj->pop();
 * obj->increment(k,val);
 */
