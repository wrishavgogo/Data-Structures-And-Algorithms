Link : https://www.hackerearth.com/practice/basic-programming/input-output/basics-of-input-output/practice-problems/algorithm/maximum-border-9767e14c/

Question : 

You are given a table with 
 rows and 
 columns. Each cell is colored with white or black. Considering the shapes created by black cells, what is the maximum border of these shapes? Border of a shape means the maximum number of consecutive black cells in any row or column without any white cell in between.

A shape is a set of connected cells. Two cells are connected if they share an edge. Note that no shape has a hole in it.

Input format

The first line contains 
 denoting the number of test cases.
The first line of each test case contains integers 
 denoting the number of rows and columns of the matrix. Here, '#' represents a black cell and '.' represents a white cell. 
Each of the next 
 lines contains 
 integers.
Output format

Print the maximum border of the shapes.

Sample Input
10
2 15
.....####......
.....#.........
7 9
...###...
...###...
..#......
.####....
..#......
...#####.
.........
18 11
.#########.
########...
.........#.
####.......
.....#####.
.....##....
....#####..
.....####..
..###......
......#....
....#####..
...####....
##.........
#####......
....#####..
....##.....
.#######...
.#.........
1 15
.....######....
5 11
..#####....
.#######...
......#....
....#####..
...#####...
8 13
.....######..
......##.....
########.....
...#.........
.............
#######......
..######.....
####.........
7 5
.....
..##.
###..
..##.
.....
..#..
.#...
14 2
..
#.
..
#.
..
#.
..
..
#.
..
..
..
#.
..
7 15
.###########...
##############.
...####........
...##########..
.......#.......
.....#########.
.#######.......
12 6
#####.
###...
#.....
##....
###...
......
.##...
..##..
...#..
..#...
#####.
####..
Sample Output
4
5
9
6
7
8
3
1
14
5


Code :  

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.*;
class TestClass {
    public static void main(String args[] ) throws Exception {
        //BufferedReader
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int t = Integer.parseInt(br.readLine());
        
        while( t > 0 ) {
            int maxBorder = 0;
            t--;
            String str = br.readLine();
            String[] strSplit = str.split(" ");
            int n = Integer.parseInt(strSplit[0]);
            int m = Integer.parseInt(strSplit[1]);

            char[][] ans = new char[n][m];

            for(int i = 0 ; i < n ; i ++ ) {
                String row = br.readLine();
                ans[i] = row.toCharArray();
            }  

            // first lets travel row wise 
            for(int i = 0 ; i < n ; i ++ ) {
                int rowSum = 0;
                for(int j = 0 ; j < m ; j ++ ) {
                    if(ans[i][j] == '#') {
                        rowSum++;
                    } else {
                        maxBorder = Math.max(maxBorder , rowSum);
                        rowSum = 0;
                    }
                }

                maxBorder = Math.max(maxBorder , rowSum);
            }


            // second lets travel col wise

            for(int j = 0 ; j < m ; j ++ ) {
                int colSum = 0;
                for(int i = 0 ; i < n ; i ++ ) {

                    if(ans[i][j] == '#') {
                        colSum++;
                    } else {
                        maxBorder = Math.max(maxBorder , colSum);
                        colSum = 0;
                    }
                }

                maxBorder = Math.max(maxBorder , colSum);
            } 

            System.out.println(maxBorder);
        }

    }
}
