Link : https://leetcode.com/problems/find-in-mountain-array/description/

Prerequisite : https://github.com/wrishavgogo/Data-Structures-And-Algorithms/tree/main/Leetcode%20For%20Interviews/Company%20Wise/Google/Medium/Revise%20852.%20Peak%20Index%20in%20a%20Mountain%20Array

Question : 

(This problem is an interactive problem.)

You may recall that an array arr is a mountain array if and only if:

arr.length >= 3
There exists some i with 0 < i < arr.length - 1 such that:
arr[0] < arr[1] < ... < arr[i - 1] < arr[i]
arr[i] > arr[i + 1] > ... > arr[arr.length - 1]
Given a mountain array mountainArr, return the minimum index such that mountainArr.get(index) == target. If such an index does not exist, return -1.

You cannot access the mountain array directly. You may only access the array using a MountainArray interface:

MountainArray.get(k) returns the element of the array at index k (0-indexed).
MountainArray.length() returns the length of the array.
Submissions making more than 100 calls to MountainArray.get will be judged Wrong Answer. Also, any solutions that attempt to circumvent the judge will result in disqualification.

 

Example 1:

Input: mountainArr = [1,2,3,4,5,3,1], target = 3
Output: 2
Explanation: 3 exists in the array, at index=2 and index=5. Return the minimum index, which is 2.
Example 2:

Input: mountainArr = [0,1,2,4,2,1], target = 3
Output: -1
Explanation: 3 does not exist in the array, so we return -1.
 

Constraints:

3 <= mountainArr.length() <= 104
0 <= target <= 109
0 <= mountainArr.get(index) <= 109


soln : 

/**
 * // This is MountainArray's API interface.
 * // You should not implement it, or speculate about its implementation
 * interface MountainArray {
 *     public int get(int index) {}
 *     public int length() {}
 * }
 */
 
class Solution {
    public int findInMountainArray(int target, MountainArray mountainArr) {
        
        int n = mountainArr.length();

        // given its a mountain array , we can find the peak index 
        // ofc 0 and n - 1 wont be peak index 


        int lo = 0;
        int hi = n - 1;
        int peak = -1;
        while(lo <= hi ) {
            int mid = (lo + hi) / 2;
            if(mid == 0 ) { 
                lo = mid + 1;
                continue;
            }
            if(mid == n - 1 ) {
                hi = mid - 1;
                continue;
            }
            int val = mountainArr.get(mid);
            int leftVal = mountainArr.get(mid - 1);
            int rightVal = mountainArr.get(mid + 1);

            if(val > leftVal && val > rightVal ) {
                peak = mid;
                break;
            }

            // non peak situation
            if(val > rightVal) {
                // we are in the rightHalf of the peak
                hi = mid - 1;
            }

            if(val < rightVal ) {
                // we are on the left half
                lo = mid + 1 ;
            }
        }

        int result = Math.min(findIndexLeft(mountainArr , 0 , peak , target ) , 
                              findIndexRight(mountainArr , peak , n-1  , target ));

        return result == Integer.MAX_VALUE ? -1 : result;

    }

    int findIndexLeft(MountainArray mountainArr , int start , int end , int target ) {

        // array is increasing 

        int lo = start ;
        int hi = end ;
        int index = Integer.MAX_VALUE;
        while(lo <= hi ) {
            int mid = (lo + hi)/2;
            int val = mountainArr.get(mid);
            if(val >= target ) {
                hi = mid - 1;
                if(val == target ) {
                    index = mid;
                }
            } else {
                lo = mid + 1;
            }
        }

        return index;
    }

    int findIndexRight(MountainArray mountainArr , int start , int end , int target  ) {

        // decreasing array 
        int lo = start ;
        int hi = end ;
        int index = Integer.MAX_VALUE;
        while(lo <= hi ) {
            int mid = (lo + hi)/2;
            int val = mountainArr.get(mid);
            if(val >= target ) {
                lo = mid + 1;
                if(val == target ) {
                    index = mid;
                }
            } else {
                hi = mid - 1;
            }
        }

        return index;
    }
}
