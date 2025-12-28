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
### Q7] Flood Fill

---
### Q16] Detect Cycle in Directed Graph
**M1: DFS**
>[!INFO] use path-visited
```java
class Solution {
    public boolean isCyclic(int V, int[][] edges) {
        // build adjacency list
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < V; i++) {
            adj.add(new ArrayList<>());
        }
        for (int[] e : edges) {
            adj.get(e[0]).add(e[1]);
        }

        boolean[] visited = new boolean[V];
        boolean[] pathVisited = new boolean[V];

        for (int i = 0; i < V; i++) {
            if (!visited[i]) {
                if (dfs(i, adj, visited, pathVisited)) {
                    return true;
                }
            }
        }
        return false;
    }

    private boolean dfs(int node, List<List<Integer>> adj, boolean[] visited, boolean[] pathVisited) {
        visited[node] = true;
        pathVisited[node] = true;

        for (int neighbor : adj.get(node)) {
            if (!visited[neighbor]) {
                if (dfs(neighbor, adj, visited, pathVisited)) {
                    return true;
                }
            } else if (pathVisited[neighbor]) {
                return true;
            }
        }

        pathVisited[node] = false;
        return false;
    }
}
```
**M2: Use BFS (Kahn's Algorithm)**
```java
class Solution {
    public boolean isCyclic(int V, int[][] edges) {
        List<List<Integer>> adj = new ArrayList<>();
        int[] indegree = new int[V];
        
        for (int i = 0; i < V; i++) adj.add(new ArrayList<>());
        for (int[] e : edges) {
            adj.get(e[0]).add(e[1]);
            indegree[e[1]]++;
        }
        
        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < V; i++) {
            if (indegree[i] == 0) q.add(i);
        }
        
        int processed = 0;
        while (!q.isEmpty()) {
            int front = q.poll();
            processed++;
            for (int n: adj.get(front)) {
                indegree[n]--;
                if (indegree[n] == 0) q.add(n);
            }
        }
        
        return processed != V;
    }
}
```
---
### Q17] Topological Sort
>[!INFO] can only be applied on DAG's

**M1: BFS**
```java
class Solution {
    public ArrayList<Integer> topoSort(int V, int[][] edges) {
        // build adjacency list
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < V; i++) {
            adj.add(new ArrayList<>());
        }
        for (int[] e : edges) {
            adj.get(e[0]).add(e[1]);
        }

        boolean[] visited = new boolean[V];
        Stack<Integer> st = new Stack<>();
        for (int i = 0; i < V; i++) {
            if (!visited[i]) {
                dfs(i, adj, visited, st);
            }
        }

        ArrayList<Integer> res = new ArrayList<>();
        while (!st.isEmpty()) {
            res.add(st.pop());
        }
        return res;
    }

    private void dfs(int node, List<List<Integer>> adj, boolean[] visited, Stack<Integer> st) {
        visited[node] = true;

        // dfs on all unvisited neighbours
        for (int x: adj.get(node)) {
            if (!visited[x]) {
                dfs(x, adj, visited, st);
            }
        }

        st.push(node);
    }
}
```
**M2: Using BFS (Kahn's Algorithm)**
> [!INFO] use indegree

```java
class Solution {
    public ArrayList<Integer> topoSort(int V, int[][] edges) {
        // build adjacency list
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < V; i++) {
            adj.add(new ArrayList<>());
        }
        for (int[] e : edges) {
            adj.get(e[0]).add(e[1]);
        }

        boolean[] visited = new boolean[V];
        Stack<Integer> st = new Stack<>();
        for (int i = 0; i < V; i++) {
            if (!visited[i]) {
                dfs(i, adj, visited, st);
            }
        }

        ArrayList<Integer> res = new ArrayList<>();
        while (!st.isEmpty()) {
            res.add(st.pop());
        }
        return res;
    }

    private void dfs(int node, List<List<Integer>> adj, boolean[] visited, Stack<Integer> st) {
        visited[node] = true;

        // dfs on all unvisited neighbours
        for (int x: adj.get(node)) {
            if (!visited[x]) {
                dfs(x, adj, visited, st);
            }
        }

        st.push(node);
    }
}
```
---
### Q18] Course Schedule
**M1: BFS (Kahn's Algorithm)**
```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < numCourses; i++) {
            adj.add(new ArrayList<>());
        }

        int[] indegree = new int[numCourses];
        for (int[] p : prerequisites) {
            adj.get(p[1]).add(p[0]);
            indegree[p[0]]++;
        }

        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (indegree[i] == 0) q.add(i);
        }

        int completed = 0;
        while (!q.isEmpty()) {
            int curr = q.poll();
            completed++;

            for (int n : adj.get(curr)) {
                indegree[n]--;
                if (indegree[n] == 0) {
                    q.add(n);
                }
            }
        }

        return completed == numCourses;
    }
}
```
**M2: DFS**
```java
class Solution {  
    public boolean canFinish(int numCourses, int[][] prerequisites) {  
        List<List<Integer>> adj = new ArrayList<>();  
        for (int i = 0; i < numCourses; i++) {  
            adj.add(new ArrayList<>());  
        }  
  
        for (int[] p : prerequisites) {  
            adj.get(p[1]).add(p[0]);  
        }  
  
        int[] state = new int[numCourses]; // 0 = unvisited, 1 = visiting, 2 = visited  
  
        for (int i = 0; i < numCourses; i++) {  
            if (state[i] == 0) {  
                if (dfs(i, adj, state)) {  
                    return false; // cycle found  
                }  
            }  
        }  
  
        return true;  
    }  
  
    private boolean dfs(int node, List<List<Integer>> adj, int[] state) {  
        state[node] = 1; // visiting  
  
        for (int next : adj.get(node)) {  
            if (state[next] == 1) {  
                return true; // cycle detected  
            }  
            if (state[next] == 0 && dfs(next, adj, state)) {  
                return true;  
            }  
        }  
  
        state[node] = 2; // visited  
        return false;  
    }  
}
```
---
### Q19] Course Schedule II
**M1: BFS**
```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < numCourses; i++) {
            adj.add(new ArrayList<>());
        }

        int[] indegree = new int[numCourses];
        for (int[] p : prerequisites) {
            adj.get(p[1]).add(p[0]);
            indegree[p[0]]++;
        }

        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (indegree[i] == 0) q.add(i);
        }

        int[] order = new int[numCourses];
        int idx = 0, completed = 0;
        while (!q.isEmpty()) {
            int curr = q.poll();
            order[idx++] = curr;
            completed++;

            for (int n : adj.get(curr)) {
                indegree[n]--;
                if (indegree[n] == 0) {
                    q.add(n);
                }
            }
        }

        return completed == numCourses ? order : new int[0];
    }
}
```
**M1: DFS**
```java
class Solution {  
    public int[] findOrder(int numCourses, int[][] prerequisites) {  
        List<List<Integer>> adj = new ArrayList<>();  
        for (int i = 0; i < numCourses; i++) {  
            adj.add(new ArrayList<>());  
        }  
        for (int[] p : prerequisites) {  
            adj.get(p[1]).add(p[0]);  
        }  
  
        int[] state = new int[numCourses]; // 0=unvisited, 1=visiting, 2=visited  
        List<Integer> order = new ArrayList<>();  
  
        for (int i = 0; i < numCourses; i++) {  
            if (state[i] == 0) {  
                if (dfs(i, adj, state, order)) {  
                    return new int[0];  
                }  
            }  
        }  
  
        // Reverse postorder  
        Collections.reverse(order);  
        return order.stream().mapToInt(i -> i).toArray();  
    }  
  
    private boolean dfs(int node, List<List<Integer>> adj, int[] state, List<Integer> order) {  
        state[node] = 1;  
  
        for (int next : adj.get(node)) {  
            if (state[next] == 1) return true;  
            if (state[next] == 0 && dfs(next, adj, state, order)) {  
                return true;  
            }  
        }  
  
        state[node] = 2;  
        order.add(node); // postorder add  
        return false;  
    }  
}
```
---
