Question : 

You are given a 0-indexed integer array nums and an integer pivot. Rearrange nums such that the following conditions are satisfied:

Every element less than pivot appears before every element greater than pivot.
Every element equal to pivot appears in between the elements less than and greater than pivot.
The relative order of the elements less than pivot and the elements greater than pivot is maintained.
More formally, consider every pi, pj where pi is the new position of the ith element and pj is the new position of the jth element. If i < j and both elements are smaller (or larger) than pivot, then pi < pj.
Return nums after the rearrangement.

 

Example 1:

Input: nums = [9,12,5,10,14,3,10], pivot = 10
Output: [9,5,3,10,10,12,14]
Explanation: 
The elements 9, 5, and 3 are less than the pivot so they are on the left side of the array.
The elements 12 and 14 are greater than the pivot so they are on the right side of the array.
The relative ordering of the elements less than and greater than pivot is also maintained. [9, 5, 3] and [12, 14] are the respective orderings.
Example 2:

Input: nums = [-3,4,3,2], pivot = 2
Output: [-3,2,4,3]
Explanation: 
The element -3 is less than the pivot so it is on the left side of the array.
The elements 4 and 3 are greater than the pivot so they are on the right side of the array.
The relative ordering of the elements less than and greater than pivot is also maintained. [-3] and [4, 3] are the respective orderings.
 

Constraints:

1 <= nums.length <= 105
-106 <= nums[i] <= 106
pivot equals to an element of nums.



Answer : 

Intuition :->  
Create a new array , you can start using two pointers :
 i------>   <----j
[9,12,5,10,14,3,10], pivot = 10

make i go from 0 to n - 1 
and j go from n - 1 to  0 
                  smaller     greater
in the new array [_,_,_,_,_,_,_]

have smaller and greater pointer , keep moving i as i++ , and whenever you encounter a number smaller than number 
just do newArr[smaller] = arr[i] and do smaller++
then do newArr[greater] = arr[j] and do greater--

and at the end fill up the empty places with pivot , while(smaller <= greater) 

  Code : 
class Solution {
    public int[] pivotArray(int[] nums, int pivot) {
        
        int n = nums.length;
        int[] ans = new int[n];

        Arrays.fill(ans , 0);

        int smaller = 0;
        int greater = n - 1;
        int i = 0 ;
        int j = n - 1 ;

        while(i < n && j >= 0 ) {

            if(nums[i] < pivot) {
                ans[smaller] = nums[i];
                smaller++;
            }
            if(nums[j] > pivot) {
                ans[greater] = nums[j];
                greater--;
            }
            i++;
            j--;
        }

        while(smaller <= greater && smaller < n ) {
            ans[smaller] = pivot;
            smaller++;
        }


        return ans;




    }
}

