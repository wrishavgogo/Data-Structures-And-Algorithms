*** Go through the explanation.pdf to understand what mistakes you did and why you need to read the question properly 

Link : https://leetcode.com/problems/construct-quad-tree/description/?envType=company&envId=uber&favoriteSlug=uber-three-months

Question : 

Given a n * n matrix grid of 0's and 1's only. We want to represent grid with a Quad-Tree.

Return the root of the Quad-Tree representing grid.

A Quad-Tree is a tree data structure in which each internal node has exactly four children. Besides, each node has two attributes:

val: True if the node represents a grid of 1's or False if the node represents a grid of 0's. Notice that you can assign the val to True or False when isLeaf is False, and both are accepted in the answer.
isLeaf: True if the node is a leaf node on the tree or False if the node has four children.
class Node {
    public boolean val;
    public boolean isLeaf;
    public Node topLeft;
    public Node topRight;
    public Node bottomLeft;
    public Node bottomRight;
}
We can construct a Quad-Tree from a two-dimensional area using the following steps:

If the current grid has the same value (i.e all 1's or all 0's) set isLeaf True and set val to the value of the grid and set the four children to Null and stop.
If the current grid has different values, set isLeaf to False and set val to any value and divide the current grid into four sub-grids as shown in the photo.
Recurse for each of the children with the proper sub-grid.

If you want to know more about the Quad-Tree, you can refer to the wiki.

Quad-Tree format:

You don't need to read this section for solving the problem. This is only if you want to understand the output format here. The output represents the serialized format of a Quad-Tree using level order traversal, where null signifies a path terminator where no node exists below.

It is very similar to the serialization of the binary tree. The only difference is that the node is represented as a list [isLeaf, val].

If the value of isLeaf or val is True we represent it as 1 in the list [isLeaf, val] and if the value of isLeaf or val is False we represent it as 0.

 

Example 1:


Input: grid = [[0,1],[1,0]]
Output: [[0,1],[1,0],[1,1],[1,1],[1,0]]
Explanation: The explanation of this example is shown below:
Notice that 0 represents False and 1 represents True in the photo representing the Quad-Tree.

Example 2:



Input: grid = [[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,1,1,1,1],[1,1,1,1,1,1,1,1],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0]]
Output: [[0,1],[1,1],[0,1],[1,1],[1,0],null,null,null,null,[1,0],[1,0],[1,1],[1,1]]
Explanation: All values in the grid are not the same. We divide the grid into four sub-grids.
The topLeft, bottomLeft and bottomRight each has the same value.
The topRight have different values so we divide it into 4 sub-grids where each has the same value.
Explanation is shown in the photo below:

 

Constraints:

n == grid.length == grid[i].length
n == 2x where 0 <= x <= 6



Solution : 


/*
// Definition for a QuadTree node.
class Node {
    public boolean val;
    public boolean isLeaf;
    public Node topLeft;
    public Node topRight;
    public Node bottomLeft;
    public Node bottomRight;

    
    public Node() {
        this.val = false;
        this.isLeaf = false;
        this.topLeft = null;
        this.topRight = null;
        this.bottomLeft = null;
        this.bottomRight = null;
    }
    
    public Node(boolean val, boolean isLeaf) {
        this.val = val;
        this.isLeaf = isLeaf;
        this.topLeft = null;
        this.topRight = null;
        this.bottomLeft = null;
        this.bottomRight = null;
    }
    
    public Node(boolean val, boolean isLeaf, Node topLeft, Node topRight, Node bottomLeft, Node bottomRight) {
        this.val = val;
        this.isLeaf = isLeaf;
        this.topLeft = topLeft;
        this.topRight = topRight;
        this.bottomLeft = bottomLeft;
        this.bottomRight = bottomRight;
    }
}
*/

class Solution {
    public Node construct(int[][] grid) {

        int n = grid.length;
        return recursiveConstruction(grid , 0 , 0 , n - 1 , n - 1 );
        
        
    }


    public Node recursiveConstruction(int[][] grid ,  int startRow , int startCol , int endRow , int endCol ) {

        Node node = new Node();

        if(startRow == endRow &&  startCol == endCol ) {
            // start and end are the same node 
            // that means we are in single cell level
            node.val = grid[startRow][startCol] == 1 ? true : false;
            node.isLeaf = true;
            return node ;
        }


        
        int midRow = (startRow + endRow) / 2;
        int midCol = (startCol + endCol) / 2;
        
        // topLeft
        Node firstQuad = recursiveConstruction(grid , startRow , startCol , midRow , midCol );
        // topRight
        Node secondQuad = recursiveConstruction(grid , startRow , midCol + 1 , midRow , endCol );
        // bottomLeft
        Node thirdQuad = recursiveConstruction(grid , midRow + 1 , startCol , endRow , midCol );
        // bottomRight
        Node fourthQuad = recursiveConstruction(grid , midRow + 1 , midCol + 1  , endRow , endCol );

        node.val = firstQuad.val;
        boolean conditionThatValuesAreEqual = firstQuad.val == secondQuad.val && secondQuad.val == thirdQuad.val && thirdQuad.val == fourthQuad.val;
        boolean areChildrenLeafNodes = firstQuad.isLeaf && secondQuad.isLeaf &&  thirdQuad.isLeaf && fourthQuad.isLeaf;

        if(conditionThatValuesAreEqual && areChildrenLeafNodes ) {
            // that means all the vals are the same 
            /**
                If the current grid has the same value (i.e all 1's or all 0's) 
                set isLeaf True and set val to the value of the grid 
                and set the four children to Null and stop.
             */
            node.isLeaf = true;
            return node;
        } 

        // we are not in leaf  anymore 
        node.isLeaf = false;

        node.topLeft = firstQuad;
        node.topRight = secondQuad;
        node.bottomLeft = thirdQuad;
        node.bottomRight = fourthQuad;

        return node;

    }
}
