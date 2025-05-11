Link : https://leetcode.com/problems/maximum-sum-of-edge-values-in-a-graph/description/

Question : 

You are given an undirected graph of n nodes, numbered from 0 to n - 1. Each node is connected to at most 2 other nodes.

The graph consists of m edges, represented by a 2D array edges, where edges[i] = [ai, bi] indicates that there is an edge between nodes ai and bi.

You have to assign a unique value from 1 to n to each node. The value of an edge will be the product of the values assigned to the two nodes it connects.

Your score is the sum of the values of all edges in the graph.

Return the maximum score you can achieve.

 

Example 1:


Input: n = 7, edges = [[0,1],[1,2],[2,0],[3,4],[4,5],[5,6]]

Output: 130

Explanation:

The diagram above illustrates an optimal assignment of values to nodes. The sum of the values of the edges is: (7 * 6) + (7 * 5) + (6 * 5) + (1 * 3) + (3 * 4) + (4 * 2) = 130.

Example 2:


Input: n = 6, edges = [[0,3],[4,5],[2,0],[1,3],[2,4],[1,5]]

Output: 82

Explanation:

The diagram above illustrates an optimal assignment of values to nodes. The sum of the values of the edges is: (1 * 2) + (2 * 4) + (4 * 6) + (6 * 5) + (5 * 3) + (3 * 1) = 82.

 

Constraints:

1 <= n <= 5 * 104
m == edges.length
1 <= m <= n
edges[i].length == 2
0 <= ai, bi < n
ai != bi
There are no repeated edges.
Each node is connected to at most 2 other nodes.


Code : 

class Solution {
    public long maxScore(int n, int[][] edges) {
        
        
        
        List<List<Integer>> graph = new ArrayList<>();
        int[] val = new int[n];
        int[] degree = new int[n];
        
        for(int i = 0 ; i < n ; i ++ ) {
            
            graph.add(new ArrayList<>());
        }
        
        for(int i = 0 ; i < edges.length ; i ++ ) {
            
            int u = edges[i][0];
            int v = edges[i][1];
            
            degree[u]++;
            degree[v]++;
            
            graph.get(u).add(v);
            graph.get(v).add(u);
        }
        
        
        int maxValue = n;
        
        Set<Integer> visitedSet = new HashSet<>();
        
        for(int i = 0 ; i < n ; i ++ ) {
            
            
            if(isCycle(graph , i) && !visitedSet.contains(i)) {
                maxValue = assignValueInCycle(graph , i , val , visitedSet , maxValue);
            }
        }
        
        
        for(int i = 0 ; i < n ; i ++ ) {
            
            if(degree[i] == 1 && !visitedSet.contains(i)) {
                maxValue = assignValueNormally(graph , i , val , visitedSet , maxValue);
            }
            
        }
        
        visitedSet.clear();
        
        long sum = 0;
        
        for(int i = 0 ; i < n ; i ++ ) {
            
            sum += getSum(graph , visitedSet , i , val);
        }
        
        
        return sum;
        
        
    }
    
    private long getSum(List<List<Integer>> graph  ,  Set<Integer> visitedSet , int node , int[] val) {
        
        
        visitedSet.add(node);
        Queue<Integer> q = new LinkedList<>();
        q.offer(node);
        
        long sum = 0;
        while(!q.isEmpty()) {
            
            int parent = q.poll();
            
            for(int i = 0 ; i < graph.get(parent).size() ; i ++ ) {
                
                int child = graph.get(parent).get(i);
                if(!visitedSet.contains(child)) {
                    
                    q.offer(child);
                    visitedSet.add(child);
                    sum += (long)(val[parent]) * (long)(val[child]);
                }
            }
        }

        
        
        return sum;
    }
    
    private int assignValueNormally(List<List<Integer>> graph , int node , int[] val ,Set<Integer> visitedSet, int maxValue ) {
        
        
        visitedSet.add(node);
        Queue<Integer> q = new LinkedList<>();
        q.offer(node);
        List<Integer> chain = new ArrayList<>();

        while(!q.isEmpty()) {
            
            int parent = q.poll();
            chain.add(parent);
            
            for(int i = 0; i < graph.get(parent).size() ; i ++) {
                int child = graph.get(parent).get(i);
                if(!visitedSet.contains(child)) {
                    visitedSet.add(child);  
                }
            }
        }

        int mid = -1;
        if(chain.size() % 2 == 1 ) { 
            mid = chain.size() / 2;
        } else {
            mid = chain.size() / 2 ;
            mid--;
        }

        int i = mid;
        int j = mid + 1 ;

        while(i >= 0 && j < chain.size()) {

            val[chain.get(i)] = maxValue;
            maxValue--;
            i--;
            val[chain.get(j)] = maxValue;
            maxValue--;
            j++;
        }
        
        return maxValue;
    }
    
    
    
    private int assignValueInCycle(List<List<Integer>> graph , int node , int[] val ,  Set<Integer> visitedSet , int maxValue ) {
        
        
        
        visitedSet.add(node);
        val[node] = maxValue;
        maxValue--;
        
        Queue<Integer> q = new LinkedList<>();
        q.offer(node);
        
        
        while(!q.isEmpty()) {
            
            int parent = q.poll();
            
            for(int i = 0 ; i < graph.get(parent).size() ; i ++ ) {
                
                int child = graph.get(parent).get(i);
                if(!visitedSet.contains(child)) {
                    val[child] = maxValue;
                    maxValue--;
                    visitedSet.add(child);
                    q.offer(child);
                }
            }
            
        }
        
        
        
        return maxValue;
        
    }
    
    
    private boolean isCycle(List<List<Integer>> graph , int node ) {
        
        
        Set<Integer> visited = new HashSet<>();
        Queue<Integer> q = new LinkedList<>();
        visited.add(node);
        q.offer(node);
        
        while(!q.isEmpty()) {
            
           int parent = q.poll();
            
           for(int i = 0 ; i < graph.get(parent).size() ; i ++ ) {
               
               int child = graph.get(parent).get(i);
               if(child != node ) {
                   
                   if(visited.contains(child)) {
                       return true;
                   }
                   
                   visited.add(child);
                   q.offer(child);
                   
               }
           } 
            
        }
        
        
        return false;
    }
}
