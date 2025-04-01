Link : https://leetcode.com/problems/n-queens/description/?envType=company&envId=amazon&favoriteSlug=amazon-three-months

Question : 

The n-queens puzzle is the problem of placing n queens on an n x n chessboard such that no two queens attack each other.

Given an integer n, return all distinct solutions to the n-queens puzzle. You may return the answer in any order.

Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space, respectively.

 

Example 1:


Input: n = 4
Output: [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above
Example 2:

Input: n = 1
Output: [["Q"]]
 

Constraints:

1 <= n <= 9


Code :  

Important learning in this code , during recursion i was wondering how , after the backtrack process 

 positionList.add(j); // adding in the list
 backTrack(queensPlaced + 1 , positionList  , n);
 positionList.removeLast(); // removing from the list O(1) 

when we declare List<Integer> positionList = new LinkedList<>();
LinkedList gives us a function removeLast() or removeFirst() whichever we do takes O(1) operation 

also one more important learning , in ArrayList -> list.remove(list.size() - 1 ) -> is O(1) operation and not O(n) operation 


class Solution {

    // we dont need hashMap of row as we are placing each queen on row by row 
    Map<Integer , Integer> col = new HashMap<>();
    Map<Integer , Integer> positiveDiag = new HashMap<>();
    Map<Integer , Integer> negativeDiag = new HashMap<>();
    List<List<String>> ans = new ArrayList<>();

    public List<List<String>> solveNQueens(int n) {

        List<Integer> positionList = new LinkedList<>();
        backTrack(0 , positionList , n );

        return ans;
        
        
    }

    void backTrack(int queensPlaced , List<Integer> positionList , int n) {

        if(queensPlaced == n ) {
            // all queens are placed
            populateUsingPositionList(positionList);
            return;
        }


        int row = queensPlaced ; // we are trying n queens 1 by 1
                                // queen #i (1 based indexing) will be placed at row i - 1 
                                // hence row = queens Already placed excluding this one 

        
        int i = row ; // for readability 

        for(int j = 0 ; j < n ; j ++ ) {
            
            if( col.getOrDefault(j , 0) == 0 && positiveDiag.getOrDefault(i + j , 0) == 0   // ----------> in this section i was doing  a silly mistke of using OR condition instead of  && 
            && negativeDiag.getOrDefault(i - j , 0) == 0  ) {
                // no crossover from anywhere 
                col.put(j , 1);
                positiveDiag.put(i + j , 1);
                negativeDiag.put(i - j , 1);
                positionList.add(j); // adding in the list

                backTrack(queensPlaced + 1 , positionList  , n);

                // revert operations
                col.put(j , 0);
                positiveDiag.put(i + j , 0);
                negativeDiag.put(i - j , 0);
                positionList.removeLast(); // removing from the list O(1)
            }
        }
    }


    void populateUsingPositionList(List<Integer> positionList) {

        // positionList.stream().forEach(System.out::print);
        // System.out.println();

        // positionList contains the columns 
        List<String> state = new ArrayList<>();
        int n = positionList.size();
        for(int i = 0 ; i < n ; i ++ ) {
            StringBuilder sb = new StringBuilder();
            for(int k = 0 ; k < n ; k ++ ) {
                if(positionList.get(i) == k ) {
                    sb.append('Q');
                } else {
                    sb.append('.');
                }
            }
            state.add(sb.toString());
        }

        ans.add(state);
    }

    




}
