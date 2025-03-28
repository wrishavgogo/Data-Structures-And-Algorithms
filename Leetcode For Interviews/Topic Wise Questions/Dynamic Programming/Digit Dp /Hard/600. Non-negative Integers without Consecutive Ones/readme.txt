Link  : https://leetcode.com/problems/non-negative-integers-without-consecutive-ones/description/


Question : 

Given a positive integer n, return the number of the integers in the range [0, n] whose binary representations do not contain consecutive ones.

 

Example 1:

Input: n = 5
Output: 5
Explanation:
Here are the non-negative integers <= 5 with their corresponding binary representations:
0 : 0
1 : 1
2 : 10
3 : 11
4 : 100
5 : 101
Among them, only integer 3 disobeys the rule (two consecutive ones) and the other 5 satisfy the rule. 
Example 2:

Input: n = 1
Output: 2
Example 3:

Input: n = 2
Output: 3
 

Constraints:

1 <= n <= 109



Soln :  -> Later have to note down the normal dp solns as well 

I ended up doing a normal DP problem using digit dp haha 


Idea : 
But for now i will brief you what i did 
we have n right ? convert that into binary string 
then you have the nums String which you traverse over in digit dp 
from 0 to length
in normal nums we have limit from 0 to 9 
in this one we have limit from 0 to 1 thats it nothing different 
and here we had to follow a constraint that no consecutive ones 
so we just had to keep a state previousOne , it would be 1 if previousBit chosen by us was 1 or else it would be 0 


  Code

class Solution {

    
    int[][][] cache;
    
    String convertToBinaryRep(int n ) {

        StringBuilder sb = new StringBuilder();

        while(n != 0 ) {

            int modulo = n % 2;
            sb.append(String.valueOf(modulo));
            n = n/2;
        }

        sb.reverse();
        return sb.toString();
    }

    void invalidateCache(final int maxPosSize , final int maxTightSize , final int maxPreviousWasOne ) {

        cache = new int[maxPosSize][maxTightSize][maxPreviousWasOne];

        for(int i = 0 ; i < maxPosSize ; i ++ ) {
            for(int j = 0 ; j < maxTightSize ; j ++ ) {
                for(int k = 0 ; k < maxPreviousWasOne ; k ++ ) {
                    cache[i][j][k] = -1;
                }
            }
        }
        
    }

    public int findIntegers(int n) {
        
        String binaryRep = convertToBinaryRep(n);

        invalidateCache(binaryRep.length() , 2 , 2 );
        return f(0 , 1 , 0 , binaryRep);
    }


    // since 0 is included hence we need not worry abt it being included in the answer 
    int f(int pos , int tight , int previousWasOne , String binaryRep) {

        if(pos == binaryRep.length()) {
            // valid string we reached 
            return 1;
        }

        if(cache[pos][tight][previousWasOne] != -1 ) {
            return cache[pos][tight][previousWasOne];
        }

        int limit = (tight == 1 ) ? binaryRep.charAt(pos) - '0' : 1;
        int ans = 0;
        for(int i = 0 ; i <= limit ; i ++ ) {

            int newTight = (tight == 1 && i == limit) ? 1 : 0;

            if(previousWasOne == 1 ) {
                // previous bit was 1 , so this has to be zero 
                if(i == 1 ) {
                    // cannot allow next bit to be 1 as well
                    continue;
                }
                else {
                    // next bit is 0 
                    ans += f(pos + 1 , newTight , 0 , binaryRep ) ;
                }
            } else {
                // we can put anything if previous bit is not 1 
                ans += f(pos + 1 , newTight , i == 1 ? 1 : 0 , binaryRep );
            } 
        }

        cache[pos][tight][previousWasOne] = ans;
        return ans;
    }


}

