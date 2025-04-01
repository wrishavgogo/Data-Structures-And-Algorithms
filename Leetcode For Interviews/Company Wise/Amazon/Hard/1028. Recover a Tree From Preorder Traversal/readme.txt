Link : https://leetcode.com/problems/recover-a-tree-from-preorder-traversal/description/?envType=company&envId=amazon&favoriteSlug=amazon-six-months

Question : 

We run a preorder depth-first search (DFS) on the root of a binary tree.

At each node in this traversal, we output D dashes (where D is the depth of this node), then we output the value of this node.  If the depth of a node is D, the depth of its immediate child is D + 1.  The depth of the root node is 0.

If a node has only one child, that child is guaranteed to be the left child.

Given the output traversal of this traversal, recover the tree and return its root.

 

Example 1:


Input: traversal = "1-2--3--4-5--6--7"
Output: [1,2,5,3,4,6,7]
Example 2:


Input: traversal = "1-2--3---4-5--6---7"
Output: [1,2,5,3,null,6,null,4,null,7]
Example 3:


Input: traversal = "1-401--349---90--88"
Output: [1,401,null,349,88,90]
 

Constraints:

The number of nodes in the original tree is in the range [1, 1000].
1 <= Node.val <= 109


Code with explanation in comments : 

/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    class TreeNodeDepth {
        TreeNode treeNode;
        int depth;

        public TreeNodeDepth(TreeNode treeNode, int depth ) {
            this.treeNode = treeNode;
            this.depth = depth;
        }

    }

    public TreeNode recoverFromPreorder(String traversal) {
        
        TreeNode root = null;
        /*** */

        Stack<TreeNodeDepth> st = new Stack<>();

        //  first number encountered in the string will be root node 
        int len = traversal.length();

        int num = 0;
        int depth = 0;
        for(int i = 0 ; i < len  ; i++ ) {

            if(traversal.charAt(i) == '-') {
                
                if(num != 0 ) {
                    // there is an active number which just ended 

                    if(st.isEmpty()) {
                        // no insertion has happened as of now 
                        // this is the root node about to get inserted
                        root = new TreeNode(num);
                        st.push(new TreeNodeDepth(root , 0));
                    }
                    else {
                        // a tree is already existing , a logic is needed to decide where this node is going
                        // to go 
                        insertIntoStack(st , num , depth);
                    }
                    // for first '-' encountered reset depth = 1 and num = 0
                    depth = 1;
                    num = 0;
                } else {
                    // zeroing out the previously calculated num 
                    // dashes represent depth
                    depth++;
                }
                
                continue;
            }

            // else its a number 
            num = 10*num + (traversal.charAt(i) - '0');

        }

        if(num != 0 ) {
            if(st.empty() ) {
                // only 1 element in the tree
                root = new TreeNode(num);
            } else {
                // this is last node like "1-2--3--4-5--6--7" we have to insert 7 in the tree
                insertIntoStack(st , num , depth);
            }
        }

        return root;
    }

    private void insertIntoStack(Stack<TreeNodeDepth> st , int num , int depth ) {

        // insert in the approriate position in the stack 
        // using a stack to simulate dfs

        while(!st.isEmpty() && depth - st.peek().depth != 1 ) {
            // we are trying to reach the possible parent node 
            // parent.depth - child.depth = -1
            st.pop();
        }

        TreeNodeDepth parentDetails = st.peek();
        // we will always get a value for st.peek() it will never be null , because 
        // we try to find the parent and parent will have a depth = node.depth - 1
        TreeNode parent = parentDetails.treeNode;
        TreeNode node = new TreeNode(num); 

        /**
                       parent
                      /.    \  
                    /        \
                node         node1
                /   \.         /
               /.    \        /
              x       y       p

              imagine if p is coming , for p to come 
              node1 has come to the stack before 

              when node1 came , the stack was like 
              {parent , node , x ,  y -> (top of the stack) }

              when node 1 came 
              it obviously popped out 
              y , x , node cause only parent.depth - node1.depth = -1 
              parent.left != null so parent.right = node1
              and the stack = {parent , node1 }

              then p enters and becomes its left child as usual
        * */
        if(parent.left == null ) {
            parent.left = node;
        } else {
            // left child already has been formed
            // so this must be the right child
            parent.right = node;
        }

        // insert into dfs stack 
        st.push(new TreeNodeDepth(node , depth));
    }
}
