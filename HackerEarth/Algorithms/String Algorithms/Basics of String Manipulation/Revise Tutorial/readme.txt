Link : https://www.hackerearth.com/practice/algorithms/string-algorithm/basics-of-string-manipulation/tutorial/

Question : 

Given a string S, and two numbers N, M - arrange the characters of string in between the indexes N and M (both inclusive) in descending order. (Indexing starts from 0).

Input Format:
First line contains T - number of test cases.
Next T lines contains a string(S) and two numbers(N, M) separated by spaces.

Output Format:
Print the modified string for each test case in new line.

Constraints:

 // |S| denotes the length of string.
SAMPLE INPUT 
3
hlleo 1 3
ooneefspd 0 8
effort 1 4
SAMPLE OUTPUT 
hlleo
spoonfeed
erofft


Code : 


import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.*;
class TestClass {
    public static void main(String args[] ) throws Exception {
        //BufferedReader

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int t = Integer.parseInt(br.readLine());

        while(t > 0 ) {
            
            t--;
            String inputStr = br.readLine();
            String[] splitInput = inputStr.split(" ");

            String s = splitInput[0];
            int n = Integer.parseInt(splitInput[1]);
            int m = Integer.parseInt(splitInput[2]);

            String subStr = s.substring(n , m + 1);
            char[] arr = subStr.toCharArray();

            Arrays.sort(arr);
            char[] actual = s.toCharArray();
            for(int i = 0 ; i <  actual.length ; i ++ ) {
                if(i >= n && i <= m ) {
                    // in range
                    actual[i] = arr[arr.length - 1 - (i - n)];
                }
            }

            String ans = new String(actual);
            System.out.println(ans);

        }
    }
}
