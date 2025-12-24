## Introduction
A graph is a non-linear data structure consisting of nodes that have data and are connected to other nodes through edges.
**There are 2 types of graphs:**
1. Undirected Graphs
2. Directed Graphs
---
## Some Terminologies
1. **Node:** Nodes are circles represented by numbers. Nodes are also referred to as vertices. They store the data.
2. **Edge:** Two nodes are connected by a horizontal line called an Edge. Edge can be directed or undirected.
3. **Path:** The path contains a lot of nodes and each of them is reachable.
4. **Degree:** It is the number of edges that go inside or outside that node.
   `In undirected graphs ⇒ Total degree = 2 x edges`
5. **Edge Weight:** A graph may have weights assigned on its edges. It is often referred to as the cost of the edge.
---
## Questions
### Q1] BFS (Breadth First Search) 
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
### Q2] DFS (Depth First Search)
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
### Q3] Connected Components
**M1: BFS**
```java
class Solution {
    static void bfs(ArrayList<ArrayList<Integer>> adj, int src, boolean[] visited, ArrayList<Integer> res) {
        Queue<Integer> q = new LinkedList<>();
        visited[src] = true;
        q.add(src);

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
    }
    public ArrayList<ArrayList<Integer>> getComponents(int V, int[][] edges) {
        ArrayList<ArrayList<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < V; i++) {
            adj.add(new ArrayList<>());
        }
        
        for (int[] edge: edges) {
            int a = edge[0];
            int b = edge[1];
            adj.get(a).add(b);
            adj.get(b).add(a);
        }
        
        
        boolean[] visited = new boolean[V];
        ArrayList<ArrayList<Integer>> res = new ArrayList<>();

        for (int i = 0; i < V; i++) {
            if (!visited[i]) {
                ArrayList<Integer> component = new ArrayList<>();
                bfs(adj, i, visited, component);
                res.add(component);
            }
        }
        return res;
    }
}
```
**M2: DFS**
```java
class Solution {
    private void dfs(ArrayList<ArrayList<Integer>> adj, int src, boolean[] visited, ArrayList<Integer> res) {
        int V = adj.size();
        visited[src] = true;
        res.add(src);

        for (int x: adj.get(src)) {
            if (!visited[x]) {
                dfs(adj, x, visited, res);
            }
        }
    }
    public ArrayList<ArrayList<Integer>> getComponents(int V, int[][] edges) {
        ArrayList<ArrayList<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < V; i++) {
            adj.add(new ArrayList<>());
        }
        
        for (int[] edge: edges) {
            int a = edge[0];
            int b = edge[1];
            adj.get(a).add(b);
            adj.get(b).add(a);
        }
        
        
        boolean[] visited = new boolean[V];
        ArrayList<ArrayList<Integer>> res = new ArrayList<>();

        for (int i = 0; i < V; i++) {
            if (!visited[i]) {
                ArrayList<Integer> component = new ArrayList<>();
                dfs(adj, i, visited, component);
                res.add(component);
            }
        }
        return res;
    }
}
```
**M3: DSU**
```java

```
---
### Q4] Complete Components
**M1: BFS**
```java
class Solution {
    private int[] bfs(ArrayList<ArrayList<Integer>> adj, int src, boolean[] visited) {
        Queue<Integer> q = new LinkedList<>();
        visited[src] = true;
        q.add(src);

        int nodes = 0;
        int degree = 0;

        while (!q.isEmpty()) {
            int curr = q.poll();
            nodes++;
            degree += adj.get(curr).size();

            // visit all the unvisited neighbours
            for (int x : adj.get(curr)) {
                if (!visited[x]) {
                    visited[x] = true;
                    q.add(x);
                }
            }
        }

        return new int[] {nodes, degree};
    }
    public int countCompleteComponents(int n, int[][] edges) {
        // make adj list
        ArrayList<ArrayList<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            adj.add(new ArrayList<>());
        }
        for (int[] edge: edges) {
            int a = edge[0];
            int b = edge[1];
            adj.get(a).add(b);
            adj.get(b).add(a);
        }
        
        boolean[] visited = new boolean[n];
        int count = 0;
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                int[] res = bfs(adj, i, visited);
                int v = res[0];
                int e = res[1] / 2;

                if (e == v * (v - 1) / 2) {
                    count++;
                }
            }
        }
        return count;
    }
}
```
**M2: DFS**
```java
class Solution {

    private int nodes;
    private int edges;

    private void dfs(int node, ArrayList<ArrayList<Integer>> adj, boolean[] visited) {
        visited[node] = true;
        nodes++;
        edges += adj.get(node).size();

        for (int x : adj.get(node)) {
            if (!visited[x]) {
                dfs(x, adj, visited);
            }
        }
    }

    public int countCompleteComponents(int n, int[][] edgesArr) {
        // build adjacency list
        ArrayList<ArrayList<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            adj.add(new ArrayList<>());
        }
        for (int[] edge : edgesArr) {
            int a = edge[0];
            int b = edge[1];
            adj.get(a).add(b);
            adj.get(b).add(a);
        }

        boolean[] visited = new boolean[n];
        int count = 0;

        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                nodes = 0;
                edges = 0;

                dfs(i, adj, visited);

                int actualEdges = edges / 2;

                if (actualEdges == nodes * (nodes - 1) / 2) {
                    count++;
                }
            }
        }
        return count;
    }
}
```
---
### Q5] Number of Provinces
**M1: BFS**
```java
class Solution {
    private void bfs (int src, int[][] adj, boolean[] visited) {
        int V = adj.length;
        Queue<Integer> q = new LinkedList<>();
        
        visited[src] = true;
        q.add(src);
				
        while (!q.isEmpty()) {
            int curr = q.poll();

            // visit all the unvisited neighbours
            for (int i = 0; i < V; i++) {
                if (!visited[i] && adj[curr][i] == 1) {
                    visited[i] = true;
                    q.add(i);
                }
            }
        }
    }
    public int findCircleNum(int[][] isConnected) {
        int V = isConnected.length;
        boolean[] visited = new boolean[V];
        
        int count = 0;
        for (int i = 0; i < V; i++) {
            if (!visited[i]) {
                bfs (i, isConnected, visited);
                count++;
            }
        }

        return count;
    }
}
```
**M2: DFS**
```java
class Solution {
    private void dfs (int src, int[][] adj, boolean[] visited) {
        int V = adj.length;
        visited[src] = true;

        for (int i = 0; i < V; i++) {
            if (!visited[i] && adj[src][i] == 1) {
                dfs(i, adj, visited);
            }
        }
    }

    public int findCircleNum(int[][] isConnected) {
        int V = isConnected.length;
        boolean[] visited = new boolean[V];
        
        int count = 0;
        for (int i = 0; i < V; i++) {
            if (!visited[i]) {
                dfs(i, isConnected, visited);
                count++;
            }
        }

        return count;
    }
}
```
---

### Q6] Number of Islands
**M1: DFS**
```java
class Solution {
    public int numIslands(char[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        int islands = 0;

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {
                    islands++;
                    dfs(grid, i, j, m, n);
                }
            }
        }
        return islands;
    }

    private void dfs(char[][] grid, int r, int c, int m, int n) {
        if (r < 0 || r >= m || c < 0 || c >= n || grid[r][c] == '0') {
            return;
        }

        grid[r][c] = '0'; // mark visited

        dfs(grid, r + 1, c, m, n);
        dfs(grid, r - 1, c, m, n);
        dfs(grid, r, c + 1, m, n);
        dfs(grid, r, c - 1, m, n);
    }
}

```
**M2: BFS**
```java

```
**M3: DSU**
```java

```
---
