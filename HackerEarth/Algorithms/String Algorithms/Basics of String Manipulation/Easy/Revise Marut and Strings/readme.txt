Link : https://www.hackerearth.com/practice/algorithms/string-algorithm/basics-of-string-manipulation/practice-problems/algorithm/marut-and-strings-4/

Question : 

Marut loves good strings. According to him, good strings are those which contain either all alphabets of uppercase or lowercase. While he is preparing for his exams, he finds many bad strings in his book and wants to convert them to good strings. But he wants to do this in minimum number of operations.
In one operation, he can pick only one character of any case and convert it to any other case.

As his exams are going on, he wants your help.

Input:
The first line contains an integer T, denoting the number of test cases.
Each test case consists of only one line with a string S which contains uppercase and lowercase alphabets..

Output:
Print the minimum number of operations, in which Marut can obtain a good string.
Print the answer for each test case in new line.

Constraints:
1 ≤ T ≤ 10
If T is not in this range, print "Invalid Test" (without the quotes)
1 ≤ Length of S ≤ 100
S can contain numbers, special characters but no spaces.
If the length of string is not in the above range or it does not contain any alphabets, print "Invalid Input" (without the quotes)

For Example, if the input is:
0
TestString

Print "Invalid Test" (without the quotes)

Sample Input
3
abcEfg
!@6#2
123A
Sample Output
1
Invalid Input
0


Code : 

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.*;
class TestClass {
    public static void main(String args[] ) throws Exception {
        
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int n = Integer.parseInt(br.readLine());

        while(n > 0) {
            n--;
            String s = br.readLine();
            int ans = 0;
            int l = 0;
            int u = 0;
            for(int i = 0 ; i < n ; i ++ ) {   
                char c = s.charAt(i);
                if(!Character.isLetter(c)) {
                    ans = -1;
                    break;
                }
                if(Character.isUpperCase(c)) {
                    u++;
                } else {
                    l++;
                }
            }

            if(ans == -1 ) {
                System.out.println("Invalid Input");
            }

            ans = Math.min(l , u);
            System.out.println(ans);

            

            
        }
    }
}
