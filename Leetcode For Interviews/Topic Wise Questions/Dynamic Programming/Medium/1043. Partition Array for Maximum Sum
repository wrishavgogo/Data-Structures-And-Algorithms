Link  : https://leetcode.com/problems/partition-array-for-maximum-sum/description/?envType=problem-list-v2&envId=dynamic-programming

Question : 

Given an integer array arr, partition the array into (contiguous) subarrays of length at most k. After partitioning, each subarray has their values changed to become the maximum value of that subarray.

Return the largest sum of the given array after partitioning. Test cases are generated so that the answer fits in a 32-bit integer.

 

Example 1:

Input: arr = [1,15,7,9,2,5,10], k = 3
Output: 84
Explanation: arr becomes [15,15,15,9,10,10,10]
Example 2:

Input: arr = [1,4,1,5,7,3,6,1,9,9,3], k = 4
Output: 83
Example 3:

Input: arr = [1], k = 1
Output: 1
 

Constraints:

1 <= arr.length <= 500
0 <= arr[i] <= 109
1 <= k <= arr.length


Solns :  Explanation in comments 

class Solution {

    int[] cache = new int[500];

    public int maxSumAfterPartitioning(int[] arr, int k) {
        Arrays.fill(cache , -1);
        return f(0 , arr , k);
    }


    // given a startPosition , 
    // what is the maximum sum i can generate 
    // given the condition k 
    int f(int startPosition , int[] arr , int k  ) {

        int n = arr.length;

        if(startPosition >= n ) {
            // we have reached end of the array 
            return 0;
        }

        if(cache[startPosition] != -1 ) {
            return cache[startPosition];
        }

        int maxSum = 0;
        int maxi = 0;
        for(int i =  startPosition ; i < n ; i ++ ) {

            int subArrayLength = i - startPosition + 1 ;
            if(subArrayLength >  k ) {
                break;
            }

            maxi = Math.max(maxi , arr[i]);
            // maxi is the max in that subarry 
            // maxi*subArrayLength is all the elements changing into that maxi 
            // and from (i + 1 , to rest ) again try to find the max sum 
            maxSum = Math.max(maxSum , maxi*subArrayLength + f(i + 1 , arr , k));
        }

        cache[startPosition] = maxSum;

        return maxSum;
    }
}
