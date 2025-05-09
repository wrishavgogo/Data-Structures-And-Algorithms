Link : https://leetcode.com/problems/minimum-costs-using-the-train-line/description/?envType=problem-list-v2&envId=dynamic-programming

Question : 
A train line going through a city has two routes, the regular route and the express route. Both routes go through the same n + 1 stops labeled from 0 to n. Initially, you start on the regular route at stop 0.

You are given two 1-indexed integer arrays regular and express, both of length n. regular[i] describes the cost it takes to go from stop i - 1 to stop i using the regular route, and express[i] describes the cost it takes to go from stop i - 1 to stop i using the express route.

You are also given an integer expressCost which represents the cost to transfer from the regular route to the express route.

Note that:

There is no cost to transfer from the express route back to the regular route.
You pay expressCost every time you transfer from the regular route to the express route.
There is no extra cost to stay on the express route.
Return a 1-indexed array costs of length n, where costs[i] is the minimum cost to reach stop i from stop 0.

Note that a stop can be counted as reached from either route.

 

Example 1:


Input: regular = [1,6,9,5], express = [5,2,3,10], expressCost = 8
Output: [1,7,14,19]
Explanation: The diagram above shows how to reach stop 4 from stop 0 with minimum cost.
- Take the regular route from stop 0 to stop 1, costing 1.
- Take the express route from stop 1 to stop 2, costing 8 + 2 = 10.
- Take the express route from stop 2 to stop 3, costing 3.
- Take the regular route from stop 3 to stop 4, costing 5.
The total cost is 1 + 10 + 3 + 5 = 19.
Note that a different route could be taken to reach the other stops with minimum cost.
Example 2:


Input: regular = [11,5,13], express = [7,10,6], expressCost = 3
Output: [10,15,24]
Explanation: The diagram above shows how to reach stop 3 from stop 0 with minimum cost.
- Take the express route from stop 0 to stop 1, costing 3 + 7 = 10.
- Take the regular route from stop 1 to stop 2, costing 5.
- Take the express route from stop 2 to stop 3, costing 3 + 6 = 9.
The total cost is 10 + 5 + 9 = 24.
Note that the expressCost is paid again to transfer back to the express route.
 

Constraints:

n == regular.length == express.length
1 <= n <= 105
1 <= regular[i], express[i], expressCost <= 105


Code : 

class Solution {
    public long[] minimumCosts(int[] regular, int[] express, int expressCost) {
        

        // consider 0 based to be 1 based indexing 
        // i.e i = 0 represents station1 
        int totalStations = regular.length;
        int totalRoutes = 2;
        long[][] cost = new long[totalStations][totalRoutes];
        long[] ans = new long[totalStations];

        /***
            cost[i][j] represents what minimum cost did i arrive to station i using route j 
            here route j is 0 or 1 
         */
        // 0 is regular route , 1 is express route 

        // to reach station 1 in regular route 
        cost[0][0] = regular[0];
        // to reach station 1 in express route 
        // first change train and then travel in express
        cost[0][1] = expressCost + express[0];
        ans[0] = Math.min(cost[0][0] , cost[0][1] );

        for(int i = 1 ; i < totalStations ; i ++ ) {

            // i came to the ith station using regular train 
            // two ways i can come there 
            // 1. i was on the regular train previously and just came to the next stop using that
            // 2. i was on express train in last stop , changed to regular and came here
            long previouslyOnRegularTrain =  cost[i - 1][0] + regular[i];
            long previouslyOnExpressTrain =  cost[i - 1][1] + regular[i]; // no cost to change from express to regular 
            cost[i][0] = Math.min(previouslyOnRegularTrain , previouslyOnExpressTrain);



             // i came to the ith station using express train 
            // two ways i can come there 
            // 1. i was on the express train previously and just came to the next stop using that
            // 2. i was on regular train in last stop , changed to express  and came here
            previouslyOnExpressTrain =  cost[i - 1][1] + express[i];
            previouslyOnRegularTrain =  cost[i - 1][0] + expressCost + express[i]; // cost to change from regular to express  
            cost[i][1] = Math.min(previouslyOnRegularTrain , previouslyOnExpressTrain);

            ans[i] = Math.min(cost[i][0] , cost[i][1]);

        }


        return ans;
    }

    
}
