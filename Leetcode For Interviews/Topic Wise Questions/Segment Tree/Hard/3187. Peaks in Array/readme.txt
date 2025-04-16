Link : https://leetcode.com/problems/peaks-in-array/description/?envType=problem-list-v2&envId=segment-tree

Question : 

A peak in an array arr is an element that is greater than its previous and next element in arr.

You are given an integer array nums and a 2D integer array queries.

You have to process queries of two types:

queries[i] = [1, li, ri], determine the count of peak elements in the subarray nums[li..ri].
queries[i] = [2, indexi, vali], change nums[indexi] to vali.
Return an array answer containing the results of the queries of the first type in order.

Notes:

The first and the last element of an array or a subarray cannot be a peak.
 

Example 1:

Input: nums = [3,1,4,2,5], queries = [[2,3,4],[1,0,4]]

Output: [0]

Explanation:

First query: We change nums[3] to 4 and nums becomes [3,1,4,4,5].

Second query: The number of peaks in the [3,1,4,4,5] is 0.

Example 2:

Input: nums = [4,1,4,2,1,5], queries = [[2,2,4],[1,0,2],[1,0,4]]

Output: [0,1]

Explanation:

First query: nums[2] should become 4, but it is already set to 4.

Second query: The number of peaks in the [4,1,4] is 0.

Third query: The second 4 is a peak in the [4,1,4,2,1].

 

Constraints:

3 <= nums.length <= 105
1 <= nums[i] <= 105
1 <= queries.length <= 105
queries[i][0] == 1 or queries[i][0] == 2
For all i that:
queries[i][0] == 1: 0 <= queries[i][1] <= queries[i][2] <= nums.length - 1
queries[i][0] == 2: 0 <= queries[i][1] <= nums.length - 1, 1 <= queries[i][2] <= 105


Mistakes which i did 

1. in the update of segment tree , 
    if(index <= mid  ) {
                if(node.left == null ) {
                    node.left = new Node();
                }
                update(node.left , start , mid , index , val);
            }else {
                if(node.right == null ) {
                    node.right = new Node();
                    update(node.right , mid + 1 , end , index , val); =--------> i accidentally put the update in the if condition 
                }       
            }

2. for query type 2 , i was only updating the peak status of the index for which the update was coming for 
    , but i did not consider that the previous index and the next index will also get affected due to change in status of current index
  nums[queries[i][1]] = queries[i][2];
                int index = queries[i][1];
                int isPeak = 0;
                if(index != 0 && index != n - 1 && nums[index] > nums[index + 1] && nums[index] > nums[index - 1]) {
                    isPeak = 1;
                }
                segmentTree.updatePoint( index, isPeak );

                // updating this index also affects the next and prev Index
                // not we dont need to update the isPeak status of last and first element of the array
                // hence checking if range is within (1 , n - 2 )
                int prevIndex = index - 1;
                isPeak = 0;
                if(prevIndex > 0 && prevIndex < n - 1 && nums[prevIndex] > nums[prevIndex + 1] && nums[prevIndex] > nums[prevIndex - 1]) {
                    isPeak = 1;
                }
                segmentTree.updatePoint( prevIndex, isPeak );

                int nextIndex = index + 1;
                isPeak = 0;
                if(nextIndex > 0 && nextIndex < n - 1 && nums[nextIndex] > nums[nextIndex + 1] && nums[nextIndex] > nums[nextIndex - 1]) {
                    isPeak = 1;
                }
                segmentTree.updatePoint( nextIndex, isPeak );

3.  //[l ,...,r]
                // since l and r are the last elements of the subarray , they wont be considererd as peaks
                // we ideally have to calculate for [l + 1 , ..... , r - 1]
                
                l++;
                r--;
                if(l <= r ) {
                    ans.add(segmentTree.queryRange( l,r ));
                } else {
                    ans.add(0);
    
                }

Intuition , -> first check if the node is to get updated by checking if nums[index] > nums[index + 1] && nums[index] > nums[index - 1] 
                and as the update calls happen , check the new status of nums[index] and using the above condition
                and do pointed updated in segment tree to 0 or 1 
                and then do a range sum 

Code : 

class Solution {

    class Node {

        int val;
        Node left ,right;
    }

    class SparseSegmentTree {

        Node root;
        int L , R , def;

        public SparseSegmentTree(int L , int R , int def ) {
            root = new Node();
            this.L = L ;
            this.R = R ;
            this.def = def;
        }

        public void update(Node node , int start , int end , int index , int val ) {

            if(start == end ) {
                node.val = val;
                return;
            }

            int mid = start + (end - start ) / 2;
            if(index <= mid  ) {
                if(node.left == null ) {
                    node.left = new Node();
                }
                update(node.left , start , mid , index , val);
            }else {
                if(node.right == null ) {
                    node.right = new Node();
                }
                update(node.right , mid + 1 , end , index , val);
            }

            int leftVal = node.left != null ? node.left.val : def;
            int rightVal = node.right != null ? node.right.val : def;
            node.val = leftVal + rightVal;
        }

        public void updatePoint(int index , int val) {
            update(root , L , R , index , val);
        }

        public int query(Node node , int start , int end , int l , int r ) {
            
            if(node == null || l > end || r < start ) {
                // out of range
                return def;
            }
            if(l <= start && end <= r ) {
                // completely within range
                return node.val;
            }
            int mid = start + (end - start ) / 2;
            int leftVal = query(node.left , start , mid , l , r);
            int rightVal = query(node.right , mid + 1 , end , l , r );
            return leftVal + rightVal;
        }

        public int queryRange(int l , int r) {
            return query(root , L , R , l , r);
        }


    }
    public List<Integer> countOfPeaks(int[] nums, int[][] queries) {
        
        
        int n = nums.length;
        SparseSegmentTree segmentTree = new SparseSegmentTree(0 , n - 1 , 0);
        for(int i = 1 ; i < n - 1 ; i ++ ) {
            // 0 and n-1 will never be peak so no point in updating
            int isPeak = 0;
            if(nums[i] > nums[i - 1] && nums[i] > nums[i + 1]) {
                isPeak = 1;
            }
            //System.out.print(i + " " + isPeak + " ,");
            segmentTree.updatePoint(i , isPeak);
        }

        List<Integer> ans = new ArrayList<>();

        //System.out.println(segmentTree.queryRange(0,3));
        int m = queries.length;

        for(int i = 0 ; i < m ; i ++ ) {

            if(queries[i][0] == 1 ) {
                int l = queries[i][1];
                int r = queries[i][2];
                
                //[l ,...,r]
                // since l and r are the last elements of the subarray , they wont be considererd as peaks
                // we ideally have to calculate for [l + 1 , ..... , r - 1]
                
                l++;
                r--;
                if(l <= r ) {
                    ans.add(segmentTree.queryRange( l,r ));
                } else {
                    ans.add(0);
                }
                
            } else {
                nums[queries[i][1]] = queries[i][2];
                int index = queries[i][1];
                int isPeak = 0;
                if(index != 0 && index != n - 1 && nums[index] > nums[index + 1] && nums[index] > nums[index - 1]) {
                    isPeak = 1;
                }
                segmentTree.updatePoint( index, isPeak );

                // updating this index also affects the next and prev Index
                // not we dont need to update the isPeak status of last and first element of the array
                // hence checking if range is within (1 , n - 2 )
                int prevIndex = index - 1;
                isPeak = 0;
                if(prevIndex > 0 && prevIndex < n - 1 && nums[prevIndex] > nums[prevIndex + 1] && nums[prevIndex] > nums[prevIndex - 1]) {
                    isPeak = 1;
                }
                segmentTree.updatePoint( prevIndex, isPeak );

                int nextIndex = index + 1;
                isPeak = 0;
                if(nextIndex > 0 && nextIndex < n - 1 && nums[nextIndex] > nums[nextIndex + 1] && nums[nextIndex] > nums[nextIndex - 1]) {
                    isPeak = 1;
                }
                segmentTree.updatePoint( nextIndex, isPeak );

            }
        }

        return ans;
    }
}

