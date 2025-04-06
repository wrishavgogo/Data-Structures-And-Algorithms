Link : https://leetcode.com/problems/maximum-frequency-stack/description/?envType=company&envId=uber&favoriteSlug=uber-three-months

Question : 

Design a stack-like data structure to push elements to the stack and pop the most frequent element from the stack.

Implement the FreqStack class:

FreqStack() constructs an empty frequency stack.
void push(int val) pushes an integer val onto the top of the stack.
int pop() removes and returns the most frequent element in the stack.
If there is a tie for the most frequent element, the element closest to the stack's top is removed and returned.
 

Example 1:

Input
["FreqStack", "push", "push", "push", "push", "push", "push", "pop", "pop", "pop", "pop"]
[[], [5], [7], [5], [7], [4], [5], [], [], [], []]
Output
[null, null, null, null, null, null, null, 5, 7, 5, 4]

Explanation
FreqStack freqStack = new FreqStack();
freqStack.push(5); // The stack is [5]
freqStack.push(7); // The stack is [5,7]
freqStack.push(5); // The stack is [5,7,5]
freqStack.push(7); // The stack is [5,7,5,7]
freqStack.push(4); // The stack is [5,7,5,7,4]
freqStack.push(5); // The stack is [5,7,5,7,4,5]
freqStack.pop();   // return 5, as 5 is the most frequent. The stack becomes [5,7,5,7,4].
freqStack.pop();   // return 7, as 5 and 7 is the most frequent, but 7 is closest to the top. The stack becomes [5,7,5,4].
freqStack.pop();   // return 5, as 5 is the most frequent. The stack becomes [5,7,4].
freqStack.pop();   // return 4, as 4, 5 and 7 is the most frequent, but 4 is closest to the top. The stack becomes [5,7].
 

Constraints:

0 <= val <= 109
At most 2 * 104 calls will be made to push and pop.
It is guaranteed that there will be at least one element in the stack before calling pop.


Soln : 

Target is O(1) push and pop , read the explanation pdf , to understand the wonderful explanation 
and how beautifully human brain works 


class FreqStack {

    Map<Integer , Integer> valToFrequencyMap;
    Map<Integer , Stack<Integer>> bucketToInsertedNodesMap;
    int maxFreq;

    public FreqStack() {
        valToFrequencyMap = new HashMap<>();
        bucketToInsertedNodesMap = new HashMap<>();
        maxFreq = 0;
    }
    
    public void push(int val) {
        
        valToFrequencyMap.put(val , valToFrequencyMap.getOrDefault(val , 0) + 1);
        int freq = valToFrequencyMap.get(val);

        // push this val into the bucket of new freq
        bucketToInsertedNodesMap.computeIfAbsent(freq , (Integer key) -> new Stack<>());
        bucketToInsertedNodesMap.get(freq).push(val);

        if(freq > maxFreq) {
            maxFreq = freq;
        }
    }
    
    public int pop() {
        
        // we will pop the maxFreq's top node

        int val = bucketToInsertedNodesMap.get(maxFreq).pop();
        valToFrequencyMap.put(val , valToFrequencyMap.getOrDefault(val , 1) - 1);

        if(valToFrequencyMap.get(val) == 0 ) {
            valToFrequencyMap.remove(val);
        }
        
        if(bucketToInsertedNodesMap.get(maxFreq).size() == 0 ) {
            bucketToInsertedNodesMap.remove(maxFreq);
            maxFreq--; // the maxFreq-1 th node of val will be in the previous bucket
        }

        return val;
    }
}

/**
 * Your FreqStack object will be instantiated and called as such:
 * FreqStack obj = new FreqStack();
 * obj.push(val);
 * int param_2 = obj.pop();
 */

