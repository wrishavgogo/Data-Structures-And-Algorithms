Link : https://leetcode.com/problems/find-median-from-data-stream/description/?envType=company&envId=amazon&favoriteSlug=amazon-three-months

Question : 

The median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value, and the median is the mean of the two middle values.

For example, for arr = [2,3,4], the median is 3.
For example, for arr = [2,3], the median is (2 + 3) / 2 = 2.5.
Implement the MedianFinder class:

MedianFinder() initializes the MedianFinder object.
void addNum(int num) adds the integer num from the data stream to the data structure.
double findMedian() returns the median of all elements so far. Answers within 10-5 of the actual answer will be accepted.
 

Example 1:

Input
["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
[[], [1], [2], [], [3], []]
Output
[null, null, null, 1.5, null, 2.0]

Explanation
MedianFinder medianFinder = new MedianFinder();
medianFinder.addNum(1);    // arr = [1]
medianFinder.addNum(2);    // arr = [1, 2]
medianFinder.findMedian(); // return 1.5 (i.e., (1 + 2) / 2)
medianFinder.addNum(3);    // arr[1, 2, 3]
medianFinder.findMedian(); // return 2.0
 

Constraints:

-105 <= num <= 105
There will be at least one element in the data structure before calling findMedian.
At most 5 * 104 calls will be made to addNum and findMedian.
 

Follow up:

If all integer numbers from the stream are in the range [0, 100], how would you optimize your solution?
If 99% of all integer numbers from the stream are in the range [0, 100], how would you optimize your solution?


Answer : 

class MedianFinder {


    // use two heaps to maintain sorted order in the stream 
    // leftHeap will be a maxHeap -> [1 , 2 .. ... . .maxInLeft]
    // rightHeap will be a minHeap -> [minElement , ....... maxIn The steam]

    /***
        based on the new element which comes 
        int currSize = leftHeap.size() + rightHeap.size();

        if(currSize % 2 == 0 ) {
            // we push data into the left heap 
            as we want to maintain leftHeap.size() > rightHeap.size() by 1 
        }
        else {
            push entry in the right heap 
            because left heap will be greater in size than right heap 
            by a size of 1 
            , 
        }

    **/

    // maxHeap
    PriorityQueue<Integer> leftHeap;
    // minHeap
    PriorityQueue<Integer> rightHeap;

    public MedianFinder() {
        
        leftHeap = new PriorityQueue<Integer>((Integer a , Integer b) -> {
            return b - a;
        });

        rightHeap = new PriorityQueue<Integer>((Integer a , Integer b) -> {
            return a - b;
        });
    }
    
    public void addNum(int num) {
        
        int sz = leftHeap.size() + rightHeap.size();

        if(leftHeap.size() == 0 ) {
            // it means no element is inserted yet 
            // becuz we are going to maintain leftHeap.size() == rightHeap.size() in even size
            // leftHeap.size() == rightHeap.size() + 1 in odd size
            leftHeap.offer(num);
            return;
        }

        if(sz % 2 == 0 ) {
            
            // deciding where this num would go 

            if(rightHeap.peek() >= num ) {
                leftHeap.offer(num);
            } else {
                leftHeap.offer(rightHeap.poll());
                rightHeap.offer(num);
            }

            
        } else {

            // sz if odd currently 
            // so left has 1 element more than right ,, right now 
            // decide where the num is going to go
            if(num < leftHeap.peek()) {
                rightHeap.offer(leftHeap.poll());
                leftHeap.offer(num);
            } else {
                rightHeap.offer(num);
            }
        }

    }
    
    public double findMedian() {
        
        int sz = leftHeap.size() + rightHeap.size();

        if(sz % 2 == 1 ) {
            return (double)(leftHeap.peek());
        }
        
        double left = leftHeap.peek();
        double right = rightHeap.peek();

        double ans = (left + right) / (2.0);

        return ans;
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
