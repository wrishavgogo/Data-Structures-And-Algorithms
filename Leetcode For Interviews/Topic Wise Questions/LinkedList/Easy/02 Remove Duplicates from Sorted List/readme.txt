Question Link :- https://leetcode.com/problems/remove-duplicates-from-sorted-list/description/?envType=problem-list-v2&envId=linked-list


Q) Given the head of a sorted linked list, delete all duplicates such that each element appears only once. Return the linked list sorted as well.

Input: head = [1,1,2]
Output: [1,2]

Input: head = [1,1,2,3,3]
Output: [1,2,3]


Constraints:

The number of nodes in the list is in the range [0, 300].
-100 <= Node.val <= 100
The list is guaranteed to be sorted in ascending order.



Solution : Java code with comments self explanatory

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
    public ListNode deleteDuplicates(ListNode head) {
        

        // ans stays on the head 
        ListNode ans = head;

        // iterator moves for each value 
        /***
        
            So imagine list 
            is like 

            1->1->1->1->1->2->2->2->3->3->4 

            iterator is at the first 1 
            head is also at the first 1

            head will stay at the first one 
            and iterator will keep move untill it finds a value different from head 

            so iterator will move till it encounters 2 

            and we will make head.next = iterator

            and repeat the process 
            till iterator != null ( basically reaches the end of the stream )
         */
        ListNode iterator = head;

        while(head != null) {

            while(iterator != null && head.val == iterator.val) {
                iterator = iterator.next;
            }

            // now iterator is at a new val diff from head.val

            // in any case if iterator became null 
            // then head.next = null
            // head = head.next which is also null right 
            head.next = iterator;
            head = head.next;
        }


        return ans;
    }
}
