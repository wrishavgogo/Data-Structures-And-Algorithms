Question : https://leetcode.com/problems/reverse-linked-list/description/?envType=problem-list-v2&envId=linked-list 


Solution Java : Come on now 

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
    public ListNode reverseList(ListNode head) {
        if(head == null) {
            return head;
        }

        ListNode prev = null;
        ListNode next = null;
        ListNode node = head;
        while(node != null) {
            next = node.next;
            node.next = prev;
            prev = node;
            node = next;
        }
        return prev;
    }
}
