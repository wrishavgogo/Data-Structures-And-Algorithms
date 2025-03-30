Link : https://leetcode.com/problems/count-sorted-vowel-strings/description/?envType=problem-list-v2&envId=dynamic-programming

Question : 

Given an integer n, return the number of strings of length n that consist only of vowels (a, e, i, o, u) and are lexicographically sorted.

A string s is lexicographically sorted if for all valid i, s[i] is the same as or comes before s[i+1] in the alphabet.

 

Example 1:

Input: n = 1
Output: 5
Explanation: The 5 sorted strings that consist of vowels only are ["a","e","i","o","u"].
Example 2:

Input: n = 2
Output: 15
Explanation: The 15 sorted strings that consist of vowels only are
["aa","ae","ai","ao","au","ee","ei","eo","eu","ii","io","iu","oo","ou","uu"].
Note that "ea" is not a valid string since 'e' comes after 'a' in the alphabet.
Example 3:

Input: n = 33
Output: 66045
 

Constraints:

1 <= n <= 50 


Code : with explanation and comments 

class Solution {

    int[][] cache = new int[5][51];

    void invalidateCache() {

        for(int i = 0 ; i < 5 ; i ++ ) {
            for(int j = 0 ; j < 51 ; j++ ) {
                cache[i][j] = -1;
            }
        }
    }

    public int countVowelStrings(int n) {

        int ans = 0;

        invalidateCache();

        // using 
        // 0 -> a
        // 1 -> e
        // 2 -> i 
        // 3 -> o
        // 4 -> u
        for(int i = 0 ; i <= 4 ; i ++ ) {
            // we have to put all the characters in the start 
            ans += f(i , n);
        }

        return ans;
    }

    

    /***
        this function tells me what is the 
        number of strings i can form which follow the condition 
        1. only consists of vowels
        2. is lexicographically sorted 

        where start = "starting character of the string"
        length = length of the string am about to form


    * */
    int f(int start , int length ) {

        if(length == 1 ) {
            // obviously for 1 length am gonna have 1 string the vowel itself
            return 1;
        }

        if(cache[start][length] != -1 ) {
            return cache[start][length];
        }

        int ans = 0;

        for(int i = start ; i <= 4 ; i ++ ) {

            // so for the next character of the string ,
            // it will have to be >= start for it be lexicographically sorted
            ans += f(i , length - 1 ); 
        }

       cache[start][length] = ans;

       return ans;

        
    }

    
}

