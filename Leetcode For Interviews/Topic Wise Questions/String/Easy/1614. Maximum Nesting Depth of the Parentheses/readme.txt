Link : https://leetcode.com/problems/maximum-nesting-depth-of-the-parentheses/description/?envType=problem-list-v2&envId=string

Question : 

Given a valid parentheses string s, return the nesting depth of s. The nesting depth is the maximum number of nested parentheses.

 

Example 1:

Input: s = "(1+(2*3)+((8)/4))+1"

Output: 3

Explanation:

Digit 8 is inside of 3 nested parentheses in the string.

Example 2:

Input: s = "(1)+((2))+(((3)))"

Output: 3

Explanation:

Digit 3 is inside of 3 nested parentheses in the string.

Example 3:

Input: s = "()(())((()()))"

Output: 3

 

Constraints:

1 <= s.length <= 100
s consists of digits 0-9 and characters '+', '-', '*', '/', '(', and ')'.
It is guaranteed that parentheses expression s is a VPS.


Solution : Simple , at any given time count the number of open brackets thats it 

class Solution {
    public int maxDepth(String s) {
        
        int open = 0;
        int ans = 0;
        for(int i = 0 ; i < s.length() ; i ++ ) {

            if(s.charAt(i) == '(') {
                open++;
            } else if(s.charAt(i) == ')') {
                open--;
            }

            ans = Math.max(ans , open);
        }

        return ans;
    }
}
