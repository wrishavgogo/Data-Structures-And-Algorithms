Link : https://leetcode.com/problems/all-possible-full-binary-trees/description/?envType=problem-list-v2&envId=dynamic-programming

Question : 

Given an integer n, return a list of all possible full binary trees with n nodes. Each node of each tree in the answer must have Node.val == 0.

Each element of the answer is the root node of one possible tree. You may return the final list of trees in any order.

A full binary tree is a binary tree where each node has exactly 0 or 2 children.

 

Example 1:


Input: n = 7
Output: [[0,0,0,null,null,0,0,null,null,0,0],[0,0,0,null,null,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,null,null,null,null,0,0],[0,0,0,0,0,null,null,0,0]]
Example 2:

Input: n = 3
Output: [[0,0,0]]
 

Constraints:

1 <= n <= 20


Code with comments and explanation : 

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
    public List<TreeNode> allPossibleFBT(int n) {
        
        List<TreeNode> treeNodeList = new ArrayList<>();

        // never possible to build full binary tree using even number of nodes 
        if(n % 2 == 0 ) {
            return treeNodeList;
        }

        if(n == 1 ) {
            // only 1 node 
            treeNodeList.add(new TreeNode(0));
            return treeNodeList;
        }

        // more than 1 node 
        //  root node consider as 1 node 
        //  left you can assign x nodes or the right you can assign n - 1 - x nodes 
        // x can start from 1 , because the root node this current node 
        // needs to have atleast 1 in the left (as we want 0 or 2 children) rest in the right 

        // next iteration it should have 3 , because no matter what we cannot pass
        // even number of nodes to the function 

        // x is always odd so n - 1 - x will always be odd as well , because n is odd 
        // n -1 is even , and n - 1 - odd will be odd as well 


        for(int i = 1 ; i <= n - 1   ; i += 2  ) {
            // we cannot take n - 1 nodes ,as we cannot send 0 nodes to the right function call
            

            int numberOfNodesInLeft = i;
            int numberOfNodesInRight = n - 1 - i;

            if(numberOfNodesInRight <= 0 ) {
                continue;
            }

            List<TreeNode> leftRootList = allPossibleFBT(numberOfNodesInLeft);
            List<TreeNode> rightRootList = allPossibleFBT(numberOfNodesInRight);

            //System.out.println(n + " leftTreeSize = " + i + " - " + leftRootList.size() + 
            //                    " rightTreeSize = " + (n - 1 - i) + " - " + rightRootList.size());
            // now all combinations have to be merged 

            for(int k = 0 ; k < leftRootList.size() ; k++ ) {
                for(int l = 0 ; l < rightRootList.size(); l ++  ) {
                    TreeNode root = new TreeNode(0);
                    root.left = leftRootList.get(k);
                    root.right = rightRootList.get(l);
                    treeNodeList.add(root);
                }
            }
        }

        return treeNodeList;

    }
}



A certain bit of optimisation , introducing cache to store precalculated results 

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

    Map<Integer , List<TreeNode>> cache = new HashMap<>();

    public List<TreeNode> allPossibleFBT(int n) {
        
        List<TreeNode> treeNodeList = new ArrayList<>();

        // never possible to build full binary tree using even number of nodes 
        if(n % 2 == 0 ) {
            return treeNodeList;
        }

        if(n == 1 ) {
            // only 1 node 
            treeNodeList.add(new TreeNode(0));
            return treeNodeList;
        }

        if(cache.containsKey(n)) {
            return cache.get(n);
        }

        // more than 1 node 
        //  root node consider as 1 node 
        //  left you can assign x nodes or the right you can assign n - 1 - x nodes 
        // x can start from 1 , because the root node this current node 
        // needs to have atleast 1 in the left (as we want 0 or 2 children) rest in the right 

        // next iteration it should have 3 , because no matter what we cannot pass
        // even number of nodes to the function 

        // x is always odd so n - 1 - x will always be odd as well , because n is odd 
        // n -1 is even , and n - 1 - odd will be odd as well 


        for(int i = 1 ; i <= n - 1   ; i += 2  ) {
            // we cannot take n - 1 nodes ,as we cannot send 0 nodes to the right function call
            

            int numberOfNodesInLeft = i;
            int numberOfNodesInRight = n - 1 - i;

            if(numberOfNodesInRight <= 0 ) {
                continue;
            }

            List<TreeNode> leftRootList = allPossibleFBT(numberOfNodesInLeft);
            List<TreeNode> rightRootList = allPossibleFBT(numberOfNodesInRight);

            //System.out.println(n + " leftTreeSize = " + i + " - " + leftRootList.size() + 
            //                    " rightTreeSize = " + (n - 1 - i) + " - " + rightRootList.size());
            // now all combinations have to be merged 

            for(int k = 0 ; k < leftRootList.size() ; k++ ) {
                for(int l = 0 ; l < rightRootList.size(); l ++  ) {
                    TreeNode root = new TreeNode(0);
                    root.left = leftRootList.get(k);
                    root.right = rightRootList.get(l);
                    treeNodeList.add(root);
                }
            }
        }

        cache.put(n , treeNodeList);
        return treeNodeList;

    }
}
