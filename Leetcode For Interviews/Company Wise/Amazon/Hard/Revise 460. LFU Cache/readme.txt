Link : https://leetcode.com/problems/lfu-cache/description/?envType=company&envId=amazon&favoriteSlug=amazon-three-months

Question: 

Design and implement a data structure for a Least Frequently Used (LFU) cache.

Implement the LFUCache class:

LFUCache(int capacity) Initializes the object with the capacity of the data structure.
int get(int key) Gets the value of the key if the key exists in the cache. Otherwise, returns -1.
void put(int key, int value) Update the value of the key if present, or inserts the key if not already present. When the cache reaches its capacity, it should invalidate and remove the least frequently used key before inserting a new item. For this problem, when there is a tie (i.e., two or more keys with the same frequency), the least recently used key would be invalidated.
To determine the least frequently used key, a use counter is maintained for each key in the cache. The key with the smallest use counter is the least frequently used key.

When a key is first inserted into the cache, its use counter is set to 1 (due to the put operation). The use counter for a key in the cache is incremented either a get or put operation is called on it.

The functions get and put must each run in O(1) average time complexity.

 

Example 1:

Input
["LFUCache", "put", "put", "get", "put", "get", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [3], [4, 4], [1], [3], [4]]
Output
[null, null, null, 1, null, -1, 3, null, -1, 3, 4]

Explanation
// cnt(x) = the use counter for key x
// cache=[] will show the last used order for tiebreakers (leftmost element is  most recent)
LFUCache lfu = new LFUCache(2);
lfu.put(1, 1);   // cache=[1,_], cnt(1)=1
lfu.put(2, 2);   // cache=[2,1], cnt(2)=1, cnt(1)=1
lfu.get(1);      // return 1
                 // cache=[1,2], cnt(2)=1, cnt(1)=2
lfu.put(3, 3);   // 2 is the LFU key because cnt(2)=1 is the smallest, invalidate 2.
                 // cache=[3,1], cnt(3)=1, cnt(1)=2
lfu.get(2);      // return -1 (not found)
lfu.get(3);      // return 3
                 // cache=[3,1], cnt(3)=2, cnt(1)=2
lfu.put(4, 4);   // Both 1 and 3 have the same cnt, but 1 is LRU, invalidate 1.
                 // cache=[4,3], cnt(4)=1, cnt(3)=2
lfu.get(1);      // return -1 (not found)
lfu.get(3);      // return 3
                 // cache=[3,4], cnt(4)=1, cnt(3)=3
lfu.get(4);      // return 4
                 // cache=[4,3], cnt(4)=2, cnt(3)=3
 

Constraints:

1 <= capacity <= 104
0 <= key <= 105
0 <= value <= 109
At most 2 * 105 calls will be made to get and put.


Solution :  Go through the explanation pdf for this before coming here , also checkout 
the programming languages repo , java on 6th april 2025 , where we learnt about LinkedHashMap and LinkedHashSet 
they both being such handy datastructes 


Code : 

class LFUCache {

    class Node{

        int freq;
        Node prev;
        Node next;
        LinkedHashSet<Integer> lruKeys; // to utilize LRU

        public Node(int freq) {
            this.freq = freq;
            lruKeys = new LinkedHashSet<Integer>();
        }
    }

    Node head;
    Node tail;
    Map<Integer , Node> keyToNodeMap;
    Map<Integer , Integer> keyToValueMap;
    int capacity;

    public LFUCache(int capacity) {
        head = new Node(-1);
        tail = new Node(-1);
        head.next = tail;
        tail.prev = head;
        keyToNodeMap = new HashMap<>();
        keyToValueMap = new HashMap<>();
        this.capacity = capacity;
    }

    // in the DLL we are going to store in increasing order
    // from left to right 
    // head <-> minFreqNode <-> 2nd minimum ......... <-> maxFreqNode <-> Tail 
    
    public int get(int key) {
        
        if(!keyToValueMap.containsKey(key)) {
            // key not present 
            return -1;
        }

        int value = keyToValueMap.get(key);
        // simulate reput using the old values 
        put(key , value);
        return value;
    }
    
    public void put(int key, int value) {
        
        // key already exists
        if(keyToValueMap.containsKey(key)) {

            // new value assigned
            keyToValueMap.put(key , value);
            
            Node node = keyToNodeMap.get(key);
            keyToNodeMap.remove(key);

            int prevFreq = node.freq;
            node.lruKeys.remove(key);

            int newFreq = prevFreq + 1;

            if(node.next.freq == newFreq ) {
                node.next.lruKeys.add(key);
                keyToNodeMap.put(key , node.next);
            } else {
                Node newNode = new Node(newFreq);
                newNode.lruKeys.add(key);
                keyToNodeMap.put(key , newNode);
                insertNode(node , node.next , newNode);
            }

            if(node.lruKeys.size() == 0 ) {
                removeNode(node);
            }

        }  else {
            // key does not exist 
            if(capacity == keyToValueMap.size()) {
                // cache is full , eviction needs to be done 
                evict();
            }
            
            if(head.next.freq != 1 || head.next == tail) {
                // minFreq != 1 
                // or maybe capacity == 1 and evict removed the 1 node then also newNode has to be created

                Node newNode = new Node(1);
                newNode.lruKeys.add(key);
                keyToNodeMap.put(key , newNode);
                keyToValueMap.put(key , value);  
                insertNode(head , head.next , newNode);
            } else {
                Node node = head.next;
                node.lruKeys.add(key);
                keyToNodeMap.put(key , node);
                keyToValueMap.put(key , value);  
            }

        }
    }

    public void evict() {

        if(head.next == tail ) {
            return;
        }

        Node minNode = head.next;
        int keyToEvict = minNode.lruKeys.iterator().next();

        // remove the key from the maps
        keyToNodeMap.remove(keyToEvict);
        keyToValueMap.remove(keyToEvict);

        // remove key from the lru cache of the node
        minNode.lruKeys.remove(keyToEvict);
        if(minNode.lruKeys.size() == 0 ) {
            removeNode(minNode);
        }

    }

    public void insertNode(Node firstNode , Node secondNode , Node toBeInsertedNode) {

        firstNode.next = toBeInsertedNode;
        secondNode.prev = toBeInsertedNode;

        toBeInsertedNode.prev = firstNode;
        toBeInsertedNode.next = secondNode;
    }

    public void removeNode(Node node) {

        Node prevNode = node.prev;
        Node nextNode = node.next;

        node.prev = null;
        node.next = null;

        prevNode.next = nextNode;
        nextNode.prev = prevNode;
    }
}

/**
 * Your LFUCache object will be instantiated and called as such:
 * LFUCache obj = new LFUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
 
