Link : https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/

Question :

You are given an integer array prices where prices[i] is the price of a given stock on the ith day.

On each day, you may decide to buy and/or sell the stock. You can only hold at most one share of the stock at any time. However, you can buy it then immediately sell it on the same day.

Find and return the maximum profit you can achieve.

 

Example 1:

Input: prices = [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
Total profit is 4 + 3 = 7.
Example 2:

Input: prices = [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
Total profit is 4.
Example 3:

Input: prices = [7,6,4,3,1]
Output: 0
Explanation: There is no way to make a positive profit, so we never buy the stock to achieve the maximum profit of 0.
 

Constraints:

1 <= prices.length <= 3 * 104
0 <= prices[i] <= 104


Solution : Go check in the easy section of dynamic programming for the break down 
          also the code has comments as well 

// in this problem you can buy and sell multiple times 
        // buy cannot buy before selling because atmost 1 stock


class Solution {
    const static int siz = 1e5 + 1;
    int dp[siz][2];
    // dp[i][j] returns me the maximum profit i can get , if am standing at index i , and have choice j = 0 , 1 
public:
    int maxProfit(vector<int>& prices) {
        memset(dp , -1 , sizeof(dp));
        return helper(1 , 0 , prices);
    }
    
    int helper(int canbuy , int idx , vector<int>& prices) {
        // helper(canbuy , idx) --> maximum profit i can get if am standing on idx , with my current canBuy status 
        // helper returns me that standing on index , idx of prices and given the choice
        // canbuy = 0 , means i have a stock in hand and cannot buy and have to either sell or not see
        // canbuy = 1 , means i dont have stock  in head and can buy or not buy 
        
        
        
        // base case 
        if(idx >= prices.size()) {
            // no stocks left
            return 0;
        }
        
        if(dp[idx][canbuy] != -1) {
            // solution has already been calculated 
            return dp[idx][canbuy];
        }
        
        if(canbuy != 1) {
            // canbuy = 0 means you cannot buy 
            
            // i want to sell this index
            int sell = +prices[idx] + helper(1 , idx + 1 , prices);
            // i dont want to sell this index
            int dontsell = helper(0 , idx + 1 , prices);
            
            // cout<<"selling\n\n";
            // cout<<"sell--> "<<sell<<" dontsell--> "<<dontsell<<"\n";
            return dp[idx][canbuy] = max(sell , dontsell);
        }
        else 
        {
            // canbuy = 1 means you can by
            
            // i want to buy this index
            int buy = -prices[idx] + helper(0 , idx + 1 , prices);
            // i dont want to buy
            int dontbuy = helper(1 , idx + 1 , prices);
            
            
            // cout<<"buying\n\n";
            // cout<<"buy--> "<<buy<<" dontbuy---> "<<dontbuy<<"\n";
            return dp[idx][canbuy] = max(buy , dontbuy);
        }
        
        return 0;
    }
};
