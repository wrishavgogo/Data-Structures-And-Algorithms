Link : https://leetcode.com/problems/count-vowels-permutation/description/?envType=company&envId=atlassian&favoriteSlug=atlassian-all

Question : 

Given an integer n, your task is to count how many strings of length n can be formed under the following rules:

Each character is a lower case vowel ('a', 'e', 'i', 'o', 'u')
Each vowel 'a' may only be followed by an 'e'.
Each vowel 'e' may only be followed by an 'a' or an 'i'.
Each vowel 'i' may not be followed by another 'i'.
Each vowel 'o' may only be followed by an 'i' or a 'u'.
Each vowel 'u' may only be followed by an 'a'.
Since the answer may be too large, return it modulo 10^9 + 7.

 

Example 1:

Input: n = 1
Output: 5
Explanation: All possible strings are: "a", "e", "i" , "o" and "u".
Example 2:

Input: n = 2
Output: 10
Explanation: All possible strings are: "ae", "ea", "ei", "ia", "ie", "io", "iu", "oi", "ou" and "ua".
Example 3: 

Input: n = 5
Output: 68
 

Constraints:

1 <= n <= 2 * 10^4



Solution  : 

class Solution {

    final int MOD = (int)(1e9) + 7;
    public int countVowelPermutation(int n) {
        

        /***
        
            a -> 0
            e -> 1
            i -> 2
            o -> 3
            u -> 4
         */

        int[][] dp = new int[n][5];
        int a = 0;
        int e = 1;
        int i = 2;
        int o = 3;
        int u = 4;

        dp[0][a] = 1;
        dp[0][e] = 1;
        dp[0][i] = 1;
        dp[0][o] = 1;
        dp[0][u] = 1;

        for(int j = 1 ; j < n ; j ++ ) {

            dp[j][a] = ((dp[j - 1][e] + dp[j - 1][u]) % MOD + dp[j - 1][i]) % MOD;
            dp[j][e] = ( dp[j - 1][a] + dp[j - 1][i]) % MOD;
            dp[j][i] = ( dp[j - 1][e] + dp[j - 1][o]) % MOD;
            dp[j][o] = dp[j - 1][i];
            dp[j][u] = ( dp[j - 1][o] + dp[j - 1][i]) % MOD; 
        }


        int sum = 0;

        for(int j = 0 ; j < 5 ; j ++ ) {

            sum += dp[n - 1][j];
            sum %= MOD;
        }

        return sum;
    }
}
