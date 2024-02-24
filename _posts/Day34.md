# Day 34 - 62 Unique Paths | 63 Unique Paths II

## 62 Unique Paths
[Description on Leetcode](https://leetcode.com/problems/unique-paths/description/)

> ### Intuition
> We can define dp[i][j] means start from (0, 0) to (i, j), there are dp[i][j] paths.
> So to arrive at (i, j) and take 1 step back, there's only 2 ways: either coming from (i - 1, j) and take 1 more step to the right, or coming from (i, j - 1) and take 1 more step down. So dp[i][j] = dp[i - 1][j] + dp[i][j - 1].
> But we should be aware of the initial state: from (0, 0) to (i, 0), there are only 1 path, so dp[i][0] = 1. Similarly to that, (0, 0) to (0, j) there are only 1 path, so for all dp[0][j] = 1

### Approach
1. Dynamic Programming: based on dp[i][j] = dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
2. Graph - DFS: Conceptualise the paths to be a binary tree, the finish is the leaf nodes. To calcualte how many paths is equal to calculate the number of leaf nodes. But it will time out in Leetcode because time complexity is too large

```
/**
 * @param {number} m
 * @param {number} n
 * @return {number}
 */

/* 
* Graph - DFS, Depth First Search (Timed out)
* The path can be conceptualised as a binary tree, the finish is the leaf nodes of the tree
* So it converts to calculate the leaf nodes of the tree using DFS (Starting from 1)
* Time Complexity: O(2 ^ (m + n - 1) - 1)
* Space Complexity: O(m + n - 1)
*/
var uniquePaths = function(m, n) {
    const dfs = (i, j, m, n) => {
        if (i > m || j > n) return 0;   // out of the boundary
        if (i === m && j === n) return 1;   // find a valid path
        return dfs(i + 1, j, m, n) + dfs(i, j + 1, m, n)
    }
    return dfs(1, 1, m, n);
};

/* 
* Dynamic Programming
* Time Complexity: O(m * n)
* Space Complexity: O(m * n)
*/
var uniquePaths = function(m, n) {
    const dp = new Array(m).fill().map(() => new Array(n).fill(0));

    for (let i = 0; i < m; i++) {
        dp[i][0] = 1;  
    }
    for (let j = 0; j < n; j++) {
        dp[0][j] = 1;
    }

    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
        }
    }
    return dp[m - 1][n - 1];
};
```


## 63 Unique Paths II
[Description on Leetcode](https://leetcode.com/problems/unique-paths-ii/description/)

> ### Intuition
> It's very similar to last exercise, but here we have obstacles. Actually, we can conceptualise the obstacles as:
> if (i, j) has obstacles (obstableGrid[i][j] === 1), then dp[i][j] = 0; because it's not a valid path, we don't need to calcualte it.
> But be aware of the initial state. From (0, 0) to (i, 0), if say (k, 0) has obstacle (0 < k < i), then dp[i][0] after k are 0 not 1, because we can't bypass the obstacle so bascially no path to get into the unit after k.

### Approach
1.  Dynamic Programming: Same as last exercise but need to consider obstableGrid[i][j] === 1. So if (obstableGrid[i][j] === 0) { // No obstacle on (i, j), dp[i][j] = dp[i][j] = dp[i - 1][j] + dp[i][j - 1]}

```
/**
 * @param {number[][]} obstacleGrid
 * @return {number}
 */

/* 
* Dynamic Programming
* Time Complexity: O(m * n)
* Space Complexity: O(m * n)
*/
var uniquePathsWithObstacles = function(obstacleGrid) {
    const m = obstacleGrid.length;
    const n = obstacleGrid[0].length;
    const dp = new Array(m).fill().map(() => new Array(n).fill(0));

    for (let i = 0; i < m && obstacleGrid[i][0] === 0; i++) {
        dp[i][0] = 1;
    }
    for (let j = 0; j < n && obstacleGrid[0][j] === 0; j++) {
        dp[0][j] = 1;
    }

    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            if (obstacleGrid[i][j] === 0) {
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
    }
    return dp[m - 1][n - 1];
};
```
