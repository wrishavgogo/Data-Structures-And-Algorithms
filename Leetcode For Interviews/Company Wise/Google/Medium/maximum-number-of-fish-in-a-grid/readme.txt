Link : https://leetcode.com/problems/maximum-number-of-fish-in-a-grid/?envType=company&envId=google&favoriteSlug=google-six-months


You are given a 0-indexed 2D matrix grid of size m x n, where (r, c) represents:

A land cell if grid[r][c] = 0, or
A water cell containing grid[r][c] fish, if grid[r][c] > 0.
A fisher can start at any water cell (r, c) and can do the following operations any number of times:

Catch all the fish at cell (r, c), or
Move to any adjacent water cell.
Return the maximum number of fish the fisher can catch if he chooses his starting cell optimally, or 0 if no water cell exists.

An adjacent cell of the cell (r, c), is one of the cells (r, c + 1), (r, c - 1), (r + 1, c) or (r - 1, c) if it exists.

 

Example 1:


Input: grid = [[0,2,1,0],[4,0,0,3],[1,0,0,4],[0,3,2,0]]
Output: 7
Explanation: The fisher can start at cell (1,3) and collect 3 fish, then move to cell (2,3) and collect 4 fish.
Example 2:


Input: grid = [[1,0,0,0],[0,0,0,0],[0,0,0,0],[0,0,0,1]]
Output: 1
Explanation: The fisher can start at cells (0,0) or (3,3) and collect a single fish. 
 

Constraints:

m == grid.length
n == grid[i].length
1 <= m, n <= 10
0 <= grid[i][j] <= 10


soln : Vanialla dfs on each cell , just make the nodes visited as water node so you dont visit them again 

Code : 
class Solution {

    int[][] dir = new int[][]{
    {1, 0},  // Right
    {0, 1},  // Down
    {-1, 0}, // Left
    {0, -1}  // Up
    };

    public int findMaxFish(int[][] grid) {
        

        // we can do bfs on each of the water nodes 
        // we can use the grid array itself to mark nodes visited 
        // by make grid[i][j] = 0 , if nodes are 0 


        int n = grid.length;
        int m = grid[0].length;

        int ans = 0;
        for(int i = 0 ; i < n ; i ++ ) {
            for(int j = 0 ; j < m ; j ++ ) {

                ans = Math.max(ans , dfs(i , j , grid));
            }
        }

        return ans;


    }


    private int dfs(int x ,int y  , int[][] grid) {

        if(!isValidCell(x , y , grid)) {
            return 0;
        }

        if(grid[x][y] == 0) {
            //already visited cell
            return 0;
        }


        int sum = grid[x][y];
        grid[x][y] = 0; // marking them visited
        for(int i = 0 ; i < dir.length ; i ++ ) {

            int newx = x + dir[i][0];
            int newy = y + dir[i][1];
            
            sum += dfs(newx , newy , grid);
        }

        return sum;


    }

    private boolean isValidCell(int x , int y , int[][] grid) {

        int n = grid.length;
        int m = grid[0].length;

        if( x >= 0 && x < n && y >= 0 && y < m ) {
            return true;
        }

        return false;
    }
}
