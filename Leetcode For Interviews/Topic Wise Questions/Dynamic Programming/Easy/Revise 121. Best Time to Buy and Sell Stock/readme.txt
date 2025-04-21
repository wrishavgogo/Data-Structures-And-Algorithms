Link : https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/?envType=problem-list-v2&envId=dynamic-programming

Question : 

You are given an array prices where prices[i] is the price of a given stock on the ith day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

 

Example 1:

Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.
Example 2:

Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transactions are done and the max profit = 0.
 

Constraints:

1 <= prices.length <= 105
0 <= prices[i] <= 104


soln : Naive solution which came to my mind 

    You just have to find a pair i , j such that 
        i < j and arr[j] - arr[i] is maximum , basically selling in future you want to maximize your sell value 
        so for any index i  , you have to find the right maximum for it 


Intuition
You just have to find a pair i , j such that
i < j and arr[j] - arr[i] is maximum

Approach
travel from right to left keeping a track of maximum and just subtract
with current index value to find answer

Complexity
Time complexity:
O(n)

Space complexity:
O(1)

Code
class Solution {
    public int maxProfit(int[] prices) {
        

        int ans = 0 ;
        int n = prices.length;
        int maxVal = prices[n - 1];

        for(int i = n - 2 ; i >= 0 ; i -- ) {

            ans = Math.max(ans , maxVal - prices[i]);
            maxVal = Math.max(maxVal , prices[i]);
        }

        return ans;
    }
}





DP solution   :This i wrote to build the thought process of the solutions 


i used a variable , 
                if a person has not bought the stock the person has two options standing at any given index 
                          1. buy the stock   ----> int buy = -prices[index] + maxProfit(index + 1 , stocksRemainingToBuy - 1 , prices);
                          2. dont buy the stock and wait in future for buying the stock ----> int dontBuy = maxProfit(index + 1 , stocksRemainingToBuy , prices);

                if a person has bought the stock then at any index he has two options 
                          1. sell the stock and end it for all ----> int sell = prices[index];
                          2. dont sell the stock and wait for next option to sell the stock   ----> int dontSell = maxProfit(index + 1 , stocksRemainingToBuy , prices);


class Solution {

    int[][] cache = new int[100001][2];

    public int maxProfit(int[] prices) {
        initCache();
        return maxProfit(0 , 1 , prices);
    }

    void initCache() {

        for(int i = 0 ; i < 100001 ; i ++ ) {
            for(int j = 0 ; j < 2 ; j ++ ) {
                cache[i][j] = -1;
            }
        }
    }

    int maxProfit(int index , int stocksRemainingToBuy  , int[] prices ) {

        if(index >= prices.length ) {
            return 0;
        }

        if(cache[index][stocksRemainingToBuy] != -1 ) {
            return cache[index][stocksRemainingToBuy];
        }

        if(stocksRemainingToBuy == 0 ) {
            // stock is already bought , two choices

            //  i sell 
            int sell = prices[index];

            // i dont sell and wait in future to sell
            int dontSell = maxProfit(index + 1 , stocksRemainingToBuy , prices);

            cache[index][stocksRemainingToBuy] = Math.max(sell , dontSell);

        } else {

            // i have not bought the stock yet man 

            // so i buy stock today
            int buy = -prices[index] + maxProfit(index + 1 , stocksRemainingToBuy - 1 , prices);

            // i dont buy stock today
            int dontBuy = maxProfit(index + 1 , stocksRemainingToBuy , prices);

            cache[index][stocksRemainingToBuy] = Math.max(buy , dontBuy);
        }

        return cache[index][stocksRemainingToBuy];
    }
}
