Link : https://leetcode.com/problems/count-numbers-with-unique-digits-ii/description/?envType=problem-list-v2&envId=dynamic-programming

Question : 

Given two positive integers a and b, return the count of numbers having unique digits in the range [a, b] (inclusive).
 

Example 1:

Input: a = 1, b = 20
Output: 19
Explanation: All the numbers in the range [1, 20] have unique digits except 11. Hence, the answer is 19.
Example 2:

Input: a = 9, b = 19
Output: 10
Explanation: All the numbers in the range [9, 19] have unique digits except 11. Hence, the answer is 10. 
Example 3:

Input: a = 80, b = 120
Output: 27
Explanation: There are 41 numbers in the range [80, 120], 27 of which have unique digits.
 

Constraints:

1 <= a <= b <= 1000


Code :  

Read the explanation pdf that i have added in this folder , to understand what mistake i did and how did i correct it 

class Solution {

    final int MASK_SIZE = (int) (Math.pow(2 , 10) - 1 )   ;
    int[][][][] cache = new int[4][2][MASK_SIZE][2];

    public int numberCount(int a, int b) {
        
        initCache();
        String numsb = String.valueOf(b);
        int ansb = func(0 , 1 , 0 , numsb , 0 );

     
        initCache();
        String numsa = String.valueOf(a - 1);
        int ansa = func(0 , 1 , 0 , numsa , 0  );

        return ansb - ansa;
    }


    void initCache() {
        
        for(int i = 0 ; i < 4 ; i ++ ) {
            for(int j = 0 ; j < 2 ; j ++ ) {
                for(int k = 0 ; k < MASK_SIZE ; k ++ ) { 
                    for(int l = 0 ; l < 2 ; l ++ ) {
                        cache[i][j][k][l] = -1;
                    }  
                }
            }
        }
        
    }

    int func(int pos , int tight , int mask , String nums , int hasNumberStarted ) {

        if(pos == nums.length() ) {
            // valid number 
            //System.out.println(mask);
            return (hasNumberStarted == 1) ? 1 : 0;
        }

        if(cache[pos][tight][mask][hasNumberStarted] != -1 ) {
            return cache[pos][tight][mask][hasNumberStarted];
        }

        int limit = (tight == 1 ) ? ( nums.charAt(pos) - '0' ) : 9;
        int ans = 0;

        for(int i = 0 ; i <= limit ; i ++ ) {

            if(( mask & (1 << i)) != 0 ) {
                // digit already present 
                continue;
            }

            // digit not present
            int newMask = mask | (1 << i);
            int newTight = (tight == 1 && i == limit) ? 1 : 0;

            if(hasNumberStarted == 0 ) {
                
                    if(i == 0 ) {
                        // 00000 is continuing like this 
                        ans += func(pos + 1 , newTight , 0 , nums , hasNumberStarted );
                    } else {
                        ans += func(pos + 1 , newTight , newMask , nums , 1);
                    }
                
            } else {
                ans += func(pos + 1 , newTight , newMask , nums , hasNumberStarted);
            }
            
        }

        cache[pos][tight][mask][hasNumberStarted] = ans;

        return ans;

    }
}
