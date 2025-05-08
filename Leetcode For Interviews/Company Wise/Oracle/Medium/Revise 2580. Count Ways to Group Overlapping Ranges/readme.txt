Link : https://leetcode.com/problems/count-ways-to-group-overlapping-ranges/description/?envType=company&envId=oracle&favoriteSlug=oracle-all

Question : 

You are given a 2D integer array ranges where ranges[i] = [starti, endi] denotes that all integers between starti and endi (both inclusive) are contained in the ith range.

You are to split ranges into two (possibly empty) groups such that:

Each range belongs to exactly one group.
Any two overlapping ranges must belong to the same group.
Two ranges are said to be overlapping if there exists at least one integer that is present in both ranges.

For example, [1, 3] and [2, 5] are overlapping because 2 and 3 occur in both ranges.
Return the total number of ways to split ranges into two groups. Since the answer may be very large, return it modulo 109 + 7.

 

Example 1:

Input: ranges = [[6,10],[5,15]]
Output: 2
Explanation: 
The two ranges are overlapping, so they must be in the same group.
Thus, there are two possible ways:
- Put both the ranges together in group 1.
- Put both the ranges together in group 2.
Example 2:

Input: ranges = [[1,3],[10,20],[2,5],[4,8]]
Output: 4
Explanation: 
Ranges [1,3], and [2,5] are overlapping. So, they must be in the same group.
Again, ranges [2,5] and [4,8] are also overlapping. So, they must also be in the same group. 
Thus, there are four possible ways to group them:
- All the ranges in group 1.
- All the ranges in group 2.
- Ranges [1,3], [2,5], and [4,8] in group 1 and [10,20] in group 2.
- Ranges [1,3], [2,5], and [4,8] in group 2 and [10,20] in group 1.
 

Constraints:

1 <= ranges.length <= 105
ranges[i].length == 2
0 <= starti <= endi <= 109


Code : 

class Solution {

    final int mod = (int) (1e9) + 7;
    public int countWays(int[][] ranges) {

        int n = ranges.length;
        int m = ranges[0].length;

        Integer[][] arr = new Integer[n][m];

        for(int i = 0 ; i < ranges.length ; i ++ ) {
            for(int j = 0 ; j < ranges[i].length ; j ++ ) {
                arr[i][j] = ranges[i][j];
            }
        }

        Arrays.sort(arr , ( Integer[] a , Integer[] b) -> {

            if(a[0] == b[0]) {
                return a[1] - b[1];
            }
            return a[0] - b[0];
        });

        
        int numberOfGroups = 1;
        int prevEnd = arr[0][1];
        for(int i = 1 ; i < n ; i ++ ) {

            if(prevEnd >= arr[i][0]) {
                // overLap
                prevEnd = Math.max(prevEnd , arr[i][1]);
            } else {
                numberOfGroups++;
                prevEnd = arr[i][1];
            }
        }

        //System.out.println(numberOfGroups);

        long ans= ( binaryExpo(2 , numberOfGroups , mod ) % mod) ;

        return (int) ans;
    }

    long binaryExpo(int a , int b  , int mod ) {

        System.out.println(b + " " + mod);
        if(b == 0 ) {
            return 1;
        }

        if(b % 2 == 0 ) {
            long val = binaryExpo(a , b/2 , mod);
            val = val % mod;
            return (val * val) % mod;
        } 

        return ( a * binaryExpo(a ,  b - 1 , mod )) % mod;
    }
}
