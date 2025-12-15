### Introduction
A graph is a non-linear data structure consisting of nodes that have data and are connected to other nodes through edges.
**There are 2 types of graphs:**
1. Undirected Graphs
2. Directed Graphs
---
### Some Terminologies
1. **Node:** Nodes are circles represented by numbers. Nodes are also referred to as vertices. They store the data.
2. **Edge:** Two nodes are connected by a horizontal line called ****Edge. Edge can be directed or undirected.
3. **Path:** The path contains a lot of nodes and each of them is reachable.
4. **Degree:** It is the number of edges that go inside or outside that node.
   `In undirected graphs ⇒ Total degree = 2 x edges`
5. **Edge Weight:** A graph may have weights assigned on its edges. It is often referred to as the cost of the edge.
---
### Questions
**Q1] BFS (Breadth First Search)** 
```java
class Solution {
    public ArrayList<Integer> bfs(ArrayList<ArrayList<Integer>> adj) {
        int V = adj.size();
        boolean[] visited = new boolean[V];
        Queue<Integer> q = new LinkedList<>();
        
        int src = 0;
        visited[src] = true;
        q.add(src);
				
				ArrayList<Integer> res = new ArrayList<>();
        while (!q.isEmpty()) {
            int curr = q.poll();
            res.add(curr);

            // visit all the unvisited neighbours
            for (int x : adj.get(curr)) {
                if (!visited[x]) {
                    visited[x] = true;
                    q.add(x);
                }
            }
        }
        
        return res;
    }
}
```
---
**Q2] DFS (Depth First Search)**
```java
class Solution {
    private void dfsRec(int node, ArrayList<ArrayList<Integer>> adj, boolean[] vis, ArrayList<Integer> res) {
        vis[node] = true;
        res.add(node);
        
        // dfs on all unvisited neighbours
        for (int x: adj.get(node)) {
            if (!vis[x]) {
                dfsRec(x, adj, vis, res);
            }
        }
    }
    public ArrayList<Integer> dfs(ArrayList<ArrayList<Integer>> adj) {
        int V = adj.size();
        boolean[] vis = new boolean[V];
        ArrayList<Integer> res = new ArrayList<>();
        
        dfsRec(0, adj, vis, res);
        return res;
    }
}
```
---
