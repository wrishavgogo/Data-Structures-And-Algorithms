
Link : https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string-ii/description/?envType=company&envId=oracle&favoriteSlug=oracle-three-months

Question : 

You are given a string s and an integer k, a k duplicate removal consists of choosing k adjacent and equal letters from s and removing them, causing the left and the right side of the deleted substring to concatenate together.

We repeatedly make k duplicate removals on s until we no longer can.

Return the final string after all such duplicate removals have been made. It is guaranteed that the answer is unique.

 

Example 1:

Input: s = "abcd", k = 2
Output: "abcd"
Explanation: There's nothing to delete.
Example 2:

Input: s = "deeedbbcccbdaa", k = 3
Output: "aa"
Explanation: 
First delete "eee" and "ccc", get "ddbbbdaa"
Then delete "bbb", get "dddaa"
Finally delete "ddd", get "aa"
Example 3:

Input: s = "pbbcggttciiippooaais", k = 2
Output: "ps"
 

Constraints:

1 <= s.length <= 105
2 <= k <= 104
s only contains lowercase English letters.


Soln : 

class Solution {

    class Node {

        char c ;
        int contiguousLength;

        public Node(char c ,int contiguousLength) {
            this.c = c;
            this.contiguousLength = contiguousLength;
        }
    }


    public String removeDuplicates(String s, int k) {
        

        Stack<Node> st = new Stack<>();
        int n = s.length();

        for(int i = 0 ; i < n ; i ++ ) {

            char c1 = s.charAt(i);
            if(st.isEmpty()) {
                st.push(new Node(c1 , 1));
                continue;
            }

            if(st.peek().c == c1 ) {
                Node node = st.peek();
                st.push(new Node(node.c , node.contiguousLength + 1));
            } else {
                st.push(new Node(c1 , 1));
            }

            if(st.peek().contiguousLength >= k ) {
                char cRepeated = st.peek().c;
                while(!st.empty() && st.peek().c == cRepeated && 
                st.peek().contiguousLength > 0 ) {
                    st.pop();
                }
            }

            
        }

        StringBuilder sb = new StringBuilder();
        while(!st.isEmpty()) {
            sb.append(st.pop().c);
        }
        sb.reverse();

        return sb.toString();
    }
}
