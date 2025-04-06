Link : https://leetcode.com/problems/paint-house-ii/description/?envType=company&envId=linkedin&favoriteSlug=linkedin-three-months

Question : 

There are a row of n houses, each house can be painted with one of the k colors. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by an n x k cost matrix costs.

For example, costs[0][0] is the cost of painting house 0 with color 0; costs[1][2] is the cost of painting house 1 with color 2, and so on...
Return the minimum cost to paint all houses.

 

Example 1:

Input: costs = [[1,5,3],[2,9,4]]
Output: 5
Explanation:
Paint house 0 into color 0, paint house 1 into color 2. Minimum cost: 1 + 4 = 5; 
Or paint house 0 into color 2, paint house 1 into color 0. Minimum cost: 3 + 2 = 5.
Example 2:

Input: costs = [[1,3],[2,4]]
Output: 5
 

Constraints:

costs.length == n
costs[i].length == k
1 <= n <= 100
2 <= k <= 20
1 <= costs[i][j] <= 20
 

Follow up: Could you solve it in O(nk) runtime?


solution : I was able to do the solution my self pretty easily still i will share the thought process in the explanation pdf

class Solution {
    public int minCostII(int[][] costs) {
        
        int n = costs.length;
        int k = costs[0].length;

        int[][] dp = new int[n][k];
        /* dp[i][j] -> 
            represents from 1 -> i  houses are painted
            and min cost of painting all the houses till now 
            such that you are painting house i with jth color
        */
        

        for(int j = 0 ; j < k ; j ++ ) {
            // cost to paint 0th house with colours 
            dp[0][j] = costs[0][j];
        }

        for(int i = 1 ; i < n ; i ++ ) {

            int minPrefix = Integer.MAX_VALUE;
            dp[i][0] = minPrefix;
            for(int j = 1 ; j < k ; j ++ ) {
                minPrefix = Math.min(minPrefix, dp[i - 1][j - 1] );
                dp[i][j] = minPrefix;
            }

            int minSuffix = Integer.MAX_VALUE;
            for(int j = k - 2 ; j >= 0 ; j -- ) {
                minSuffix = Math.min(minSuffix , dp[i - 1][j + 1]);
                dp[i][j] = Math.min(minSuffix , dp[i][j] );
            }

            for(int j = 0 ; j < k ; j ++ ) {
                dp[i][j] += costs[i][j];
            }
        }

        int ans = Integer.MAX_VALUE;

        for(int j = 0 ; j < k ; j ++ ) {

            ans = Math.min(ans , dp[n-1][j]);
        }
        
        return ans;
        
    }
}
