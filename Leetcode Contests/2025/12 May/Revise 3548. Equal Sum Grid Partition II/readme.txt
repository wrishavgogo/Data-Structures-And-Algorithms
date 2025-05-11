Link : https://leetcode.com/problems/equal-sum-grid-partition-ii/description/


Question : 

You are given an m x n matrix grid of positive integers. Your task is to determine if it is possible to make either one horizontal or one vertical cut on the grid such that:

Each of the two resulting sections formed by the cut is non-empty.
The sum of elements in both sections is equal, or can be made equal by discounting at most one single cell in total (from either section).
If a cell is discounted, the rest of the section must remain connected.
Return true if such a partition exists; otherwise, return false.

Note: A section is connected if every cell in it can be reached from any other cell by moving up, down, left, or right through other cells in the section.

 

Example 1:

Input: grid = [[1,4],[2,3]]

Output: true

Explanation:



A horizontal cut after the first row gives sums 1 + 4 = 5 and 2 + 3 = 5, which are equal. Thus, the answer is true.
Example 2:

Input: grid = [[1,2],[3,4]]

Output: true

Explanation:



A vertical cut after the first column gives sums 1 + 3 = 4 and 2 + 4 = 6.
By discounting 2 from the right section (6 - 2 = 4), both sections have equal sums and remain connected. Thus, the answer is true.
Example 3:

Input: grid = [[1,2,4],[2,3,5]]

Output: false

Explanation:



A horizontal cut after the first row gives 1 + 2 + 4 = 7 and 2 + 3 + 5 = 10.
By discounting 3 from the bottom section (10 - 3 = 7), both sections have equal sums, but they do not remain connected as it splits the bottom section into two parts ([2] and [5]). Thus, the answer is false.
Example 4:

Input: grid = [[4,1,8],[3,2,6]]

Output: false

Explanation:

No valid cut exists, so the answer is false.

 

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
        long[][] actual = new long[n][m];
        long[][] prefix = new long[n][m];
        
        Map<Integer , List<Integer>> valToRowList = new HashMap<>();
        Map<Integer , List<Integer>> valToColList = new HashMap<>();
        
        for(int i = 0 ; i < n ; i ++ ) {
            for(int j = 0 ; j < m ; j ++  ) {
                
                int val = grid[i][j];
                prefix[i][j] = grid[i][j];
                actual[i][j] = grid[i][j];
                
                valToRowList.computeIfAbsent(val , (Integer key) -> new ArrayList<>());
                valToColList.computeIfAbsent(val , (Integer key) -> new ArrayList<>());
                
                if(valToRowList.get(val).size() > 0 ) {
                    
                    int sz = valToRowList.get(val).size();
                    if(valToRowList.get(val).get(sz - 1 ) < i ) {
                        valToRowList.get(val).add(i);
                    }
                } else {
                    valToRowList.get(val).add(i);
                }
                
                
                if(valToColList.get(val).size() > 0 ) {
                    
                    int sz = valToColList.get(val).size();
                    if(valToColList.get(val).get(sz - 1 ) < j ) {
                        valToColList.get(val).add(j);
                    }
                } else {
                    valToColList.get(val).add(j);
                }
                
                
                    
                prefix[i][j] += j > 0 ? prefix[i][j - 1] : 0;
                
                
                //System.out.print(grid[i][j]);
            }
        }
        
        for(int j = 0 ; j < m ; j ++ ) {
            
            for(int i = 0 ; i < n ; i ++ ) {
                prefix[i][j] += i > 0 ? prefix[i - 1][j] : 0;
            }
        }
        
        
        
        long tot = prefix[n - 1][m - 1];
        
        // row wise spliting 
        
        //System.out.println(tot);
        
        for(int i = 0 ; i < n - 1 ; i ++ ) {
            
            long half = prefix[i][m - 1];
            long rem = tot - half;
            
            if(half == rem) {
                return true;
            }
            
            
            long diff = half - rem;
            
           if(diff >  0) {
               
               // search in upper half 
               
               if(i == 0 ) {
                   
                   if(actual[0][0] == diff || actual[0][m - 1] == diff) {
                       return true;
                   }
                        
               } else {
                   
                   if(bSearchLessThanEqualTo(valToRowList.get(diff) , i)) {
                       return true;
                   }
                   
               }
               
               
           } else {
               
               // search in lower half 
               diff = Math.abs(diff);
               
              if(i == n - 2 ) {
                  
                  if(actual[n-1][0] == diff || actual[n-1][m - 1] == diff) {
                       return true;
                   }
              } else {
                  
                  if(bSearchGreaterThanEqualTo(valToRowList.get(diff) , i + 1)) {
                       return true;
                   }
              }
           }
            
            
            
            
            
        }
        
        for(int j = 0 ; j < m - 1 ; j ++ ) {
            
            long half = prefix[n - 1][j];
            long rem = tot - half;
            
            
            if(half == rem) {
                return true;
            }
            
            long diff = half - rem;
            
            if(diff >  0) {
               
               // search in left half 
               
               if(j == 0 ) {
                   
                   if(actual[0][0] == diff || actual[n-1][0] == diff) {
                       return true;
                   }
                        
               } else {
                   
                   if(bSearchLessThanEqualTo(valToColList.get(diff) , j)) {
                       return true;
                   }
                   
               }
               
               
           } else {
               
               // search in lower half 
               diff = Math.abs(diff);
               
              if(j == m - 2 ) {
                  
                  if(actual[0][m-1] == diff || actual[n-1][m - 1] == diff) {
                       return true;
                   }
              } else {
                  
                  if(bSearchGreaterThanEqualTo(valToColList.get(diff) , j + 1)) {
                       return true;
                   }
              }
           }
            
            
            
        }
        
        return false;
        
        
    }
    
    private boolean bSearchLessThanEqualTo(List<Integer> arr , int k) {
        
        if(arr == null ) {
            return false;
        }
        
        int lo = 0;
        int hi = arr.size() - 1;
        
        boolean found = false;
        while(lo <= hi ) {
            
            int mid = (lo + hi)/2;
            
            if(arr.get(mid) > k ) {
                hi = mid - 1;
            } else {
                found = true;
                lo = mid + 1 ;
            }
        }
        
        
        return found;
    }
    
    private boolean bSearchGreaterThanEqualTo(List<Integer> arr , int k) {
        
        if(arr == null ) {
            return false;
        }
        int lo = 0;
        int hi = arr.size() - 1;
        
        boolean found = false;
        while(lo <= hi ) {
            
            int mid = (lo + hi)/2;
            
            if(arr.get(mid) < k ) {
                lo = mid + 1;
            } else {
                found = true;
                hi = mid - 1;
            }
        }
        
        
        return found;
    }
    
    
}
