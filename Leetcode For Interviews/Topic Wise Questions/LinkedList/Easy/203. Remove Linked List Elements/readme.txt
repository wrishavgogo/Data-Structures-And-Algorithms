Question Link : https://leetcode.com/problems/remove-linked-list-elements/description/?envType=problem-list-v2&envId=linked-list

Soln : Java code with comments self explanatory 


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
    public ListNode removeElements(ListNode head, int val) {
        
        ListNode ans = null;
        ListNode prevIterator = null; // have a prevPointer  which holds the node already included
        ListNode currentIterator = head; // have a pointer at current node , not sure whether to include or not

        while(currentIterator != null ) {

            if(currentIterator.val == val) {
                // keep moving currentIterator
                currentIterator = currentIterator.next;
                continue;
            } else {
                // current node is valid node 
                if(prevIterator == null ) {
                    // init has not be done 
                    // not a single node yet was valid
                    ans = currentIterator;
                    prevIterator = currentIterator;
                }  else {
                    // init was done already , valid list already existing 
                    prevIterator.next = currentIterator;
                    prevIterator = prevIterator.next;
                }
            }

            
            // since at this point prev and current are pointing to same node 
            // so if we do the detaching before moving current , we loose the original list 
            currentIterator = currentIterator.next;


            // its important to detach this selected list from the actual linked List 
            // for lets say the list was like 
            // 1 - 2 - 2 - 2 - 2 and val = 2 
            // we would select 1 and skip over all the 2's 
            // but since we did not detach prevIterator it would still be pointing to the 2's 
            // as we are not creating new linkedList and doing inplace modifications  
            if(prevIterator != null ) {
                prevIterator.next = null;
            }
            
        }

        return ans;
    }
}
