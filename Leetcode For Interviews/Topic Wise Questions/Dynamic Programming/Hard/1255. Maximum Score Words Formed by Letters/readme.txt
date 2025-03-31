Link : https://leetcode.com/problems/maximum-score-words-formed-by-letters/description/?envType=problem-list-v2&envId=dynamic-programming

Question : 

Given a list of words, list of  single letters (might be repeating) and score of every character.

Return the maximum score of any valid set of words formed by using the given letters (words[i] cannot be used two or more times).

It is not necessary to use all characters in letters and each letter can only be used once. Score of letters 'a', 'b', 'c', ... ,'z' is given by score[0], score[1], ... , score[25] respectively.

 

Example 1:

Input: words = ["dog","cat","dad","good"], letters = ["a","a","c","d","d","d","g","o","o"], score = [1,0,9,5,0,0,3,0,0,0,0,0,0,0,2,0,0,0,0,0,0,0,0,0,0,0]
Output: 23
Explanation:
Score  a=1, c=9, d=5, g=3, o=2
Given letters, we can form the words "dad" (5+1+5) and "good" (3+2+2+5) with a score of 23.
Words "dad" and "dog" only get a score of 21.
Example 2:

Input: words = ["xxxz","ax","bx","cx"], letters = ["z","a","b","c","x","x","x"], score = [4,4,4,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,5,0,10]
Output: 27
Explanation:
Score  a=4, b=4, c=4, x=5, z=10
Given letters, we can form the words "ax" (4+5), "bx" (4+5) and "cx" (4+5) with a score of 27.
Word "xxxz" only get a score of 25.
Example 3:

Input: words = ["leetcode"], letters = ["l","e","t","c","o","d"], score = [0,0,1,1,1,0,0,0,0,0,0,1,0,0,1,0,0,0,0,1,0,0,0,0,0,0]
Output: 0
Explanation:
Letter "e" can only be used once.
 

Constraints:

1 <= words.length <= 14
1 <= words[i].length <= 15
1 <= letters.length <= 100
letters[i].length == 1
score.length == 26
0 <= score[i] <= 10
words[i], letters[i] contains only lower case English letters.



Code : Well written good comments and good names for attribute 

class Solution {

    // without cache solution

    public int maxScoreWords(String[] words, char[] letters, int[] score) {

        Map<Character , Integer> charToFrequencyMap = new HashMap<>();

        for(int i = 0 ; i < letters.length ; i ++ ) {
            char c = letters[i];
            charToFrequencyMap.put(c  , charToFrequencyMap.getOrDefault(c , 0 ) + 1 );
        }

        return f(0 , charToFrequencyMap , score , words );
        
    }

    int f(int startPosition , Map<Character , Integer> charToFrequencyMap , int[] score , String[] words  ) {

        // at any index i have two choices , either pick the work or reject it 

        if(startPosition >= words.length) {
            // no score can be generated 
            return 0;
        }


        // in this position decide if you want to pick this work or not 

        /// first of all check if work can be at all picked or not 

        if(!canWordBePicked(words[startPosition] , charToFrequencyMap  )) {
            return f(startPosition + 1 , charToFrequencyMap , score , words);
        }

        // word can be satisfied , now you decide whether to pick it or not 

        int maxScore = 0;

        // 1st option pick the word 
        maxScore = Math.max(maxScore , pickWord(words[startPosition] , charToFrequencyMap , score ) + f(startPosition + 1 , charToFrequencyMap , score , words ) );



        // 2nd option dont pick the word
        putWordBack(words[startPosition], charToFrequencyMap);

        maxScore = Math.max(maxScore , f(startPosition + 1 , charToFrequencyMap , score , words ));


        return maxScore;


    }

    private int pickWord(String word , Map<Character , Integer> charToFrequencyMap , int[] score ) {

        int scoreForThisWord = 0 ;

        for(int i = 0 ; i < word.length() ; i ++ ) {
            char c = word.charAt(i);
            charToFrequencyMap.put(c , charToFrequencyMap.getOrDefault(c , 1) - 1  );
            scoreForThisWord += score[c - 'a']; 
        }

        return scoreForThisWord;
    }

    private void putWordBack(String word , Map<Character , Integer> charToFrequencyMap ) {

        for(int i = 0 ; i < word.length() ; i ++ ) {
            char c = word.charAt(i);
            charToFrequencyMap.put(c , charToFrequencyMap.getOrDefault(c , 0) +  1  );            
        }

    }

    private boolean canWordBePicked(String word , Map<Character , Integer> charToFrequencyMap ) {

        Map<Character , Integer > charToFrequencyMapForWord = new HashMap<>();
        for(int i = 0 ; i < word.length() ; i ++ ) {
            char c = word.charAt(i);
            charToFrequencyMapForWord.put(c , charToFrequencyMapForWord.getOrDefault(c , 0) + 1 );
        }

        for(Map.Entry<Character ,Integer> entry : charToFrequencyMapForWord.entrySet() ) {
            char c = entry.getKey();
            int freq = entry.getValue();
            if(charToFrequencyMap.getOrDefault(c , 0 ) < freq ) {
                return false;
            }
        }

        return true;
    }
}
