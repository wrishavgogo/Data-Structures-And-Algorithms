Link : https://leetcode.com/problems/minimum-genetic-mutation/description/?envType=company&envId=oracle&favoriteSlug=oracle-all

Question : 

A gene string can be represented by an 8-character long string, with choices from 'A', 'C', 'G', and 'T'.

Suppose we need to investigate a mutation from a gene string startGene to a gene string endGene where one mutation is defined as one single character changed in the gene string.

For example, "AACCGGTT" --> "AACCGGTA" is one mutation.
There is also a gene bank bank that records all the valid gene mutations. A gene must be in bank to make it a valid gene string.

Given the two gene strings startGene and endGene and the gene bank bank, return the minimum number of mutations needed to mutate from startGene to endGene. If there is no such a mutation, return -1.

Note that the starting point is assumed to be valid, so it might not be included in the bank.

 

Example 1:

Input: startGene = "AACCGGTT", endGene = "AACCGGTA", bank = ["AACCGGTA"]
Output: 1
Example 2:

Input: startGene = "AACCGGTT", endGene = "AAACGGTA", bank = ["AACCGGTA","AACCGCTA","AAACGGTA"]
Output: 2
 

Constraints:

0 <= bank.length <= 10
startGene.length == endGene.length == bank[i].length == 8
startGene, endGene, and bank[i] consist of only the characters ['A', 'C', 'G', 'T'].


Code : 

class Solution {
    public int minMutation(String startGene, String endGene, String[] bank) {
        
        Set<String> validGeneSet = new HashSet<>();
        Set<String> visited = new HashSet<>();
        for(int i = 0 ; i < bank.length ; i ++ ) {
            validGeneSet.add(bank[i]);
        }

        char[] aminoAcid = new char[4];
        aminoAcid[0] = 'A';
        aminoAcid[1] = 'C';
        aminoAcid[2] = 'G';
        aminoAcid[3] = 'T';
        

        Queue<Node> q = new LinkedList<Node>();
        q.offer(new Node(startGene , 0));
        visited.add(startGene);

        int ans = -1;

        while(!q.isEmpty()) {

            Node node  = q.poll();
            if(node.state.equals(endGene)) {
                ans = node.depth;
                break;
            }
            String parent = node.state;
            int depth = node.depth;
            char[] parentArr = parent.toCharArray();
            int n = parentArr.length;
            char[] childArr = new char[n];

            for(int i = 0 ; i < n; i ++ ) {
                childArr[i] = parentArr[i];
            }

            for(int i = 0 ; i < n ; i ++ ) {
                
                for(int j = 0 ; j < 4 ; j ++ ) {
                    childArr[i] = aminoAcid[j];
                    String childString = new String(childArr);
                    if(validGeneSet.contains(childString) && !visited.contains(childString)) {
                        visited.add(childString);
                        q.offer(new Node(childString , depth + 1));
                    }
                } 
                // since one single mutation is considered one move
                // hence we revert back the previous i's character 
                childArr[i] = parentArr[i];
            }

        }



        return ans;

    }

    class Node {

        String state;
        int depth;

        public Node(String state , int depth) {
            this.state = state;
            this.depth = depth;
        }
    }
}
