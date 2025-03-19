Link : https://leetcode.com/problems/isomorphic-strings/description/?envType=company&envId=salesforce&favoriteSlug=salesforce-all



Question : 

Given two strings s and t, determine if they are isomorphic.

Two strings s and t are isomorphic if the characters in s can be replaced to get t.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character, but a character may map to itself.

 

Example 1:

Input: s = "egg", t = "add"

Output: true

Explanation:

The strings s and t can be made identical by:

Mapping 'e' to 'a'.
Mapping 'g' to 'd'.
Example 2:

Input: s = "foo", t = "bar"

Output: false

Explanation:

The strings s and t can not be made identical as 'o' needs to be mapped to both 'a' and 'r'.

Example 3:

Input: s = "paper", t = "title"

Output: true

 

Constraints:

1 <= s.length <= 5 * 104
t.length == s.length
s and t consist of any valid ascii character.

Soln: Watch pdf explanation

class Solution {
    public boolean isIsomorphic(String s, String t) {
        

        Map<Character , Character > mappingsSToT = new HashMap<>();
        Map<Character , Character > mappingsTToS = new HashMap<>();

        // length is given same for both
        int n = s.length();

        for(int i = 0 ; i < n ; i ++  ) {

            char cs = s.charAt(i);
            char ct = t.charAt(i);

            // case1 mapping of cs is not there 
            if(!mappingsSToT.containsKey(cs)) {
                
                // but at the same time check that ct is not mapped to anycharacter 

                if(!mappingsTToS.containsKey(ct)) {
                    // happy flow no mapping for both the new characters encountered 

                    // create mapping then
                    mappingsSToT.put(cs , ct);
                    mappingsTToS.put(ct , cs);
                }
                else if(mappingsTToS.get(ct) != cs) {
                    // some other character was already mapped to ct other than cs
                    // and one character cannot be mapped to multiple characters 
                    return false;
                }

            } else {

                // mapping for cs is already made 
                // if its not equal to the ct then ct cannot be mapped 
                if(mappingsSToT.get(cs) != ct) {
                    return false;
                }
            }
        }

        return true;


    }
}
