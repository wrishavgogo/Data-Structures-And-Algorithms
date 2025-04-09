Link : https://leetcode.com/problems/is-graph-bipartite/description/?envType=problem-list-v2&envId=graph

Question : 

There is an undirected graph with n nodes, where each node is numbered between 0 and n - 1. You are given a 2D array graph, where graph[u] is an array of nodes that node u is adjacent to. More formally, for each v in graph[u], there is an undirected edge between node u and node v. The graph has the following properties:

There are no self-edges (graph[u] does not contain u).
There are no parallel edges (graph[u] does not contain duplicate values).
If v is in graph[u], then u is in graph[v] (the graph is undirected).
The graph may not be connected, meaning there may be two nodes u and v such that there is no path between them.
A graph is bipartite if the nodes can be partitioned into two independent sets A and B such that every edge in the graph connects a node in set A and a node in set B.

Return true if and only if it is bipartite.

 

Example 1:


Input: graph = [[1,2,3],[0,2],[0,1,3],[0,2]]
Output: false
Explanation: There is no way to partition the nodes into two independent sets such that every edge connects a node in one and a node in the other.
Example 2:


Input: graph = [[1,3],[0,2],[1,3],[0,2]]
Output: true
Explanation: We can partition the nodes into two sets: {0, 2} and {1, 3}.
 

Constraints:

graph.length == n
1 <= n <= 100
0 <= graph[u].length < n
0 <= graph[u][i] <= n - 1
graph[u] does not contain u.
All the values of graph[u] are unique.
If graph[u] contains v, then graph[v] contains u.


Soln : read explanation .pdf 

class Solution {
    public boolean isBipartite(int[][] graph) {
        
        int n = graph.length;
        List<List<Integer>> adj = new ArrayList<>();
        int[] color = new int[n];
        Arrays.fill(color , -1);
        // we will be filling colors as 0 and 1 

        for(int i = 0 ; i < n ; i ++ ) {
            adj.add(new ArrayList<>());
        }


        for(int i = 0 ; i < n ; i ++ ) { 
            for(int j = 0 ; j < graph[i].length ; j ++ ) { 

                int u = i;
                int v = graph[i][j];

                adj.get(u).add(v);
                adj.get(v).add(u);

            }
        }

        // lets start with 0 color 0 as 0 

        for(int i = 0 ; i < n ; i ++ ) {
            
            // looping as graph can be disconnected 
            if(color[i] != -1) {
                // already visited
                continue;
            }

            color[i] = 0;
            Queue<Integer> q = new LinkedList<>();
            q.offer(i);

            while(!q.isEmpty()) {
            
                int node = q.poll();
                for(int j = 0 ; j < adj.get(node).size() ; j ++ ) {

                    int child = adj.get(node).get(j);
                    if(color[child] == color[node]) {
                        // not bipartite
                        return false;
                    }

                    if(color[child] == -1) {
                        // reversing it 
                        // -1 meaning its not visited
                        color[child] = 1 - color[node];
                        q.offer(child);
                    }
                }

            }


        }
        

        

        return true;

    }
}
