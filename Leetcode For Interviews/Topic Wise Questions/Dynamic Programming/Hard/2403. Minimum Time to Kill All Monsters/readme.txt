Link : https://leetcode.com/problems/minimum-time-to-kill-all-monsters/description/?envType=problem-list-v2&envId=dynamic-programming


Todo : think how to reduce state to 1 dimension ,  only keep bitmask as the state need to think about it 


Question : 

You are given an integer array power where power[i] is the power of the ith monster.

You start with 0 mana points, and each day you increase your mana points by gain where gain initially is equal to 1.

Each day, after gaining gain mana, you can defeat a monster if your mana points are greater than or equal to the power of that monster. When you defeat a monster:

your mana points will be reset to 0, and
the value of gain increases by 1.
Return the minimum number of days needed to defeat all the monsters.

 

Example 1:

Input: power = [3,1,4]
Output: 4
Explanation: The optimal way to beat all the monsters is to:
- Day 1: Gain 1 mana point to get a total of 1 mana point. Spend all mana points to kill the 2nd monster.
- Day 2: Gain 2 mana points to get a total of 2 mana points.
- Day 3: Gain 2 mana points to get a total of 4 mana points. Spend all mana points to kill the 3rd monster.
- Day 4: Gain 3 mana points to get a total of 3 mana points. Spend all mana points to kill the 1st monster.
It can be proven that 4 is the minimum number of days needed. 
Example 2:

Input: power = [1,1,4]
Output: 4
Explanation: The optimal way to beat all the monsters is to:
- Day 1: Gain 1 mana point to get a total of 1 mana point. Spend all mana points to kill the 1st monster.
- Day 2: Gain 2 mana points to get a total of 2 mana points. Spend all mana points to kill the 2nd monster.
- Day 3: Gain 3 mana points to get a total of 3 mana points.
- Day 4: Gain 3 mana points to get a total of 6 mana points. Spend all mana points to kill the 3rd monster.
It can be proven that 4 is the minimum number of days needed. 
Example 3:

Input: power = [1,2,4,9]
Output: 6
Explanation: The optimal way to beat all the monsters is to:
- Day 1: Gain 1 mana point to get a total of 1 mana point. Spend all mana points to kill the 1st monster.
- Day 2: Gain 2 mana points to get a total of 2 mana points. Spend all mana points to kill the 2nd monster.
- Day 3: Gain 3 mana points to get a total of 3 mana points.
- Day 4: Gain 3 mana points to get a total of 6 mana points.
- Day 5: Gain 3 mana points to get a total of 9 mana points. Spend all mana points to kill the 4th monster.
- Day 6: Gain 4 mana points to get a total of 4 mana points. Spend all mana points to kill the 3rd monster.
It can be proven that 6 is the minimum number of days needed.
 

Constraints:

1 <= power.length <= 17
1 <= power[i] <= 109



Soln : 

Explanation : at any instant we can choose which monster we want to kill and we maintain a bitmask to say that this monster has been killed or not killed 
if not then pick it and kill it and update gain 

class Solution {

    final int MAX_MASK_VALUE = (int) Math.pow(2 , 17) ;
    final int MAX_GAIN_VALUE = 19; // gain can go to 1 to 1 + 17 = 18 
    long[][] cache = new long[MAX_MASK_VALUE][MAX_GAIN_VALUE];

    public long minimumTime(int[] power) {
        
        int noOfMonsters = power.length;
        int maskOfMonsters =(int) Math.pow(2 , noOfMonsters) - 1 ;
        invalidateCache();
        // initial gain is zero 
        return f(maskOfMonsters , 1 , power );
    }

    private void invalidateCache() {

        for(int i = 0 ; i < MAX_MASK_VALUE ; i ++  ) {
            for(int j = 0 ; j < MAX_GAIN_VALUE ; j ++  ) {
                cache[i][j] = -1;
            }
        }
    }


    long f(int mask , int gain  , int[] power) {

        if(mask == 0 ) {
            // all monsters are killed 
            return 0;
        }

        if(cache[mask][gain] != -1 ) {
            return cache[mask][gain] ;
        }


        int noOfMonsters = power.length;
        long ans = Long.MAX_VALUE;
        // out of all the monsters decide which one to kill

        for(int i = 0 ; i < noOfMonsters ; i ++ ) {

            if ( ((mask >> i) & 1)  == 1 ) {
                // here we decide which monster to kill 
                // check if ith monster is alive

                // mark this monster dead and flip the bit 
                int newMask = mask ^ (1 << i);
                ans = Math.min(ans , minDaysToKillMonster(power[i] , gain) + f(newMask , gain + 1 , power) );
            }
        }

        cache[mask][gain] = ans;
        return ans;

    }

    


    int minDaysToKillMonster(int powerOfMonster , int gain ) {

        int days = powerOfMonster/gain;
        if(powerOfMonster % gain != 0 ) {
            days++;
        }
        return days;
    }
}
