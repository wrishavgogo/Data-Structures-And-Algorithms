Link : https://leetcode.com/problems/design-compressed-string-iterator/description/?envType=problem-list-v2&envId=string

Question : 

Design and implement a data structure for a compressed string iterator. The given compressed string will be in the form of each letter followed by a positive integer representing the number of this letter existing in the original uncompressed string.

Implement the StringIterator class:

next() Returns the next character if the original string still has uncompressed characters, otherwise returns a white space.
hasNext() Returns true if there is any letter needs to be uncompressed in the original string, otherwise returns false.
 

Example 1:

Input
["StringIterator", "next", "next", "next", "next", "next", "next", "hasNext", "next", "hasNext"]
[["L1e2t1C1o1d1e1"], [], [], [], [], [], [], [], [], []]
Output
[null, "L", "e", "e", "t", "C", "o", true, "d", true]

Explanation
StringIterator stringIterator = new StringIterator("L1e2t1C1o1d1e1");
stringIterator.next(); // return "L"
stringIterator.next(); // return "e"
stringIterator.next(); // return "e"
stringIterator.next(); // return "t"
stringIterator.next(); // return "C"
stringIterator.next(); // return "o"
stringIterator.hasNext(); // return True
stringIterator.next(); // return "d"
stringIterator.hasNext(); // return True
 

Constraints:

1 <= compressedString.length <= 1000
compressedString consists of lower-case an upper-case English letters and digits.
The number of a single character repetitions in compressedString is in the range [1, 10^9]
At most 100 calls will be made to next and hasNext.



Soln : This is an implementation heavy code please make your implementation better by revising it , i used stack to implement it 
        mistake what i made is not considering L123 , like this 123 has to be picked , so i have to do a while(Characters.isDigit(st.peek()) ---> new thing i learnt 

class StringIterator {

    Stack<Character> st;
    int n;
    int currentCount = 0;
    char currentChar = '*';
    public StringIterator(String compressedString) {
        
        n = compressedString.length();
        st = new Stack<Character>();
        for(int i = n - 1 ; i >= 0 ; i -- ) {
            st.push(compressedString.charAt(i));
        }

        /***  
            head -> ["L1e2t1C1o1d1e1"]
         */
    }
    
    public char next() {
        
        if(currentCount == 0 ) {
            // condition to check if we want to switch to next character
            if(st.size() == 0 ) {
                return ' ';
            }  else {
                currentChar = st.pop(); // the character 
                while(st.size() > 0 && Character.isDigit(st.peek())) {       -------> where i made the mistake 
                    currentCount = 10*currentCount + (st.pop() - '0');
                }
            }
        }

        currentCount--;
        return currentChar;
    }
    
    public boolean hasNext() {
        
        if(currentCount == 0 && st.size() == 0 ) {
            return false;
        }

        return true;
    }
}

/**
 * Your StringIterator object will be instantiated and called as such:
 * StringIterator obj = new StringIterator(compressedString);
 * char param_1 = obj.next();
 * boolean param_2 = obj.hasNext();
 */
