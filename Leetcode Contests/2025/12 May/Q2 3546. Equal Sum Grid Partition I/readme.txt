Link : https://leetcode.com/problems/equal-sum-grid-partition-i/

Question : 

You are given an m x n matrix grid of positive integers. Your task is to determine if it is possible to make either one horizontal or one vertical cut on the grid such that:

Each of the two resulting sections formed by the cut is non-empty.
The sum of the elements in both sections is equal.
Return true if such a partition exists; otherwise return false.

 

Example 1:

Input: grid = [[1,4],[2,3]]

Output: true

Explanation:



A horizontal cut between row 0 and row 1 results in two non-empty sections, each with a sum of 5. Thus, the answer is true.

Example 2:

Input: grid = [[1,3],[2,4]]

Output: false

Explanation:

No horizontal or vertical cut results in two non-empty sections with equal sums. Thus, the answer is false.

 

Constraints:

1 <= m == grid.length <= 105
1 <= n == grid[i].length <= 105
2 <= m * n <= 105
1 <= grid[i][j] <= 105


Code : 

class Solution {
    public boolean canPartitionGrid(int[][] grid) {
        
        
        int n = grid.length;
        int m = grid[0].length;
        
        for(int i = 0 ; i < n ; i ++ ) {
            for(int j = 0 ; j < m ; j ++  ) {
                grid[i][j] += j > 0 ? grid[i][j - 1] : 0;
                //System.out.print(grid[i][j]);
            }
        }
        
        for(int j = 0 ; j < m ; j ++ ) {
            
            for(int i = 0 ; i < n ; i ++ ) {
                grid[i][j] += i > 0 ? grid[i - 1][j] : 0;
            }
        }
        
        
        int tot = grid[n - 1][m - 1];
        
        // row wise spliting 
        
        //System.out.println(tot);
        
        for(int i = 0 ; i < n - 1 ; i ++ ) {
            
            int half = grid[i][m - 1];
            int rem = tot - half;
            
            //System.out.println(half + " " + rem);
            
            if(half == rem ) {
                return true;
            }
        }
        
        for(int j = 0 ; j < m - 1 ; j ++ ) {
            
            int half = grid[n - 1][j];
            int rem = tot - half;
            
            System.out.println(half + " " + rem);
            
            if(half == rem) {
                return true;
            }
            
        }
        
        return false;
        
        
    }
}
