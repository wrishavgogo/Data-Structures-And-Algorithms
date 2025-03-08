Premium 
Question Link : https://leetcode.com/problems/delete-n-nodes-after-m-nodes-of-a-linked-list/description/?envType=problem-list-v2&envId=linked-list

You are given the head of a linked list and two integers m and n.

Traverse the linked list and remove some nodes in the following way:

Start with the head as the current node.
Keep the first m nodes starting with the current node.
Remove the next n nodes
Keep repeating steps 2 and 3 until you reach the end of the list.
Return the head of the modified list after removing the mentioned nodes.


Input: head = [1,2,3,4,5,6,7,8,9,10,11,12,13], m = 2, n = 3
Output: [1,2,6,7,11,12]
Explanation: Keep the first (m = 2) nodes starting from the head of the linked List  (1 ->2) show in black nodes.
Delete the next (n = 3) nodes (3 -> 4 -> 5) show in read nodes.
Continue with the same procedure until reaching the tail of the Linked List.
Head of the linked list after removing nodes is returned.


Input: head = [1,2,3,4,5,6,7,8,9,10,11], m = 1, n = 3
Output: [1,5,9]
Explanation: Head of linked list after removing nodes is returned.


Constraints:

The number of nodes in the list is in the range [1, 104].
1 <= Node.val <= 106
1 <= m, n <= 1000
 

Follow up: Could you solve this problem by modifying the list in-place?



Solution : Java code with comments also go through the excaliDraw explanation that i did myself 


/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode deleteNodes(ListNode head, int m, int n) {
        
    
        ListNode iterator = head;

        int counter = 0;

        while(iterator != null ) {

            counter++;

            switch(counter % 2 ) {

                case (1) : {
                    // you need to keep the first m nodes 
                    int keptNodes = 1;

                    while(iterator != null && keptNodes < m ) {
                        // as you know if java keepNodeCount-- wont work 
                        // as it does not convert Integer to boolean implicityly 
                        // in while or if condition 
                        keptNodes++;
                        iterator = iterator.next;
                        
                    }

                }
                    break;

                default : {
                    // 2nd process remove the next n nodes 
                    ListNode removerIterator = iterator;
                    int nodesRemoved = 0;

                    while(removerIterator != null && nodesRemoved <= n ) {
                        
                        removerIterator = removerIterator.next;
                        nodesRemoved++;
                    }

                    // stiching iterator to removerIterator
                    iterator.next = removerIterator;
                    iterator = iterator.next;

                    
                }
            }
        }

        // head is always the starting node which will always stay so fine 
        return head;
    }
}
