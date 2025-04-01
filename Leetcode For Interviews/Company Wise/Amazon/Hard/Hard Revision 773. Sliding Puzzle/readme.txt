Link : https://leetcode.com/problems/sliding-puzzle/description/?envType=company&envId=amazon&favoriteSlug=amazon-six-months

Question : 

On an 2 x 3 board, there are five tiles labeled from 1 to 5, and an empty square represented by 0. A move consists of choosing 0 and a 4-directionally adjacent number and swapping it.

The state of the board is solved if and only if the board is [[1,2,3],[4,5,0]].

Given the puzzle board board, return the least number of moves required so that the state of the board is solved. If it is impossible for the state of the board to be solved, return -1.

 

Example 1:


Input: board = [[1,2,3],[4,0,5]]
Output: 1
Explanation: Swap the 0 and the 5 in one move.
Example 2:


Input: board = [[1,2,3],[5,4,0]]
Output: -1
Explanation: No number of moves will make the board solved.
Example 3:


Input: board = [[4,1,2],[5,0,3]]
Output: 5
Explanation: 5 is the smallest number of moves that solves the board.
An example path:
After move 0: [[4,1,2],[5,0,3]]
After move 1: [[4,1,2],[0,5,3]]
After move 2: [[0,1,2],[4,5,3]]
After move 3: [[1,0,2],[4,5,3]]
After move 4: [[1,2,0],[4,5,3]]
After move 5: [[1,2,3],[4,5,0]]
 

Constraints:

board.length == 2
board[i].length == 3
0 <= board[i][j] <= 5
Each value board[i][j] is unique.




Solution : In the pdf which i will upload will contain the full explanation what i did and what i didn't and what needed to be done 

class Solution {
    Set<Integer> visited = new HashSet<>();
    int[][] directions = {{1 , 0} , {-1 , 0} , {0 , 1 } , {0 , -1}};

    class BoardDetails {

        int state;
        int depth;

        public BoardDetails(int state , int depth) {
            this.state = state;
            this.depth = depth;
        }

    }

    public int slidingPuzzle(int[][] board) {
        
        int posx = -1;
        int posy = -1;

        for(int i = 0 ; i < 2 ; i ++ ) {
            for(int j = 0 ; j < 3 ; j ++ ) {

                if(board[i][j] == 0 ) {
                    posx = i;
                    posy = j;
                }
            }
        }

        int requiredState = 123450;
        Queue<BoardDetails> q = new LinkedList<>();
        int firstStateOfBoard = getState(board);
        q.offer(new BoardDetails(firstStateOfBoard , 0));
        visited.add(firstStateOfBoard);

        while(!q.isEmpty()) {

            BoardDetails boardDetails = q.poll();
            int state = boardDetails.state;
            int depth = boardDetails.depth;

            if(state == requiredState) {
                return depth;
            }

            int[][] boardForThisState = getBoardForThisState(state);

            // find the position of 0 
            posx = -1;
            posy = -1;

            for(int i = 0 ; i < 2 ; i ++ ) {
                for(int j = 0 ; j < 3 ; j ++ ) {
                    if(boardForThisState[i][j] == 0) {
                        posx = i;
                        posy = j;
                    }
                }
            }

            // try all direction swap

            for(int i = 0 ; i < directions.length ; i ++ ) {

                int newx = posx + directions[i][0];
                int newy = posy + directions[i][1];

                if(!isInvalidCell(newx , newy , boardForThisState)) {
                    swapCells(posx ,posy ,newx ,newy ,boardForThisState );
                    int newState = getState(boardForThisState);
                    if(!visited.contains(newState)) {
                        // never seen this state before 
                        visited.add(newState);
                        q.offer(new BoardDetails(newState , depth + 1 ));
                    }
                    // reverse swap the cells for next direction
                    swapCells(posx ,posy ,newx ,newy ,boardForThisState );
                }
            }
        
        }

        // impossible to reach that state then return -1 
        return -1;

    }

    int[][] getBoardForThisState(int state) {

        int[][] board = new int[2][3];
        String s = String.valueOf(state);
        int len = s.length();
        if(len == 5 ) {
            // for the state 012345 where leading 0 , this is important
            s = "0" + s;
            
        }
        //System.out.println(s);
        int cnt = -1;
        for(int i = 0 ; i < 2 ; i ++ ) {
            for(int j = 0 ; j < 3 ; j ++ ) {
                cnt++;
                board[i][j] = s.charAt(cnt) - '0';
            }
        }

       

        return board;
    }

    void swapCells(int posx , int posy , int newx , int newy , int[][] board ) {

        int temp = board[posx][posy];     
        board[posx][posy] = board[newx][newy];
        board[newx][newy] = temp;

    }

    boolean isInvalidCell(int x , int y , int[][] board) {

        return x < 0 || x >= 2 || y < 0 || y >= 3;
    }

    int getState(int[][] board ) {

        /**
            state of board represented by string append of the order of the numbers
            [1 , 2 , 3]
            [4 , 5 , 0]

            -> 123450
        * */

        int num = 0;

        for(int i = 0 ; i < 2 ; i ++ ) {
            for(int j = 0 ; j < 3 ; j ++ ) {
                num = num * 10 + board[i][j];
            }
        }

        return num;
    }
}
