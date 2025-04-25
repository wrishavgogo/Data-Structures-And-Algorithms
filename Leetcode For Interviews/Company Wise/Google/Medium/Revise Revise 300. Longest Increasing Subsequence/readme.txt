Link : https://leetcode.com/problems/longest-increasing-subsequence/description/

Question : 

Given an integer array nums, return the length of the longest strictly increasing subsequence.

 

Example 1:

Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
Example 2:

Input: nums = [0,1,0,3,2,3]
Output: 4
Example 3:

Input: nums = [7,7,7,7,7,7,7]
Output: 1
 

Constraints:

1 <= nums.length <= 2500
-104 <= nums[i] <= 104
 

Follow up: Can you come up with an algorithm that runs in O(n log(n)) time complexity?


Soln : 

class Solution {
    public int lengthOfLIS(int[] nums) {
        
        int n = nums.length;
        List<Integer> lisSet = new ArrayList<>();
        
        for(int i = 0 ; i < n ; i ++ ) {

            int val = Collections.binarySearch(lisSet , nums[i]);

            if(val >= 0 ) {
                // element exists fine 
                // no need to reinsert it 
            } else {
                // element does not exist but we are returned an index 
                // where the element can go 
                int insertionIndex = -val - 1;

                // two cases 
                if(insertionIndex == lisSet.size()) {
                    // simply insert this into the list 
                    lisSet.add(nums[i]);
                } else {
                    // why are we doing this i will explain in the 
                    // excali diagram am gonna draw 
                    lisSet.set(insertionIndex , nums[i]);
                }
            }
        }

        return lisSet.size();

    }
}
