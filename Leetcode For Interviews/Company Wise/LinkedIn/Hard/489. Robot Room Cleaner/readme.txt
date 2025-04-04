Premium 
Link : https://leetcode.com/problems/robot-room-cleaner/description/?envType=company&envId=linkedin&favoriteSlug=linkedin-three-months

Question : 

You are controlling a robot that is located somewhere in a room. The room is modeled as an m x n binary grid where 0 represents a wall and 1 represents an empty slot.

The robot starts at an unknown location in the room that is guaranteed to be empty, and you do not have access to the grid, but you can move the robot using the given API Robot.

You are tasked to use the robot to clean the entire room (i.e., clean every empty cell in the room). The robot with the four given APIs can move forward, turn left, or turn right. Each turn is 90 degrees.

When the robot tries to move into a wall cell, its bumper sensor detects the obstacle, and it stays on the current cell.

Design an algorithm to clean the entire room using the following APIs:

interface Robot {
  // returns true if next cell is open and robot moves into the cell.
  // returns false if next cell is obstacle and robot stays on the current cell.
  boolean move();

  // Robot will stay on the same cell after calling turnLeft/turnRight.
  // Each turn will be 90 degrees.
  void turnLeft();
  void turnRight();

  // Clean the current cell.
  void clean();
}
Note that the initial direction of the robot will be facing up. You can assume all four edges of the grid are all surrounded by a wall.

 

Custom testing:

The input is only given to initialize the room and the robot's position internally. You must solve this problem "blindfolded". In other words, you must control the robot using only the four mentioned APIs without knowing the room layout and the initial robot's position.

 

Example 1:


Input: room = [[1,1,1,1,1,0,1,1],[1,1,1,1,1,0,1,1],[1,0,1,1,1,1,1,1],[0,0,0,1,0,0,0,0],[1,1,1,1,1,1,1,1]], row = 1, col = 3
Output: Robot cleaned all rooms.
Explanation: All grids in the room are marked by either 0 or 1.
0 means the cell is blocked, while 1 means the cell is accessible.
The robot initially starts at the position of row=1, col=3.
From the top left corner, its position is one row below and three columns right.
Example 2:

Input: room = [[1]], row = 0, col = 0
Output: Robot cleaned all rooms.
 

Constraints:

m == room.length
n == room[i].length
1 <= m <= 100
1 <= n <= 200
room[i][j] is either 0 or 1.
0 <= row < m
0 <= col < n
room[row][col] == 1
All the empty cells can be visited from the starting position.




Soln :  Read in the datewise desc , i have written why equals and hashcode method overriding was important , the bucketing thing in hashMap and hASHsET

/**
 * // This is the robot's control interface.
 * // You should not implement it, or speculate about its implementation
 * interface Robot {
 *     // Returns true if the cell in front is open and robot moves into the cell.
 *     // Returns false if the cell in front is blocked and robot stays in the current cell.
 *     public boolean move();
 *
 *     // Robot will stay in the same cell after calling turnLeft/turnRight.
 *     // Each turn will be 90 degrees.
 *     public void turnLeft();
 *     public void turnRight();
 *
 *     // Clean the current cell.
 *     public void clean();
 * }
 */

class Solution {

    class Position {
        int x ; 
        int y;

        public Position(int x , int y ) {
            this.x = x;
            this.y = y;
        }

        @Override
        public boolean equals(Object o ) {
            if(o == null ) {
                return false;
            }
            if(! (o instanceof Position)) {
                return false;
            }
            Position other = (Position) o;
            return this.x == other.x && this.y == other.y;
        }

        @Override
        public int hashCode() {
            return Objects.hash(x , y);
        }

    }

    Set<Position> visitedSet = new HashSet<>();
    /**
        initially robot facing upwards 
        {-1 , 0} -> move up
        {0 , 1} -> turn right to move right
        {1 , 0} -> turn right to move down 
        {0 , -1} -> turn right to move left
     */
    int[][] directions = {{-1 , 0} , {0 , 1} , {1 , 0} , {0 , -1}};

    public void cleanRoom(Robot robot) {

        // 0 ,0 arbritary search indexes in perspective to starting index 
        // 0 -> currenttly facing up
        dfs(0 , 0 , robot , 0);
        
    }

    void dfs(int x , int y , Robot robot , int currentOrientation) {

        if(visitedSet.contains(new Position(x , y))) {
            // cell was already visited before 
            return ;
        }

        robot.clean(); // clean the room 
        visitedSet.add(new Position(x , y));

        for(int i = 1 ; i <= 4 ; i ++  ) {

            /***
                Whatever was the previous orientation 
                we are adding 1 to it each time 
                if currentOrientation = 2 ( means its facing down)
                if we do a rotateRight i.e +1 
                currentOrientation = 3 ( means now its facing left )
                so each time adding 1 means moving it to the right 
                and thats why we start with 1 = 1 ---> 4 
                rotating it in all directions 
            * */

            int newOrientation = (currentOrientation + i) % 4;

            int newx = x + directions[newOrientation][0];
            int newy = y + directions[newOrientation][1];

            robot.turnRight();

            if(!visitedSet.contains(new Position(newx , newy)) && robot.move() ) {
                // next cell is not visited and robot can move right
                dfs(newx , newy , robot , newOrientation);
            }
            

        }

        // after exploring all the posibilities on this cell on all 4 directions
        // now its my responsibility to return it to the cell where it came from 
        // in the same state which it started its journey from 

        /***
            prevCell    nextCell
            [ ]         [ -> ]

            prevCell    nextcell
            [ -> ].     []

            so i have to make the robot first face the previous cell 

            prevCell    nextCell
            [ ]         [ <- ]

            then move it 

             prevCell    nextCell
            [<-  ]         [ ]

            then again make it face as it was facing earlier

            prevCell    nextCell
            [->  ]         [ ]
                    
         */
        
        robot.turnRight();
        robot.turnRight();
        // 180 degree reverse turn 
        robot.move();
        robot.turnRight();
        robot.turnRight();
    }


}
