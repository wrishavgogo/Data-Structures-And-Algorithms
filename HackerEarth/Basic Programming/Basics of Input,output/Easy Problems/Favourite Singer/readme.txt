Link : https://www.hackerearth.com/practice/basic-programming/input-output/basics-of-input-output/practice-problems/algorithm/favourite-singer-a18e086a/

Slowly gaining confidence in Input/output
Question : 

Problem
Bob has a playlist of 
 songs, each song has a singer associated with it (denoted by an integer)

Favourite singer of Bob is the one whose songs are the most on the playlist

Count the number of Favourite Singers of Bob

Input Format 

The first line contains an integer 
, denoting the number of songs in Bob's playlist.

The following input contains 
 integers, 
 integer denoting the singer of the 
 song.

Output Format

Output a single integer, the number of favourite singers of Bob

Note: Use 64 bit data type

Constraints


Sample Input
5
1 1 2 2 4
Sample Output
2



Code : 

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.*;
class TestClass {
    public static void main(String args[] ) throws Exception {
        
         BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int n = Integer.parseInt(br.readLine());

        String inputStr = br.readLine();
        String[] strs = inputStr.split(" ");

        Map<String , Integer> mp = new HashMap<>();
        int maxFreq = 0;

        for(int i = 0 ; i < strs.length ; i ++ ) { 

            mp.put(strs[i] , mp.getOrDefault(strs[i] , 0) + 1);
            maxFreq = Math.max(maxFreq , mp.get(strs[i]));
        }

        int ans = 0 ;

        for(Map.Entry<String , Integer> entry : mp.entrySet()) {

            if(entry.getValue() == maxFreq) {
                ans++;
            }
        }

        System.out.println(ans);

    }
}
