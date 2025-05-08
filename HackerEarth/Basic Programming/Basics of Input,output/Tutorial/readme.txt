In hackerearth we have a tutorial section which helps us in getting fair understanding abt the problem

Link : https://www.hackerearth.com/practice/basic-programming/input-output/basics-of-input-output/tutorial/

Question : 

Read different types of data from standard input, process them as shown in output format and print the answer to standard output.

Input format:
First line contains integer N.
Second line contains string S.

Output format:
First line should contain N x 2.
Second line should contain the same string S.

Constraints:

 where S length of string S

SAMPLE INPUT 
5
helloworld
SAMPLE OUTPUT 
10
helloworld


Code : 

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.*;
class TestClass {
    public static void main(String args[] ) throws Exception {
        //BufferedReader
        
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int n = Integer.parseInt(br.readLine());
        String s = br.readLine();

        System.out.println(n*2);
        System.out.println(s);
    }
}
