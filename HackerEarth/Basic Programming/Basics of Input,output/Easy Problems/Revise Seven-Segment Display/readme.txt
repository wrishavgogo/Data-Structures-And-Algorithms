Link : https://www.hackerearth.com/practice/basic-programming/input-output/basics-of-input-output/practice-problems/algorithm/seven-segment-display-nov-easy-e7f87ce0/

Question : 

Problem
You all must have seen a seven segment display.If not here it is:



Alice got a number written in seven segment format where each segment was creatted used a matchstick.

Example: If Alice gets a number 123 so basically Alice used 12 matchsticks for this number.

Alice is wondering what is the numerically largest value that she can generate by using at most the matchsticks that she currently possess.Help Alice out by telling her that number.

 

Input Format:

First line contains T (test cases).

Next T lines contain a Number N.

Output Format:

Print the largest possible number numerically that can be generated using at max that many number of matchsticks which was used to generate N.

Constraints:



Sample Input
2
1
0
Sample Output
1
111
Time Limit: 1
Memory Limit: 256
Source Limit:
Explanation
If you have 1 as your number that means you have 2 match sticks.You can generate 1 using this.

If you have 0 as your number that means you have 6 match sticks.You can generate 111 using this.



Code : 

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.*;
class TestClass {
    public static void main(String args[] ) throws Exception {
        
        Map<Integer , Integer > mp = new HashMap<>(){{
            put(0 , 6);
            put(1 , 2);
            put(2 , 5);
            put(3 , 5);
            put(4 , 4);
            put(5 , 5);
            put(6 , 6);
            put(7 , 3);
            put(8 , 7);
            put(9 , 6);
        }};

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int t = Integer.parseInt(br.readLine());
        while(t > 0 ) {
            t--;
            String s = br.readLine();
            int sum = 0;
            for(int i = 0 ; i < s.length() ; i ++ ) {
                sum += mp.get(s.charAt(i) - '0');
            }

            int rem = sum%2;
            sum /= 2;
            StringBuilder sb = new StringBuilder();

            while(sum > 0 ) {
                sum--;

                if(rem > 0 ) {
                    sb.append('7');
                    rem--;
                } else {
                    sb.append('1');
                }
                
            }

            System.out.println(sb.toString());
        }
    }
}
