Link : https://leetcode.com/problems/queries-on-a-permutation-with-key/description/?envType=problem-list-v2&envId=binary-indexed-tree

Question : 
Given the array queries of positive integers between 1 and m, you have to process all queries[i] (from i=0 to i=queries.length-1) according to the following rules:

In the beginning, you have the permutation P=[1,2,3,...,m].
For the current i, find the position of queries[i] in the permutation P (indexing from 0) and then move this at the beginning of the permutation P. Notice that the position of queries[i] in P is the result for queries[i].
Return an array containing the result for the given queries.

 

Example 1:

Input: queries = [3,1,2,1], m = 5
Output: [2,1,2,1] 
Explanation: The queries are processed as follow: 
For i=0: queries[i]=3, P=[1,2,3,4,5], position of 3 in P is 2, then we move 3 to the beginning of P resulting in P=[3,1,2,4,5]. 
For i=1: queries[i]=1, P=[3,1,2,4,5], position of 1 in P is 1, then we move 1 to the beginning of P resulting in P=[1,3,2,4,5]. 
For i=2: queries[i]=2, P=[1,3,2,4,5], position of 2 in P is 2, then we move 2 to the beginning of P resulting in P=[2,1,3,4,5]. 
For i=3: queries[i]=1, P=[2,1,3,4,5], position of 1 in P is 1, then we move 1 to the beginning of P resulting in P=[1,2,3,4,5]. 
Therefore, the array containing the result is [2,1,2,1].  
Example 2:

Input: queries = [4,1,2,2], m = 4
Output: [3,1,2,0]
Example 3:

Input: queries = [7,5,5,8,3], m = 8
Output: [6,5,0,7,5]
 

Constraints:

1 <= m <= 10^3
1 <= queries.length <= m
1 <= queries[i] <= m


Solution  : Watch the explanation pdf to get clear cut understanding 

class Solution {

    class BitTree {

        int[] tree;
        int n;

        public BitTree(int n ) {
            tree = new int[n + 1];
            this.n = n;
        }

        public void update(int index , int val) {
            // remember range of 1 then 2 then 4 and then 8
            while(index <= n ) {
                tree[index] += val;
                index += (index) & (-index);
            }
        }

        public int rangeQuery(int l , int r ) {
            return prefixQuery(r) - prefixQuery(l - 1);
        }

        // prefix sum from 1 to index
        public int prefixQuery(int index) {
            int sum = 0;
            while(index > 0 ) {
                sum += tree[index];
                index -= (index) & (-index);
            }
            return sum;
        }
    }

    public int[] processQueries(int[] queries, int m) {
        
        int q = queries.length;
        int[] ans = new int[q];
        int p = q; // pointer to know where next insertion is to happen
        // integer to location map
        Map<Integer , Integer> hmap = new HashMap<>();
        BitTree bitTree = new BitTree(q + m);
        for(int i = 1 ; i <= m ; i ++  ) {
            bitTree.update(q + i , 1 );
            hmap.put(i , q + i);
        }

        for(int i = 0 ; i < q ; i ++ ) {

            int k = queries[i];
            // get previous location
            int prevLoc = hmap.get(k);
            
            // get how many elements are there before prevLoc
            // how many 1's from 1 to prevLoc 
            ans[i] = bitTree.prefixQuery(prevLoc) - 1 ; // 0 based indexing 

            // make the prevLoc free
            // remove k from that location
            bitTree.update(prevLoc , -1);

            // make entry in the new location
            // put k in the new location
            bitTree.update(p , 1 );

            // enter its new location
            hmap.put(k , p); 

            //System.out.println(k + " " + prevLoc + " " + p);

            p--; // update where the new insertion will happen 
        }



        return ans;


    }
}

