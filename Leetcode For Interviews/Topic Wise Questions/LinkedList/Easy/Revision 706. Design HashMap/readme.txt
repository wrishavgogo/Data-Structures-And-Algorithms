Question : Design a HashMap without using any built-in hash table libraries.

Implement the MyHashMap class:

MyHashMap() initializes the object with an empty map.
void put(int key, int value) inserts a (key, value) pair into the HashMap. If the key already exists in the map, update the corresponding value.
int get(int key) returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key.
void remove(key) removes the key and its corresponding value if the map contains the mapping for the key.
 

Example 1:

Input
["MyHashMap", "put", "put", "get", "get", "put", "get", "remove", "get"]
[[], [1, 1], [2, 2], [1], [3], [2, 1], [2], [2], [2]]
Output
[null, null, null, 1, -1, null, 1, null, -1]

Explanation
MyHashMap myHashMap = new MyHashMap();
myHashMap.put(1, 1); // The map is now [[1,1]]
myHashMap.put(2, 2); // The map is now [[1,1], [2,2]]
myHashMap.get(1);    // return 1, The map is now [[1,1], [2,2]]
myHashMap.get(3);    // return -1 (i.e., not found), The map is now [[1,1], [2,2]]
myHashMap.put(2, 1); // The map is now [[1,1], [2,1]] (i.e., update the existing value)
myHashMap.get(2);    // return 1, The map is now [[1,1], [2,1]]
myHashMap.remove(2); // remove the mapping for 2, The map is now [[1,1]]
myHashMap.get(2);    // return -1 (i.e., not found), The map is now [[1,1]]
 

Constraints:

0 <= key, value <= 106
At most 104 calls will be made to put, get, and remove.


Question : 



class MyHashMap {

    final int BUCKET_SIZE = 777;
    AVLTree[] buckets;
    
    public MyHashMap() {
        buckets = new AVLTree[BUCKET_SIZE];
    }

    public void put(int key, int value) {
        int index = getIndex(key);
        if(buckets[index] != null ) {
            buckets[index].put(key , value);
        }
        else {
            buckets[index] = new AVLTree(key , value);
        }
    }

    public int get(int key) {
        int index = getIndex(key);
        if(buckets[index] != null ) {
            return buckets[index].get(key);
        }
        return -1;
    }

    public void remove(int key) {
        int index = getIndex(key);
        if(buckets[index] != null ) {
            buckets[index].remove(key);
        }
    }
    
    private int getIndex(int key) {
        return key % BUCKET_SIZE;
    }
}

class Node {
    Entry entry;
    Node left;
    Node right;
    int height;

    public Node(Entry entry, Node left , Node right) {
        this.entry = entry;
        this.left = left;
        this.right = right;
        height = 1;
    }
}

class Entry {
    
    int key;
    int value;
    
    public Entry(int key , int value) {
        this.key = key;
        this.value = value;
    }
}

class AVLTree{

    Node root;

    public AVLTree(int key , int value) {
        root = new Node(new Entry(key , value) , null , null);
    }

    public int get(int key) {
        return getKey(key , root);
    }

    public void put(int key , int value) {
        
        root =  putEntry(new Entry(key , value) , root);
    }

    public void remove(int key) {
        root = delete(key , root);
    }

    private int getKey(int key , Node node) {

        if(node == null ) {
            return -1;
        } else if(node.entry.key > key) {
            return getKey(key , node.left);
        } else if(node.entry.key < key) {
            return getKey(key , node.right);
        }
        // we found the node and we are at the node
        return node.entry.value;
    }

    private Node putEntry(Entry entry,  Node node) {

        if(node == null ) {
            return new Node(entry, null , null);
        } else if(node.entry.key > entry.key ) {
            node.left = putEntry(entry, node.left);
        } else if(node.entry.key < entry.key ) {
            node.right = putEntry(entry , node.right);
        } else {
            // update the corresponding value thats it
            node.entry = entry;
            return node;
        }
        
        node.height = 1 + Math.max(getHeight(node.left) , getHeight(node.right));
        
        int balanceFactor = getBalanceFactor(node);
        
        // LL
        if(balanceFactor > 1 && node.left.entry.key > entry.key ) {
            return rightRotate(node);
        }
        
        // LR
        if(balanceFactor > 1 && node.left.entry.key < entry.key ) {
            node.left = leftRotate(node.left);
            return rightRotate(node);
        }

        // RR
        if(balanceFactor < -1 && node.right.entry.key < entry.key ) {
            return leftRotate(node);
        }

        // RL
        if(balanceFactor < -1 && node.right.entry.key > entry.key ) {
            node.right = rightRotate(node.right);
            return leftRotate(node);
        }
        
        return node;
        
    }
    
    private Node delete(int key , Node node) {
        
        if(node == null ) {
            return null;
        } else if(node.entry.key > key) {
            node.left = delete(key , node.left);
        } else if(node.entry.key < key) {
            node.right = delete(key , node.right);
        } else {
            // this the key to be deleted
            
            // case 1: 1 or zero children 
            if(node.left == null || node.right == null ) {
                return node.left != null ? node.left : node.right;
            }
            
            // case 2 : has both the children
            node.entry = inorderSuccessorValue(node);
            node.right = delete(node.entry.key , node.right);
        }
        
        node.height = 1 + Math.max(getHeight(node.left) , getHeight(node.right));
        
        int balanceFactor = getBalanceFactor(node);
        
        
        // LL case
        if(balanceFactor > 1 && getBalanceFactor(node.left) >= 0 ) {
            return rightRotate(node);
        }

        // LR case
        if(balanceFactor > 1 && getBalanceFactor(node.left) < 0 ) {
            node.left = leftRotate(node.left);
            return rightRotate(node);
        }

        // RR case
        if(balanceFactor < -1 && getBalanceFactor(node.right) < 0 ) {
            return leftRotate(node);
        }
        
        // RL case 
        if(balanceFactor < -1 && getBalanceFactor(node.right) >= 0 ) {
            node.right = rightRotate(node.right);
            return leftRotate(node);
        }
        
        return node;
        
    }
    
    private Entry inorderSuccessorValue(Node x) {
        
        Entry entry = x.entry;
        Node y = x.right;
        while(y != null) {
            entry = y.entry;
            y = y.left;
        }
        
        return entry;
    }
    
    /**
     *          
     *          X
     *         / \
     *        y
     *         \
     *          T
     * 
     * */
    private Node rightRotate(Node x) {
        
        Node y = x.left;
        Node T = y.right;
        
        // remap
        x.left = T;
        y.right = x;
        
        // height calc
        x.height = 1 + Math.max(getHeight(x.left) , getHeight(x.right));
        y.height = 1 + Math.max(getHeight(y.left) , getHeight(y.right));
        
        return y;
    }

    /**
     *
     *          X
     *         / \
     *            y
     *           /
     *          T
     *
     * */
    private Node leftRotate(Node x) {

        Node y = x.right;
        Node T = y.left;

        // remap
        x.right = T;
        y.left = x;

        // height calc
        x.height = 1 + Math.max(getHeight(x.left) , getHeight(x.right));
        y.height = 1 + Math.max(getHeight(y.left) , getHeight(y.right));

        return y;
    }
    
    
    
    
    private int getHeight(Node x) {
        return x != null ? x.height : 0;
    }
    
    private int getBalanceFactor(Node x) {
        
        if(x == null ) {
            return 0;
        }
        return getHeight(x.left) - getHeight(x.right);
    }


}

/**
 * Your MyHashMap object will be instantiated and called as such:
 * MyHashMap obj = new MyHashMap();
 * obj.put(key,value);
 * int param_2 = obj.get(key);
 * obj.remove(key);
 */


