I wrote a solution whose runtime was

N^2LogN 
but then i saw a solution which is N*(4LogN) --> amazing soln i liked the thought process going to note it down 

Link : https://leetcode.com/problems/count-number-of-teams/description/?envType=problem-list-v2&envId=binary-indexed-tree

Question : 

There are n soldiers standing in a line. Each soldier is assigned a unique rating value.

You have to form a team of 3 soldiers amongst them under the following rules:

Choose 3 soldiers with index (i, j, k) with rating (rating[i], rating[j], rating[k]).
A team is valid if: (rating[i] < rating[j] < rating[k]) or (rating[i] > rating[j] > rating[k]) where (0 <= i < j < k < n).
Return the number of teams you can form given the conditions. (soldiers can be part of multiple teams).

 

Example 1:

Input: rating = [2,5,3,4,1]
Output: 3
Explanation: We can form three teams given the conditions. (2,3,4), (5,4,1), (5,3,1). 
Example 2:

Input: rating = [2,1,3]
Output: 0
Explanation: We can't form any team given the conditions.
Example 3:

Input: rating = [1,2,3,4]
Output: 4
 

Constraints:

n == rating.length
3 <= n <= 1000
1 <= rating[i] <= 105
All the integers in rating are unique


My Soln : N^2LogN

class Solution {

    final int MAX_RATING = (int) (1e5 + 1);
    class BitTree {

        int[] tree;
        int n;

        public BitTree(int n) {
            this.n = n;
            tree = new int[n + 1];
        }

        public void update(int index , int val) {

            // increase range 1 then 2 then 4 each time by next range
            while(index <= n ) {
                tree[index] += val;
                index += (index) & (-index);
            }
        }

        public int sum(int index) {
            int sum = 0;
            while(index > 0  ) {
                sum += tree[index];
                index -= (index) & (-index);
            }
            return sum;
        }

    }


    public int numTeams(int[] rating) {
        
        int ans = 0 ;
        int n = rating.length;
        BitTree bitTree = new BitTree(MAX_RATING);

        for(int i = 0 ; i < n ; i ++ ) {
            bitTree.update(rating[i] , 1);
        }


        for(int i = 0 ; i < n ; i ++ ) {

            bitTree.update(rating[i] , -1);

            for(int j = i + 1 ;  j < n ; j ++ ) {
                // first trying to find rat[i] < rat[j] < rat[k]
                bitTree.update(rating[j] , -1);
                if(rating[j] > rating[i]) {
                    // number of elements greater than rating[j];
                    ans += bitTree.sum(MAX_RATING) - bitTree.sum(rating[j]);
                }

                // second trying to find rat[i] > rat[j] > rat[k]
                if(rating[j] < rating[i]) {
                    // number of elements less than rating[j]
                    ans += bitTree.sum(rating[j] - 1);
                }
            }

            for(int j = i + 1 ; j < n ; j ++ ) {
                // again add 
                bitTree.update(rating[j] , 1);
            }
        }

        return ans ;
    }
}


Faster Soln : N(LOGN)

class Solution {
    public int numTeams(int[] rating) {
        int max = rating[0];
        for (int val: rating) {
            if (val > max) {
                max = val;
            }
        }

        int[] leftBIT = new int[max + 1];
        int[] rightBIT = new int[max + 1];

        for (int val: rating) {
            updateBIT(leftBIT, val, 1);
        }

        int times = 0;

        for (int cur: rating) {
            updateBIT(leftBIT, cur, -1);

            int leftSmall = getPrefixSum(leftBIT, cur - 1);
            int rightSmall = getPrefixSum(rightBIT, cur - 1);

            int leftBig = getPrefixSum(leftBIT, max) - getPrefixSum(leftBIT, cur);
            int rightBig = getPrefixSum(rightBIT, max) - getPrefixSum(rightBIT, cur);

            times += leftSmall * rightBig;
            times += rightSmall * leftBig;

            updateBIT(rightBIT, cur, 1);
        }

        return times;
    }

    void updateBIT(int[] arr, int index, int val) {
        while (index < arr.length) {
            arr[index] += val;
            index += index & (-index);
        }
    }

    int getPrefixSum(int[] arr, int index) {
        int sum = 0;
        while (index > 0) {
            sum += arr[index];
            index -= index & (-index);
        }

        return sum;
    }
}



