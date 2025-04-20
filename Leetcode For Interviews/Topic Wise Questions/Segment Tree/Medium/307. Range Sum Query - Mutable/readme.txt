Link : https://leetcode.com/problems/range-sum-query-mutable/description/?envType=problem-list-v2&envId=segment-tree

Question : 

Given an integer array nums, handle multiple queries of the following types:

Update the value of an element in nums.
Calculate the sum of the elements of nums between indices left and right inclusive where left <= right.
Implement the NumArray class:

NumArray(int[] nums) Initializes the object with the integer array nums.
void update(int index, int val) Updates the value of nums[index] to be val.
int sumRange(int left, int right) Returns the sum of the elements of nums between indices left and right inclusive (i.e. nums[left] + nums[left + 1] + ... + nums[right]).
 

Example 1:

Input
["NumArray", "sumRange", "update", "sumRange"]
[[[1, 3, 5]], [0, 2], [1, 2], [0, 2]]
Output
[null, 9, null, 8]

Explanation
NumArray numArray = new NumArray([1, 3, 5]);
numArray.sumRange(0, 2); // return 1 + 3 + 5 = 9
numArray.update(1, 2);   // nums = [1, 2, 5]
numArray.sumRange(0, 2); // return 1 + 2 + 5 = 8
 

Constraints:

1 <= nums.length <= 3 * 104
-100 <= nums[i] <= 100
0 <= index < nums.length
-100 <= val <= 100
0 <= left <= right < nums.length
At most 3 * 104 calls will be made to update and sumRange.



Solution  : basic segment tree with point update and range sum query 

C++ soln 

class NumArray {
    const static int sz = 1e5 + 1;
    int seg[sz];
    vector<int> nums1;
public:
    NumArray(vector<int>& nums) {
        int n = nums.size();
        nums1 = nums;
        build(0 , n - 1 , 0 , nums);
    }
    
    void build(int start , int end , int node , vector<int>& nums) {
        if(start == end ) {
            seg[node] = nums[start];
            return;
        }
        
        int mid = (start + end)/2;
        build(start , mid , 2*node + 1 , nums);
        build(mid + 1 , end , 2*node + 2 , nums);
        
        seg[node] = seg[2*node + 1] + seg[2*node + 2];
    }
    
    
    void updateSeg(int start , int end , int node , int index , int val , vector<int>& nums) {
        if(start == end and start == index) {
            nums[start] = val;
            seg[node] = val;
            return;
        }
        
        if(start > index or index > end ) {
            return ;
        }
        
        int mid = (start + end)/2;
        if(index <= mid ) {
            updateSeg(start , mid , 2*node + 1 , index , val , nums);
        }
        else {
            updateSeg(mid + 1 , end , 2*node + 2 , index , val , nums);
        }
        
        seg[node] = seg[2*node + 1] + seg[2*node + 2];
        
    }
    
    
    int query(int start , int end , int node , int L , int R ) {
        if(L <= start and end <= R) {
            return seg[node];
        }
        
        if(L > end or R < start ) {
            return 0;
        }
        int mid = (start + end)/2;
        int q1 = query(start , mid , 2*node + 1 , L , R);
        int q2 = query(mid + 1 , end , 2*node + 2 , L , R);
        return q1 + q2;
    }
    
    void update(int index, int val) {
        int n = nums1.size();
        updateSeg(0 , n - 1 , 0 , index , val , nums1);
    }
    
    int sumRange(int left, int right) {
        int n = nums1.size();
        return query(0 , n - 1 , 0 , left , right);
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * obj->update(index,val);
 * int param_2 = obj->sumRange(left,right);
 */
