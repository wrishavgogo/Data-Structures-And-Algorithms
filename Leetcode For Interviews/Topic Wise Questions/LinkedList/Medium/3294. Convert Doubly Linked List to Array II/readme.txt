Question Premium , 

It should be marked easy question 

Link : https://leetcode.com/problems/convert-doubly-linked-list-to-array-ii/description/?envType=problem-list-v2&envId=linked-list

Soln : 

SINCE DOUBLE LL , so move to head  , where node.prev == null , so thats the head , 


/*
// Definition for a Node.
class Node {
    public int val;
    public Node prev;
    public Node next;
};
*/

class Solution {
    public int[] toArray(Node node) {
        List<Integer> list = new ArrayList<>();
        Node t = node;
        while (t != null) {
            list.add(t.val);
            t = t.next;
        }
        t = node.prev;
        while (t != null) {
            list.add(0, t.val);
            t = t.prev;
        }
        int[] arr = new int[list.size()];
        for (int i = 0; i < list.size(); i++) {
            arr[i] = list.get(i);
        }
        return arr;
    }
}
