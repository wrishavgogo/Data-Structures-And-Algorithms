Link : https://www.hackerearth.com/practice/basic-programming/input-output/basics-of-input-output/practice-problems/algorithm/is-zoo-f6f309e7/


Question : 

Problem
You are required to enter a word that consists of 
 and 
 that denote the number of Zs and Os respectively. The input word is considered similar to word zoo if 
.

Determine if the entered word is similar to word zoo.

For example, words such as zzoooo and zzzoooooo are similar to word zoo but not the words such as zzooo and zzzooooo.

Input format

First line: A word that starts with several Zs and continues by several Os.
Note: The maximum length of this word must be 
.
Output format

Print Yes if the input word can be considered as the string zoo otherwise, print No.

Sample Input
zzzoooooo
Sample Output
Yes


Code : 

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.*;
class TestClass {
    public static void main(String args[] ) throws Exception {
        
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        String str = br.readLine();
        char[] arr = str.toCharArray();

        int x = 0;
        int y = 0;

        for(int i = 0 ; i < arr.length ; i ++ ) {

            if(arr[i] == 'z') {
                x++;
            }

            if(arr[i] == 'o') {
                y++;
            }
        }

        if(2*x == y ) {
            System.out.println("Yes");
        } else {
            System.out.println("No");
        }
    }
}
