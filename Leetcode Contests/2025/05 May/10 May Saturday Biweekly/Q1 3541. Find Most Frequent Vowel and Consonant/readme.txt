Link : https://leetcode.com/problems/find-most-frequent-vowel-and-consonant/description/

Self explanatory 

Question : 

You are given a string s consisting of lowercase English letters ('a' to 'z').

Your task is to:

Find the vowel (one of 'a', 'e', 'i', 'o', or 'u') with the maximum frequency.
Find the consonant (all other letters excluding vowels) with the maximum frequency.
Return the sum of the two frequencies.

Note: If multiple vowels or consonants have the same maximum frequency, you may choose any one of them. If there are no vowels or no consonants in the string, consider their frequency as 0.

The frequency of a letter x is the number of times it occurs in the string.
 

Example 1:

Input: s = "successes"

Output: 6

Explanation:

The vowels are: 'u' (frequency 1), 'e' (frequency 2). The maximum frequency is 2.
The consonants are: 's' (frequency 4), 'c' (frequency 2). The maximum frequency is 4.
The output is 2 + 4 = 6.
Example 2:

Input: s = "aeiaeia"

Output: 3

Explanation:

The vowels are: 'a' (frequency 3), 'e' ( frequency 2), 'i' (frequency 2). The maximum frequency is 3.
There are no consonants in s. Hence, maximum consonant frequency = 0.
The output is 3 + 0 = 3.
 

Constraints:

1 <= s.length <= 100
s consists of lowercase English letters only.


Code : 

class Solution {
    public int maxFreqSum(String s) {
        
        
        
        Set<Character> st = new HashSet<>();
        st.add('a');
        st.add('e');
        st.add('i');
        st.add('o');
        st.add('u');
        
        Map<Character , Integer > mp = new HashMap<>();
        
        int vowel = 0;
        int non = 0;
        
        for(int i = 0 ; i < s.length() ; i ++ ) {
            char c = s.charAt(i);
            mp.put(c , mp.getOrDefault(c , 0 ) + 1);
        }
        
        for(int i = 0 ; i < s.length() ; i ++ ) {
            
            char c = s.charAt(i);
            if(st.contains(c)) {
                vowel = Math.max(vowel , mp.getOrDefault(c , 0));
            } else {
                non = Math.max(non , mp.getOrDefault(c , 0));
            }
        }
        
        return non + vowel;
    }
}
