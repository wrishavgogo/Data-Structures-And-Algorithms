Link : https://leetcode.com/problems/number-of-digit-one/description/?envType=problem-list-v2&envId=dynamic-programming

Question : 

Given an integer n, count the total number of digit 1 appearing in all non-negative integers less than or equal to n.

 

Example 1:

Input: n = 13
Output: 6
Example 2:

Input: n = 0
Output: 0
 

Constraints:

0 <= n <= 109


Soln : 


We know digit dp uses the pos , tight and variable to maintain a state 

variable -> depends on the problem statement

Read the sexy algorithms part to understand what digit dp is and what is the intuition behind it 


class Solution {

    int[][][] cache = new int[20][2][20];

    public int countDigitOne(int n) {
        
        String nums = String.valueOf(n);
        invalidateCache();
        return f(0 , 1 , 0 , nums , 1);
    }

    void invalidateCache() {

        for(int i = 0 ; i < 20 ; i ++ ) {
            for(int j = 0 ; j < 2 ; j ++ ) {
                for(int k = 0; k < 20 ; k ++ ) {
                    cache[i][j][k] = -1;
                }
            }
        }
    }

    int f(int pos , int tight , int totalOnes , String nums  , int match) {

        if(pos == nums.length()) {

            return totalOnes;
        }

        if(cache[pos][tight][totalOnes] != -1) {
            return cache[pos][tight][totalOnes];
        }


        int limit = tight == 1 ? nums.charAt(pos) - '0' : 9;
        int ans = 0;

        for(int i = 0 ; i <= limit ; i ++ ) {

            int newTight = (tight == 1 && i == limit ) ? 1 : 0;
            int count = (i == match ) ? 1 : 0;
            ans += f(pos + 1 , newTight , totalOnes +  count , nums , match );
        }

        cache[pos][tight][totalOnes] = ans;
        return ans;
    }
}

