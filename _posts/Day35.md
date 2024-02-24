# Day 35 - 343 Integer Break | 96 Unique Binary Search Trees

## 343 Integer Break
[Description on Leetcode](https://leetcode.com/problems/integer-break/description/)

> ### Intuition
> How to come up with the state transition formula?
> let 1 <= j <= i, for each number that <= i, we make j as the first integer break of i, then dp[i] can derive from i * (i - j) if we only divide it into 2 integer, or j * dp[i - j].
> So dp[i] = Math.max(dp[i], j * dp[i - j], i * (i - j)).
> But do we need to check Math.max of dp[i] here? Because we loop into j where 1 <= j <= i, dp[i] represents j - 1's maximum integer break multiplies, as we want to know i's maxmimum integer break multiplies regardless of the dividend of integer, we need to keep comparing the dp[j - 1] till we finish the loop.
> Also, for initial state, we should understand that dp[0] and dp[1] are meaningfuless, so we don't need to assign a value to them.

### Approach
1. Dynamic Programming: For each i, loop into 1 <= j <= i, as dp[i] = Math.max(dp[i], j * dp[i - j], i * (i - j)).

```
/**
 * @param {number} n
 * @return {number}
 */

/* 
* Dynamic Programming
* Time Complexity: O(N ^ 2)
* Space Complexity: O(N)
*/
var integerBreak = function(n) {
    let dp = new Array(n + 1).fill(0);
    dp[2] = 1;

    for (let i = 3; i <= n; i++) {
        for (let j = 1; j <= i / 2; j++) {  
            // Why stops at i / 2? 
            // Because the integer needs to be similar to make the multiply to be maximum. 
            // We know that if j > i / 2, we've traversed the possible value before, so no need to continue on the calculation because we know the multiplies won't be larger than before.
            dp[i] = Math.max(dp[i], j * (i - j), j * dp[i - j]); 
        }
    }
    return dp[n];
};

/* 
* Greedy - we can divide n into 3, and if the last remain number is 4, then multiplies 4 to get the maximum multiplies
* Time Complexity: O(N)
* Space Complexity: O(1)
*/
var integerBreak = function(n) {
    if (n === 2) return 1;
    if (n === 3) return 2;
    if (n === 4) return 4;

    let result = 1;
    while (n > 4) {
        result *= 3;
        n -= 3;
    }
    return result * n;
};
```


## 96 Unique Binary Search Trees
[Description on Leetcode]([https://leetcode.com/problems/unique-paths-ii/description/](https://leetcode.com/problems/unique-binary-search-trees/description/))

> ### Intuition
> This is tricky. But if we take n = 3 as an example and draw the pictures to analyse, we can find that dp[3] = the number of trees that uses 1 as root + the number of trees that uses 2 as root + the number of trees that uses 3 as root.
> Then the number of trees that uses 1 as root = the number of trees whose right sub-tree has 2 elements + the number of trees whose left sub-tree has 0 elements;
> the number of trees that uses 2 as root = the number of trees whose right sub-tree has 1 element + the number of trees whose left sub-tree has 1 element;
> the number of trees that uses 3 as root = the number of trees whose right sub-tree has 0 element + the number of trees whose left sub-tree has 2 elements;
> and dp[2] means the number of trees that has 2 elements; dp[1] means the number of trees that has 1 element.
> Therefore, dp[3] = dp[2] * dp[0] + dp[1] * dp[1] + dp[0] * dp[2].
> Then we can find the state transition, dp[i] += dp[j - 1] * dp[i - j] where 1 <= j <= i.
> dp[0] = 1, because null as tree is also a binary search tree

### Approach
1.  Dynamic Programming: Based on dp[i] += dp[j - 1] * dp[i - j] where 1 <= j <= i.

```
/**
 * @param {number} n
 * @return {number}
 */

/* 
* Dynamic Programming
* Time Complexity: O(N ^ 2)
* Space Complexity: O(N)
*/
var numTrees = function(n) {
    let dp = new Array(n + 1).fill(0);
    dp[0] = 1;
    dp[1] = 1;

    for (let i = 2; i <= n; i++) {
        for (let j = 1; j <= i; j++) {
            dp[i] += dp[j - 1] * dp[i - j];
        }
    }
    return dp[n];
};
```
