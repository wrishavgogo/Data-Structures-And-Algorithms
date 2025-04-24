Link : https://leetcode.com/problems/find-peak-element/description/

Prerequisite for this question :- https://github.com/wrishavgogo/Data-Structures-And-Algorithms/tree/main/Leetcode%20For%20Interviews/Company%20Wise/Google/Medium/Revise%20852.%20Peak%20Index%20in%20a%20Mountain%20Array

Question : 

A peak element is an element that is strictly greater than its neighbors.

Given a 0-indexed integer array nums, find a peak element, and return its index. If the array contains multiple peaks, return the index to any of the peaks.

You may imagine that nums[-1] = nums[n] = -∞. In other words, an element is always considered to be strictly greater than a neighbor that is outside the array.

You must write an algorithm that runs in O(log n) time.

 

Example 1:

Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return the index number 2.
Example 2:

Input: nums = [1,2,1,3,5,6,4]
Output: 5
Explanation: Your function can return either index number 1 where the peak element is 2, or index number 5 where the peak element is 6.
 

Constraints:

1 <= nums.length <= 1000
-231 <= nums[i] <= 231 - 1
nums[i] != nums[i + 1] for all valid i.



Soln : 

class Solution {
    public int findPeakElement(int[] nums) {
        

        int n = nums.length;

        if(n == 1 ) {
            // peak element itself 
            return 0;
        }

        if(isPeak(n - 1  , nums)) {
            return n - 1;
        }

        if(isPeak(0 , nums)) {
            return 0;
        }

        

        int lo = 0 ;
        int hi = n - 1;

        while(lo <= hi ) {

            int mid = (lo + hi) /2;

            if(isPeak(mid , nums)) {
                return mid;
            }

            if(mid == n - 1 ) {
                // we checked above only to see if n - 1 can be peak or not 
                hi = mid  - 1;
                continue;
            } 

            if(mid == 0 ) {
                // we checked above only to see if 0 can be peak or not 
                lo = mid + 1;
                continue;
            }


            if(nums[mid] > nums[mid + 1]) {
                // decreasing half
                hi = mid - 1;
            } else {
                // increasing half and approaching peak 
                lo = mid + 1;
            }
        }

        return -1;
    }

    public boolean isPeak(int mid ,int[] nums ) {

        int n = nums.length;

        long val = nums[mid];
        long leftVal = mid - 1 >= 0 ? nums[mid - 1] : val - 1 ;
        long rightVal = mid + 1 < n ? nums[mid + 1] : val - 1 ;

        return (val > leftVal) && (val > rightVal) ;
    }

    

    
}
