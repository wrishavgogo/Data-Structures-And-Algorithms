Link : https://leetcode.com/problems/sum-of-nodes-with-even-valued-grandparent/?envType=company&envId=salesforce&favoriteSlug=salesforce-all


Question : Given the root of a binary tree, return the sum of values of nodes with an even-valued grandparent. If there are no nodes with an even-valued grandparent, return 0.

A grandparent of a node is the parent of its parent if it exists.

 

Example 1:


Input: root = [6,7,8,2,7,1,3,9,null,1,4,null,null,null,5]
Output: 18
Explanation: The red nodes are the nodes with even-value grandparent while the blue nodes are the even-value grandparents.
Example 2:


Input: root = [1]
Output: 0
 

Constraints:

The number of nodes in the tree is in the range [1, 104].
1 <= Node.val <= 100

Soln : 
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

    class TreeNodeMetadata {
        TreeNode treeNode;
        boolean isParentEven;
        boolean isGrandParentEven;

        public TreeNodeMetadata(TreeNode treeNode , boolean isParentEven , boolean isGrandParentEven ) {
            this.treeNode = treeNode;
            this.isParentEven = isParentEven;
            this.isGrandParentEven = isGrandParentEven;
        }
    }

    public int sumEvenGrandparent(TreeNode root) {
        
        Queue<TreeNodeMetadata> q = new LinkedList<>();

        // do a simple bfs and in the bfs just check if the nodes grandParent was even or not

        q.offer(new TreeNodeMetadata(root , false , false));
        int sum = 0;

        while(!q.isEmpty()) {

            TreeNodeMetadata nodeMetadata = q.poll();
            TreeNode treeNode = nodeMetadata.treeNode;

            if(nodeMetadata.isGrandParentEven == true ) {
                sum += treeNode.val;
            }

            if(treeNode.left != null ) {
                TreeNodeMetadata leftChildMetadata = new TreeNodeMetadata(treeNode.left ,
                 treeNode.val % 2 == 0  , nodeMetadata.isParentEven);
                q.offer(leftChildMetadata);
            }
            
            if(treeNode.right != null ) {
                TreeNodeMetadata rightChildMetadata = new TreeNodeMetadata(treeNode.right , 
                treeNode.val % 2 == 0  , nodeMetadata.isParentEven); 
                q.offer(rightChildMetadata);
            }


        }

        return sum;

    }


}
