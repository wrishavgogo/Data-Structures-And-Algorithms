Link : https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/description/?envType=company&envId=oracle&favoriteSlug=oracle-all

Question : 

Given an array nums of n integers where nums[i] is in the range [1, n], return an array of all the integers in the range [1, n] that do not appear in nums.

 

Example 1:

Input: nums = [4,3,2,7,8,2,3,1]
Output: [5,6]
Example 2:

Input: nums = [1,1]
Output: [2]
 

Constraints:

n == nums.length
1 <= n <= 105
1 <= nums[i] <= n
 

Follow up: Could you do it without extra space and in O(n) runtime? You may assume the returned list does not count as extra space


Code1 : In place Hashing 

class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        
        int n = nums.length;
        List<Integer> ans = new ArrayList<>();
        for(int i = 0 ; i < n ; i ++ ) {
            int val = Math.abs(nums[i]);
            if(val > n ) {
                continue;
            }
            val--;
            nums[val] = - Math.abs(nums[val]);
        }

        for(int i = 0 ; i < n ; i ++ ) {
            if(nums[i] > 0 ) {
                ans.add(i + 1);
            }
        }

        return ans;
    }
}

Code 2 : In place swapping 

// code pending 


class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        
        int n = nums.length;
        List<Integer> ans = new ArrayList<>();
        // the reason inplace swapping works here because 
        // its given in the question 
        // 1 <= nums[i] <= n
        
        for(int i = 0 ; i < n ; i ++ ) {

            int correctIndex = nums[i] - 1;

            while(i + 1  != nums[i]) {
                // running while till the correct value is not placed 
                swap(nums , i , nums[i] - 1);

                if(nums[nums[i] - 1] == nums[i]) {
                    // the value to be swapped is same 
                    // as current value 
                    break;
                }
            }
        }


        for(int i = 0 ; i < n ; i ++ ) {

            if(nums[i] != (i + 1 )) {
                ans.add(i + 1);
            }
        }
        

        return ans;
    }

    public void swap(int[] nums , int idx1 , int idx2) {

        int tmp = nums[idx1];
        nums[idx1] = nums[idx2];
        nums[idx2] = tmp;
    }
}
