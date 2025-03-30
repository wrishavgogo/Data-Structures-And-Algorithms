Link : https://leetcode.com/problems/stone-game-ii/description/?envType=problem-list-v2&envId=dynamic-programming

Question : 

Alice and Bob continue their games with piles of stones. There are a number of piles arranged in a row, and each pile has a positive integer number of stones piles[i]. The objective of the game is to end with the most stones.

Alice and Bob take turns, with Alice starting first.

On each player's turn, that player can take all the stones in the first X remaining piles, where 1 <= X <= 2M. Then, we set M = max(M, X). Initially, M = 1.

The game continues until all the stones have been taken.

Assuming Alice and Bob play optimally, return the maximum number of stones Alice can get.

 

Example 1:

Input: piles = [2,7,9,4,4]

Output: 10

Explanation:

If Alice takes one pile at the beginning, Bob takes two piles, then Alice takes 2 piles again. Alice can get 2 + 4 + 4 = 10 stones in total.
If Alice takes two piles at the beginning, then Bob can take all three piles left. In this case, Alice get 2 + 7 = 9 stones in total.
So we return 10 since it's larger.

Example 2:

Input: piles = [1,2,3,4,5,100]

Output: 104

 

Constraints:

1 <= piles.length <= 100
1 <= piles[i] <= 104



Code :  Explanation in code , well some coders have done it using two states only and not 3 , have to learn how did they do so 

class Solution {
    final int MAX_START_POSITION = 101;
    final int MAX_TURN = 2;
    final int MAX_M = 202;
    int[][][] cache = new int[MAX_TURN][MAX_START_POSITION][MAX_M];

    void invalidateCache() {

        for(int i = 0 ; i < MAX_TURN ; i ++  ) {
            for(int j = 0 ; j < MAX_START_POSITION ; j ++ ) {
                for(int k = 0 ; k < MAX_M ; k ++ ) {
                    cache[i][j][k]  = -1;
                }
            }
        }
    }
    public int stoneGameII(int[] piles) {
        
        invalidateCache();
        return f(0 , 0 , 1 , piles);
    }

    /**
        0 -> represents alice's turn 
        1 -> represents bob's turn 

        startPosition -> represents (startPosition , n - 1 ) -> subarray to consider
        M is the constraint 
        all the stones in the first X remaining piles
        1 <= X <= 2M

        given these contraints how many stones can alice pick ? 
    * */
    int f(int turn , int startPosition , int M , int[] piles  ) {

        if(startPosition >= piles.length) {
            return 0;
        }

        if(cache[turn][startPosition][M] != -1 ) {
            return cache[turn][startPosition][M];
        }
        
        /**
             at startposition with m = M 
            whoever is playing , decided by turn 

            what is the max coins Alice can get when the game is starting like this 
        * */

        int stonesPickedUpAlice = 0;

        if(turn == 1 ) {
            // since we need to do Math.min compare
            stonesPickedUpAlice = Integer.MAX_VALUE;
        }
        else {
            // we need to do Math.max compare 
            stonesPickedUpAlice = 0;
        }

        int sum = 0;
        for(int i = startPosition ; i < piles.length ; i ++ ) {

            int len = i - startPosition + 1;
            if(len > 2*M) {
                // constraint of question , max 2*M coins can be picked
                break;
            }

            // whoever is playing will try to pick the maximum stones right ? 

            // lets say its bob's turn 

            if(turn == 1 ) {

                // he will play optimally so that he wins 
                // he will pick upto that number of stones so that alice can pick minimum as possible in her turn 

                // since this is bob's turn so from this subarray alice wont get anything 
                // next turn is her's
                stonesPickedUpAlice = Math.min(stonesPickedUpAlice , f(1 - turn , i + 1 , Math.max(M , len) , piles ));

            } else {

                // alice's turn so she will be able to pick stones in this subarray 

                sum += piles[i];
                stonesPickedUpAlice = Math.max(stonesPickedUpAlice , sum + f(1 - turn , i + 1 , Math.max(M , len) , piles ));
            }



        }

        cache[turn][startPosition][M] = stonesPickedUpAlice;
        return stonesPickedUpAlice;
    }
}
