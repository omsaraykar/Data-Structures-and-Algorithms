## Steps to solve a Recursion Question
1. Try to represent the problem in terms of index.
2. Do all possible stuffs on that index according to the problem statement.
3. Sum of all stuff --> count all ways
   Min of all stuff -> find min
>[!NOTE] When there is something like `idx - 1` and `idx - 2` we can always do space optimization.
## Questions
### Q1] Fibonacci Number

### Q2] Climbing Stairs
### Q3] Frog Jump
**Recursion: O($2^n$) Time and O(n) Space**
```java
class Solution {
		int minCost(int[] height) {
        int n = height.length;
        return minCostRec(n - 1, height);
    }
		int minCostRec(int n, int[] height) {
        if (n == 0) return 0;
        if (n == 1) return abs(height[n] - height[n - 1]);
        
        return min(
		        minCostRec(n - 1, height) + abs(height[n] - height[n - 1]),
            minCostRec(n - 2, height) + abs(height[n] - height[n - 2])          
        );
    }
}
```
**Memoization: O(n) Time and O(n) Space**
```java
class Solution {
    int minCost(int[] height) {
        int n = height.length;

        int[] dp = new int[n + 1];
        Arrays.fill(dp, -1);

        return minCostRec(n - 1, height, dp);
    }
    int minCostRec(int n, int[] height, int[] dp) {
        if (n == 0) return 0;
        if (n == 1) return Math.abs(height[n] - height[n - 1]);
        
        if (dp[n] != -1) return dp[n];

        dp[n] = Math.min(
            minCostRec(n - 1, height, dp) + Math.abs(height[n] - height[n - 1]),
            minCostRec(n - 2, height, dp) + Math.abs(height[n] - height[n - 2])
        );

        return dp[n];
    }
}
```
**Tabulation: O(n) Time and O(n) Space**
```java
class Solution {
    int minCost(int[] height) {
        int n = height.length;
        if (n == 1) return 0;

        int[] dp = new int[n];
        dp[0] = 0;
        dp[1] = Math.abs(height[1] - height[0]);

        for (int i = 2; i < n; i++) {
            dp[i] = Math.min(
                    dp[i - 1] + Math.abs(height[i] - height[i - 1]),
                    dp[i - 2] + Math.abs(height[i] - height[i - 2])
            );
        }

        return dp[n - 1];
    }
}
```
**Space Optimization: O(n) Time and O(1) Space**
```java
class Solution {
    int minCost(int[] height) {
        int n = height.length;
        if (n == 1) return 0;

        int dp0 = 0;
        int dp1 = Math.abs(height[1] - height[0]);

        for (int i = 2; i < n; i++) {
            int curr = Math.min(
                    dp1 + Math.abs(height[i] - height[i - 1]),
                    dp0 + Math.abs(height[i] - height[i - 2])
            );
            dp0 = dp1;
            dp1 = curr;
        }

        return dp1;
    }
}
```
---
### Q4] Frog Jump with `k` distances
### Q5] House Robber
**Recursion: O(2^n) Time and O(n) Space**
```java
class Solution {
    public int rob(int[] nums) {
		    int n = nums.length;
        return robRec(n - 1, nums);
    }
    private int robRec(int i, int[] nums) {
		    if (i == 0) return nums[0];
		    if (i == 1) return nums[1];
		    
		    int take = nums[i - 2] + nums[i];
		    int notTake = nums[i - 1];
		    
		    return Math.min(take, notTake);
    }
}
```
**Memoization: O(n) Time and O(n) Space**
```java
class Solution {
    public int rob(int[] nums) {
		int n = nums.length;
        int[] dp = new int[n];
        Arrays.fill(dp, -1);
        
        return robRec(n - 1, nums, dp);
    }
    private int robRec(int i, int[] nums, int[] dp) {
        if (i == 0) return nums[0];
        if (i == 1) return Math.max(nums[0], nums[1]);

        if (dp[i] != -1) return dp[i];
        
        int take = robRec(i - 2, nums, dp) + nums[i];
        int notTake = robRec(i - 1, nums, dp);
        
        return dp[i] = Math.max(take, notTake);
    }
}
```
**Tabulation: O(n) Time and O(n) Space**
```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;

        if (n == 1) {
            return nums[0];
        }

        int[] dp = new int[n];

        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);

        for (int i = 2; i < n; i++) {
            dp[i] = Math.max(dp[i - 1], nums[i] + dp[i - 2]);
        }

        return dp[n - 1]; 
    }
}
```
**Space Optimization: O(n) Time and O(1) Space**
```java
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 1) return nums[0];

		int dp0 = nums[0];
        int dp1 = Math.max(nums[0], nums[1]);

        for (int i = 2; i < nums.length; i++) {
            int curr = Math.max(dp0 + nums[i], dp1);
            dp0 = dp1;
            dp1 = curr;
        }

        return dp1;
    }
}
```
---
### Q6] Geek's Training
**Memoization**
```java
class Solution {
    public int maximumPoints(int arr[][]) {
        int n = arr.length;
        
        int[][] dp = new int [n][3];
        for (int i = 0; i < n; i++) {
            Arrays.fill(dp[i], -1);
        }
        
        return maximumPointsRec(arr, n - 1, -1, dp);
    }
    private int maximumPointsRec(int arr[][], int idx, int last, int[][] dp) {
        if (idx == 0) {
            int maxPoints = 0;
            for (int i = 0; i < 3; i++) {
                if (i != last) {
                    int points = arr[idx][i];
                    maxPoints = Math.max(maxPoints, points);
                }
            }
            return maxPoints;
        }
        
        int maxPoints = 0;
        for (int i = 0; i < 3; i++) {
            if (i != last) {
                if (dp[idx - 1][i] == -1) {
                    dp[idx - 1][i] = maximumPointsRec(arr, idx - 1, i, dp);
                }
                int points = arr[idx][i] + dp[idx - 1] [i];
                maxPoints = Math.max(maxPoints, points);
            }
        }
        
        return maxPoints;
    }
}
```
**Tabulation**
```java
class Solution {
    public int maximumPoints(int[][] arr) {
        int n = arr.length;

        int[][] dp = new int[n][3];

        // Base case: day 0
        for (int j = 0; j < 3; j++) {
            dp[0][j] = arr[0][j];
        }

        // Fill dp table
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < 3; j++) {
                int bestPrev = 0;
                for (int k = 0; k < 3; k++) {
                    if (k != j) {
                        bestPrev = Math.max(bestPrev, dp[i - 1][k]);
                    }
                }
                dp[i][j] = arr[i][j] + bestPrev;
            }
        }

        // Final answer = max on last day
        return Math.max(dp[n - 1][0],
               Math.max(dp[n - 1][1], dp[n - 1][2]));
    }
}
```
**Spaced Optimized**
```java
class Solution {
    public int maximumPoints(int[][] arr) {
        int n = arr.length;

        int[] prev = new int[3];
        int[] curr = new int[3];

        // Base case for day 0
        for (int j = 0; j < 3; j++) {
            prev[j] = arr[0][j];
        }

        // Compute forward
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < 3; j++) {

                int bestPrev = 0;
                for (int k = 0; k < 3; k++) {
                    if (k != j) {
                        bestPrev = Math.max(bestPrev, prev[k]);
                    }
                }

                curr[j] = arr[i][j] + bestPrev;
            }

            // Move curr → prev
            prev[0] = curr[0];
            prev[1] = curr[1];
            prev[2] = curr[2];
        }

        // Final maximum
        return Math.max(prev[0], Math.max(prev[1], prev[2]));
    }
}
```
---
### Q7] Unique Paths
### Q8] Unique Paths 2
**Memoization:**
```jsx
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;

        int[][] dp = new int[m][n];
        for(int i = 0; i < m; i++) {
            Arrays.fill(dp[i], -1);
        }

        return uniquePathsRec(0, 0, obstacleGrid, dp);
    }
    private int uniquePathsRec(int i, int j, int[][] grid, int[][] dp) {
        int m = grid.length;
        int n = grid[0].length;

        if (i == m - 1 && j == n - 1 && grid[i][j] == 0) return 1;
        if (i >= m || j >= n) return 0;
        if (grid[i][j] == 1) return 0;

        if (dp[i][j] != -1) return dp[i][j];

        int rightPath = uniquePathsRec(i, j + 1, grid, dp);
        int downPath = uniquePathsRec(i + 1, j, grid, dp);

        return dp[i][j] = rightPath + downPath;
    }
}
```
**Tabulation:**
```jsx
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        if (obstacleGrid[0][0] == 1) return 0;
        
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;

        int[][] dp = new int[m][n];
        for(int i = 0; i < m; i++) {
            Arrays.fill(dp[i], -1);
        }

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i == 0 && j == 0) dp[i][j] = 1;
                else if (obstacleGrid[i][j] == 1) dp[i][j] = 0;
                else {
                    int up = 0, left = 0;
                    if (i > 0) up = dp[i - 1][j];
                    if (j > 0) left = dp[i][j - 1];

                    dp[i][j] = up + left;
                }
            }
        }

        return dp[m - 1][n - 1];
    }
}
```
**Space Optimization:**
```jsx
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        if (obstacleGrid[0][0] == 1) return 0;

        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;

        int[] dp = new int[n];

        dp[0] = 1;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (obstacleGrid[i][j] == 1) dp[j] = 0;
                else {
                    if (j > 0) dp[j] += dp[j - 1];
                }
            }
        }

        return dp[n - 1];
    }
}
```
### Q9] Minimum Path Sum
**Recursion:**
```java
class Soulution {
    public int minPathSum(int[][] grid) {
	      return minPathSumRec(grid, 0, 0);
	  }
	  private int minPathRec(int[][] grid, int r, int c) {
	      int m = grid.length - 1;
	      int n = grid[0].length - 1;
	      
	      if (r == m && c == n) return grid[r][c];
	      
	      int right = Integer.MAX_VALUE;
	      int down = Integer.MAX_VALUE;
	      if (c < n) right = minPathSumRec(grid, r, c + 1);
	      if (r < m) down = minPathSumRec(grid, r + 1, c);
	      
	      return Math.min(right, down) + grid[r][c];
	  }
}
```
**Memoization:**
```java
class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;

        int[][] dp = new int[m][n];
        for (int i = 0; i < m; i++) {
            Arrays.fill(dp[i], -1);
        }

        return minPathSumRec(grid, 0, 0, dp);
    }
    private int minPathSumRec(int[][] grid, int r, int c, int[][] dp) {
        int m = grid.length - 1;
        int n = grid[0].length - 1;

        if (r == m && c == n) return grid[r][c];
        if (dp[r][c] != -1) return dp[r][c];

        int right = Integer.MAX_VALUE;
        int down = Integer.MAX_VALUE;
        if (c < n) right = minPathSumRec(grid, r, c + 1, dp);
        if (r < m) down = minPathSumRec(grid, r + 1, c, dp);

        return dp[r][c] = Math.min(right, down) + grid[r][c];
    }
}
```
**Tabulation:**
```java
class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length - 1;
        int n = grid[0].length - 1;

        for (int i = m; i >= 0; i--) {
            for (int j = n; j >= 0; j--) {
                if (i == m && j == n) continue;
                int right = Integer.MAX_VALUE;
                int down = Integer.MAX_VALUE;
                if (j < n) right = grid[i][j + 1];
                if (i < m) down = grid[i + 1][j];
                grid[i][j] += Math.min(right, down);
            }
        }

        return grid[0][0];
    }
}
```
**Space Optimization:**
```java
class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;

        int[] dp = new int[n];

        dp[0] = grid[0][0];

        // first row
        for (int j = 1; j < n; j++) {
            dp[j] = dp[j - 1] + grid[0][j];
        }

        // rest rows
        for (int i = 1; i < m; i++) {
            dp[0] += grid[i][0]; // first column

            for (int j = 1; j < n; j++) {
                dp[j] = Math.min(dp[j], dp[j - 1]) + grid[i][j];
            }
        }

        return dp[n - 1];
    }
}

```
### Q10] Triangle
**M1: Memoization**
```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();
        Integer [][] dp = new Integer[n][n];
        return dfs(0, 0, triangle, dp);
    }

    private int dfs(int i, int j, List<List<Integer>> triangle, Integer[][] dp) {
        if (i == triangle.size() - 1) {
            return triangle.get(i).get(j);
        }

        if (dp[i][j] != null) return dp[i][j];

        int down = dfs(i + 1, j, triangle, dp);
        int downRight = dfs(i + 1, j + 1, triangle, dp);

        return dp[i][j] = triangle.get(i).get(j) + Math.min(down, downRight);
    }
}
```
**M2: Tabulation**
```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();
        int[][] dp = new int[n][n];

        for (int j = 0; j < triangle.get(n - 1).size(); j++) {
            dp[n - 1][j] = triangle.get(n - 1).get(j);
        }

        for (int i = n - 2; i >= 0; i--) {
            for (int j = 0; j <= i; j++) {
                dp[i][j] = triangle.get(i).get(j) + Math.min(dp[i + 1][j], dp[i + 1][j + 1]);
            }
        }

        return dp[0][0];
    }
}
```
**M3: Space Optimization**
```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();
        int[] dp = new int[n];
        
        for (int j = 0; j < n; j++) {
            dp[j] = triangle.get(n - 1).get(j);
        }
        
        for (int i = n - 2; i >= 0; i--) {
            for (int j = 0; j <= i; j++) {
                dp[j] = triangle.get(i).get(j) + Math.min(dp[j], dp[j + 1]);
            }
        }

        return dp[0];
    }
}
```
---
