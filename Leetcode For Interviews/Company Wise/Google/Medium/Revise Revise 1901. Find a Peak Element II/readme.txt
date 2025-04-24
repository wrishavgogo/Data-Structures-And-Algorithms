Premium 
Link : https://leetcode.com/problems/find-a-peak-element-ii/description/

Prerequisite : https://github.com/wrishavgogo/Data-Structures-And-Algorithms/tree/main/Leetcode%20For%20Interviews/Company%20Wise/Google/Medium/Revise%20Revise%20162.%20Find%20Peak%20Element

Question : 

A peak element in a 2D grid is an element that is strictly greater than all of its adjacent neighbors to the left, right, top, and bottom.

Given a 0-indexed m x n matrix mat where no two adjacent cells are equal, find any peak element mat[i][j] and return the length 2 array [i,j].

You may assume that the entire matrix is surrounded by an outer perimeter with the value -1 in each cell.

You must write an algorithm that runs in O(m log(n)) or O(n log(m)) time.

 

Example 1:



Input: mat = [[1,4],[3,2]]
Output: [0,1]
Explanation: Both 3 and 4 are peak elements so [1,0] and [0,1] are both acceptable answers.
Example 2:



Input: mat = [[10,20,15],[21,30,14],[7,16,32]]
Output: [1,1]
Explanation: Both 30 and 32 are peak elements so [1,1] and [2,2] are both acceptable answers.
 

Constraints:

m == mat.length
n == mat[i].length
1 <= m, n <= 500
1 <= mat[i][j] <= 105
No two adjacent cells are equal.



Soln : 


class Solution {
    public int[] findPeakGrid(int[][] mat) {
        

        int[] ans = new int[2];
        ans[0] = -1;
        ans[1] = -1;
        int n = mat.length;
        int m = mat[0].length;

        int lo = 0;
        int hi = m - 1;
        
        while(lo <= hi ) {

            int mid = (lo + hi) / 2;

            // mid is the col name 
            // finding the max row now 

            int maxRow = 0;
            for(int i = 1 ; i < n ; i ++ ) {
                if(mat[maxRow][mid] < mat[i][mid]) {
                    maxRow = i;
                }
            }

            if(checkPeak(mat , maxRow, mid)) {
                ans[0] = maxRow;
                ans[1] = mid;
                break;
            }

            // since its max so we dont check top and bottom value 
            // just checking left and right 
            // to decide which direction to expand our search into 
            // just check the neighbouring left and right cell value
            if(mid - 1 >= 0 && mat[maxRow][mid - 1 ] > mat[maxRow][mid]) {
                hi = mid - 1;
            }
            else {
                lo = mid + 1;
            }

        }
        
        // peak element will always exist , 
        return ans;
    }


    boolean checkPeak(int[][] mat , int i , int j ) {

        int n = mat.length;
        int m = mat[0].length;

        int leftVal = isValidCell(i , j - 1 , mat) ? mat[i][j - 1] : -1;
        int rightVal = isValidCell(i , j + 1 , mat ) ? mat[i][j + 1] : -1;
        int topVal = isValidCell(i - 1  , j  , mat) ? mat[i - 1][j] : -1;
        int bottomVal = isValidCell(i + 1  , j , mat ) ? mat[i + 1][j] : -1;

        int val = mat[i][j];

        return (val > leftVal) && (val > rightVal) && (val > topVal ) && (val > bottomVal);

    }

    boolean isValidCell(int i , int j , int[][] mat ) {

        int n = mat.length;
        int m = mat[0].length;

        return i >= 0 && i < n && j >= 0 && j < m ;
    }
}
