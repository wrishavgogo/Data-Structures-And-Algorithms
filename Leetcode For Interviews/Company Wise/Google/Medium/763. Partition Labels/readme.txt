Link : https://leetcode.com/problems/partition-labels/description/?envType=company&envId=google&favoriteSlug=google-thirty-days

Question : 

You are given a string s. We want to partition the string into as many parts as possible so that each letter appears in at most one part. For example, the string "ababcc" can be partitioned into ["abab", "cc"], but partitions such as ["aba", "bcc"] or ["ab", "ab", "cc"] are invalid.

Note that the partition is done so that after concatenating all the parts in order, the resultant string should be s.

Return a list of integers representing the size of these parts.

 

Example 1:

Input: s = "ababcbacadefegdehijhklij"
Output: [9,7,8]
Explanation:
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits s into less parts.
Example 2:

Input: s = "eccbbbbdec"
Output: [10]
 

Constraints:

1 <= s.length <= 500
s consists of lowercase English letters.


Solution : 

class Solution {
    public List<Integer> partitionLabels(String s) {
        
        Map<Character , Integer > charToLastPositionMap = new HashMap<>();
        int n = s.length();
        for(int i = 0 ; i < n ; i ++ ) {
            charToLastPositionMap.put(s.charAt(i) , i);
        }


        List<Integer> partionList = new ArrayList<>();
        int currentMax = -1;

        for(int i = 0 ; i < n ; i ++ ) {

            currentMax = Math.max(currentMax , charToLastPositionMap.get(s.charAt(i)));
            
            if(i == currentMax) {
            
                if(partionList.size() == 0 ) {
                    partionList.add(currentMax + 1);
                } else {
                    int len = partionList.size();
                    partionList.add(currentMax + 1);
                }
            }

        }

        int len = partionList.size();
        for(int i = len - 1 ; i > 0 ; i -- ) {
            int newVal = partionList.get(i) - partionList.get(i-1);
            partionList.set(i , newVal);
        }

        return partionList;


    }
}
