Link : https://leetcode.com/problems/climbing-stairs/description/?envType=problem-list-v2&envId=dynamic-programming

Question : 

You are climbing a staircase. It takes n steps to reach the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

 

Example 1:

Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
Example 2:

Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
 

Constraints:

1 <= n <= 45


Soln : We wrote a space optimised solution where we are storing the l1 and l2 , where l1  = dp[i - 2] and l2 = dp[i - 1] , 
and we keep recalulating those and at any given n , l2 is the answer 

so we return l2 

code /;

class Solution {
    public int climbStairs(int n) {
        
        // dp[i] tells me how many ways i can reach index i 

        // int[] dp = new int[n + 1];
        // // consider 0 as the first step 
        // dp[1] = 1; // 1 step to reach 1
        // dp[2] = 2;  // 2 step jump to reach 2 , 1 step 1 step jump

        // for(int i = 3 ; i <= n ; i ++ ) {

        //     dp[i] = dp[i - 1] + dp[i - 2];
        // }

        // return dp[n];


        // we can even do it without storing it in the array 

        
        int f1 = 1;
        int f2 = 2;

        if(n == 1 ) {
            return f1;
        }

        if(n == 2 ) {
            return f2;
        }


        int newf1 = f1;
        int newf2 = f2;
        n -= 2; // 2 steps are already covered
        
        while(n > 0 ) {
            int k = newf1 + newf2;
            newf1 = newf2;
            newf2 = k;
            //System.out.println(newf1 + " " + newf2 + " "  + k + " " + n);
            n--;
        }

        return newf2;

    }
}
