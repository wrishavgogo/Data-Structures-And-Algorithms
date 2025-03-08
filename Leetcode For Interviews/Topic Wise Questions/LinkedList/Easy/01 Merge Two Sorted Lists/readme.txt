Question :  Merge Two Sorted Lists


Merge the two lists into one sorted list. The list should be made by splicing together the nodes of the first two lists.

Return the head of the merged linked list.

Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]
Example 2:

Input: list1 = [], list2 = []
Output: []
Example 3:

Input: list1 = [], list2 = [0]
Output: [0]
 

Constraints:

The number of nodes in both lists is in the range [0, 50].
-100 <= Node.val <= 100
Both list1 and list2 are sorted in non-decreasing order.



Soln : Java code with comments pretty self explanatory

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
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        
        ListNode head = new ListNode();
        ListNode iterator = head;

        if(list1 == null && list2 == null ) {

            // both lists are null 
            return null;
        }

        // case where one of them is null
        if(list1 == null ) {
            return list2;
        }

        if(list2 == null ) {
            return list1;
        }


        // case when both are non null

        while(list1 != null && list2 != null ) {

            if(list1.val <= list2.val) {
                iterator.next = list1;
                list1 = list1.next;
            } else {
                iterator.next = list2;
                list2 = list2.next;
            }
            iterator = iterator.next;
            // extracting from list and detaching it from the list;
            iterator.next = null;
        }

        // any of the list remains traversal 
        // which means any one of them became null
        // simply atttach the list 
        if(list1 != null ) {
            iterator.next = list1;
        }

        if(list2 != null ) {
            iterator.next = list2;
        }

        return head.next;
        
    }
}
