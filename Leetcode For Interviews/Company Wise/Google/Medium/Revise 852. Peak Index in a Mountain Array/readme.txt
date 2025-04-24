Link : https://leetcode.com/problems/peak-index-in-a-mountain-array/description/?envType=company&envId=google&favoriteSlug=google-thirty-days

Question : 

You are given an integer mountain array arr of length n where the values increase to a peak element and then decrease.

Return the index of the peak element.

Your task is to solve it in O(log(n)) time complexity.

 

Example 1:

Input: arr = [0,1,0]

Output: 1

Example 2:

Input: arr = [0,2,1,0]

Output: 1

Example 3:

Input: arr = [0,10,5,2]

Output: 1

 

Constraints:

3 <= arr.length <= 105
0 <= arr[i] <= 106
arr is guaranteed to be a mountain array.



Solution : 

class Solution {
    public int peakIndexInMountainArray(int[] arr) {
        

        int n = arr.length;

        int lo = 0 ;
        int hi = n - 1;

        while( lo <= hi ) {

            int mid = (lo + hi) / 2;

            if(mid == 0 ) {
                // in increasing half 
                lo = mid + 1;
                continue;
            }

            if(mid == n - 1 ) {
                // in decreasing half
                hi = mid - 1;
                continue;
            }

            if(arr[mid - 1 ] < arr[mid ] && arr[mid] > arr[mid + 1]) {
                // peak index
                return mid;
            }

            if(arr[mid] > arr[mid + 1]) {
                // decreasing half 
                // we dont need to worry abt mid + 1 going beyond bound 
                // because mid is expanding range
                hi = mid - 1;
            } else {
                lo = mid + 1;
            }
        }



        return -1;
    }
}
