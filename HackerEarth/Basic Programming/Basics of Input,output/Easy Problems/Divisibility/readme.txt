Link : https://www.hackerearth.com/practice/basic-programming/input-output/basics-of-input-output/practice-problems/algorithm/divisible-or-not-81b86ad7/

Question : 

Problem
You are provided an array 
 of size 
 that contains non-negative integers. Your task is to determine whether the number that is formed by selecting the last digit of all the N numbers is divisible by 
.

Note: View the sample explanation section for more clarification.

Input format

First line: A single integer 
 denoting the size of array 
Second line: 
 space-separated integers.
Output format

If the number is divisible by 
, then print 
. Otherwise, print 
.

Constraints

Sample Input
5
85 25 65 21 84
Sample Output
No


Code : 

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.*;
class TestClass {
    public static void main(String args[] ) throws Exception {
        
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());

        String s = br.readLine();
        String[] str = s.split(" ");
        int rem = 0;

        for(int i = 0 ; i < n ; i ++ ) {

            int x = Integer.parseInt(str[i]);
            rem = ( rem * 10 + x ) % 10;
        }

        if(rem == 0 ) {
            System.out.println("Yes");
        } else {
            System.out.println("No");
        }
    }    

}
