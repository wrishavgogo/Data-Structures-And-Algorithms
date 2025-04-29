https://leetcode.com/problems/max-points-on-a-line/description/?envType=company&envId=google&favoriteSlug=google-six-months


Question : 

Given an array of points where points[i] = [xi, yi] represents a point on the X-Y plane, return the maximum number of points that lie on the same straight line.

 

Example 1:


Input: points = [[1,1],[2,2],[3,3]]
Output: 3
Example 2:


Input: points = [[1,1],[3,2],[5,3],[4,1],[2,3],[1,4]]
Output: 4
 

Constraints:

1 <= points.length <= 300
points[i].length == 2
-104 <= xi, yi <= 104
All the points are unique.


Soln : 

class Solution {

    


    public int maxPoints(int[][] points) {
        
        int n = points.length;

        int maxPoints = 1;

        for(int i = 0 ; i < n ; i ++ ) {

            Map<String , Integer > slopeToPointCountMap = new HashMap<>();
            int infSlope = 1;
            int zeroSlope = 1;
            for(int j = 0 ; j < n ; j ++ ) {

                if(j == i ) {
                    continue;
                }

                int x1 = points[i][0];
                int y1 = points[i][1];

                int x2 = points[j][0];
                int y2 = points[j][1];

                int diffx = x1 - x2;
                int diffy = y1 - y2;

                if(diffy == 0 ) {
                    infSlope++;
                    maxPoints = Math.max(maxPoints ,infSlope );
                } else if ( diffx == 0 ) {
                    zeroSlope++;
                    maxPoints = Math.max(maxPoints ,zeroSlope );
                } else {
                    
                    int gcd = getGcd(Math.abs(diffx) , Math.abs(diffy));
                    diffx /= gcd;
                    diffy /= gcd;
                    String key = String.valueOf(diffx) + "~" + String.valueOf(diffy);
                    slopeToPointCountMap.put(key , slopeToPointCountMap.getOrDefault(key , 0) + 1);
                    maxPoints = Math.max(1 + slopeToPointCountMap.get(key) ,maxPoints );
                }

            }
        }

        return maxPoints;
    }

    int getGcd(int x , int y ) {

        if(x == 0 ) {
            return y;
        }

        return getGcd(y % x , x);
    }
}

