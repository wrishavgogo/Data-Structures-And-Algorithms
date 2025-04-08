Link : https://leetcode.com/problems/maximum-performance-of-a-team/description/?envType=company&envId=amazon&favoriteSlug=amazon-three-months

Question : 
You are given two integers n and k and two integer arrays speed and efficiency both of length n. There are n engineers numbered from 1 to n. speed[i] and efficiency[i] represent the speed and efficiency of the ith engineer respectively.

Choose at most k different engineers out of the n engineers to form a team with the maximum performance.

The performance of a team is the sum of its engineers' speeds multiplied by the minimum efficiency among its engineers.

Return the maximum performance of this team. Since the answer can be a huge number, return it modulo 109 + 7.

 

Example 1:

Input: n = 6, speed = [2,10,3,1,5,8], efficiency = [5,4,3,9,7,2], k = 2
Output: 60
Explanation: 
We have the maximum performance of the team by selecting engineer 2 (with speed=10 and efficiency=4) and engineer 5 (with speed=5 and efficiency=7). That is, performance = (10 + 5) * min(4, 7) = 60.
Example 2:

Input: n = 6, speed = [2,10,3,1,5,8], efficiency = [5,4,3,9,7,2], k = 3
Output: 68
Explanation:
This is the same example as the first but k = 3. We can select engineer 1, engineer 2 and engineer 5 to get the maximum performance of the team. That is, performance = (2 + 10 + 5) * min(5, 4, 7) = 68.
Example 3:

Input: n = 6, speed = [2,10,3,1,5,8], efficiency = [5,4,3,9,7,2], k = 4
Output: 72
 

Constraints:

1 <= k <= n <= 105
speed.length == n
efficiency.length == n
1 <= speed[i] <= 105
1 <= efficiency[i] <= 108


Code : Read the explanation carefully for step by step explanation of your thought process 
how your brain thought and how you understood the solution 


class Solution {

    class Engineer {

        int speed;
        int efficiency;

        public Engineer(int speed , int efficiency ) {
            this.speed = speed;
            this.efficiency = efficiency;
        }
    }

    final int mod = (int)(1e9 + 7);

    public int maxPerformance(int n, int[] speed, int[] efficiency, int k) {
        
        List<Engineer> engineers = new ArrayList<>();
        for(int i = 0 ; i < n ; i ++ ) {
            engineers.add(new Engineer(speed[i] , efficiency[i]));
        }

        engineers.sort((Engineer a , Engineer b) -> {
            // sorting the engineers by their efficiency
            // max efficiency at the top
            return b.efficiency - a.efficiency;
        });

        // k max number of engineers to be hired

        // min speeds at the top
        // and we want to keep k speeds at max  -> top k maximum speeds
        // so compare with heap.top which is the minimum of all 
        // new speed greater than heap.top then remove the heap.top and insert the new speed
        PriorityQueue<Integer> speedQueue = new PriorityQueue<Integer>((Integer a, Integer b ) -> {
            return a - b;
        });

        long sumOfSpeedsTillNow = 0;
        long ans = 0;

        for(int i = 0 ; i < engineers.size() ; i ++  ) {
            
            
            // since we are travelling in descending order of efficiency
            // so efficiencies greater than this one wont matter as we want min efficiency
            int eff = engineers.get(i).efficiency;
            int spd = engineers.get(i).speed;
            //System.out.print(eff + " " + spd + " ----> ");
            long curAns;
            if(speedQueue.size() < k ) {
                // fill it up till size k 
                sumOfSpeedsTillNow += (long)spd;
                speedQueue.offer(spd);
                curAns = (long)sumOfSpeedsTillNow * (long)eff;
                ans = Math.max(ans , curAns);
            } else {

                // pop the min speed 
                sumOfSpeedsTillNow -= speedQueue.poll();
                // insert the new speed 
                sumOfSpeedsTillNow += (long)spd;
                speedQueue.offer(spd);

                // calculate answer with this engineer as minimum efficiency 
                curAns = (long)sumOfSpeedsTillNow * (long)eff;
                ans = Math.max(ans , curAns);
            }

            //System.out.println(ans + " " + curAns);

        }

        long val = ans % (long)mod;
        return (int) val;
        
    }
}
