Question Link : https://leetcode.com/problems/convert-doubly-linked-list-to-array-i/description/?envType=problem-list-v2&envId=linked-list

You are given the head of a doubly linked list, which contains nodes that have a next pointer and a previous pointer.

Return an integer array which contains the elements of the linked list in order.

 

Example 1:

Input: head = [1,2,3,4,3,2,1]

Output: [1,2,3,4,3,2,1]

Example 2:

Input: head = [2,2,2,2,2]

Output: [2,2,2,2,2]

Example 3:

Input: head = [3,2,3,2,3,2]

Output: [3,2,3,2,3,2]

 

Constraints:

The number of nodes in the given list is in the range [1, 50].
1 <= Node.val <= 50


Answer : 

/*
// Definition for a Node.
class Node {
    public int val;
    public Node prev;
    public Node next;
};
*/

class Solution {
    public int[] toArray(Node head) {
        // First, count the number of nodes in the linked list to determine the size of the array.
        int size = 0;
        Node current = head;
        while (current != null) {
            size++;
            current = current.next;
        }

        // Create an array to hold the values from the linked list.
        int[] result = new int[size];
        current = head;
        int index = 0;

        // Populate the array with the values from the linked list.
        while (current != null) {
            result[index++] = current.val;
            current = current.next;
        }

        return result;
    }
}
