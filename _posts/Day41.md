# Day 41 - 198 House Robber | 213 House Robber II | 337 House Robber III


## 198 House Robber
[Description on Leetcode](https://leetcode.com/problems/house-robber/description/)

> ### Intuition
> This is a typical exercise using dynamic programming.
> dp[i] means the max value of robbing [0..i] houses. And dp[i] depends on dp[i - 1] and dp[i - 2]. So if not robbing ith house, the value so far is dp[i - 1] because it means we need to consider robbing i-1th house. If we are robbing the ith house, we need to consider the dp[i - 2] which is dp[i - 2] + nums[i].
> Just need to be aware of the intial value. dp[0] = nums[0] and dp[1] = Math.max(nums[0], nums[1]).

### Approach
1. Dynamic Programming - 1 dimension dp array, using dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i]).

```
/**
 * @param {number[]} nums
 * @return {number}
 */

/* Dynamic Programming
* Time Complexity: O(N)
* Space Complexity: O(N)
*/
var rob = function(nums) {
    let dp = new Array(nums.length).fill(0);
    dp[0] = nums[0];
    dp[1] = Math.max(nums[0], nums[1]);

    for (let i = 2; i < nums.length; i++) {
        dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i]);
    }
    return dp[nums.length - 1];
};
```


## 213 House Robber II
[Description on Leetcode](https://leetcode.com/problems/house-robber-ii/description/)

> ### Intuition
> This is very similar to last exerice. The only difference is that here the nums array is in circle.
> For arrays in circle, we need to consider 3 cases:
> - Case 1: we don't need to consider the first element and the last element
> - Case 2: we only consider the first element, but we don't need to include the last element (because if we consider to rob the first element, we can't rob the last element as in circle)
> - Case 3: we only consider the last element, but we don't need to include the first element (because if we consider to rob the last element, we can't rob the first element as in circle)
> Actually, both case 2 and 3 is alreay containing case 1. So we just need to consider case 2 & 3.

### Approach
1. Dynamic Programming - 1 dimension dp array, using dp[j] = Math.min(dp[j], dp[j - coins[i]] + 1).

```
/**
 * @param {number[]} nums
 * @return {number}
 */

/* Dynamic Programming - 1 dimension dp array
* Time Complexity: O(N)
* Space Complexity: O(N)
*/
/* For an array in circle, we need to consider 2 cases:
* - 1. Consider to include the first element, so the last element should not be included because it can't be robbed
* - 2. Consider to include the last element, then the first element should not be included because it can't be robbed 
*/ 
var rob = function(nums) {
    let len = nums.length;
    if (len === 0) return 0;
    if (len === 1) return nums[0];

    const result1 = robRange(nums, 0, len - 2); // Case 2
    const result2 = robRange(nums, 1, len - 1); // Case 3
    return Math.max(result1, result2);
};

const robRange = (nums, start, end) => {
    if (start === end) return nums[start];

    let dp = new Array(nums.length).fill(0);
    dp[start] = nums[start];
    dp[start + 1] = Math.max(nums[start], nums[start + 1]);

    for (let i = start + 2; i <= end; i++) {
        dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i]);
    }
    return dp[end];
}
```

## 337 House Robber III
[Description on Leetcode](https://leetcode.com/problems/house-robber-iii/description/)

> ### Intuition
> So this is evolving tree traversal. Here we should use post-order traversal because all the return calculation is based on its children.
> Very similar to last 2 exercise, the max value of current node depends on 2 cases:
> - If we rob the current node, we can't rob its children, instead, we can only rob its grandchildren
> - If we don't rob the current node, we can consider to rob its children

### Approach
1. Recursion - traverse all nodes (timed out) / optimised traversal using a map to record all previous calculation
2. Dynamic Programming - 1 dimension dp array, using dp[j] = Math.min(dp[j], dp[j - coins[i]] + 1).

```
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */

/* 
* Recursion - traverse children but it will be timed out
* Time Complexity: O(N ^ 2)
* Space Complexity: O(Log N)
*/
var rob = function(root) {
    if (root === null) return 0;
    if (root.left === null && root.right === null) return root.val;

    // Rob this node, then cannot rob its child, instead, rob its grand children
    let val1 = root.val;
    if (root.left) val1 += rob(root.left.left) + rob(root.left.right);
    if (root.right) val1 += rob(root.right.left) + rob(root.right.right);
    // Not rob this node
    let val2 = rob(root.left) + rob(root.right);

    return Math.max(val1, val2);
};

/* 
* Optimised Recursion - postorder traverse children and use map to record previous results to avoid repeat calculation
* Time Complexity: O(N)
* Space Complexity: O(Log N)
*/
let map = new Map();
var rob = function(root) {
    if (root === null) return 0;
    if (root.left === null && root.right === null) return root.val;
    if (map.has(root)) return map.get(root);

    // Rob this node, then cannot rob its child, instead, rob its grand children
    let val1 = root.val;
    if (root.left) val1 += rob(root.left.left) + rob(root.left.right);
    if (root.right) val1 += rob(root.right.left) + rob(root.right.right);
    // Not rob this node
    let val2 = rob(root.left) + rob(root.right);

    map.set(root, Math.max(val1, val2));
    return Math.max(val1, val2);
};

/* 
* Dynamic Programming + Recursion - postorder traverse children and use dp to record the money of robbing current node or not robbing
* dp is an array with 2 elements, 0 means max value if not robbing current node, 1 means max value if robbing current node.
* Time Complexity: O(N)
* Space Complexity: O(Log N)
*/
const robTree = (cur) => {
    if (cur === null) return [0, 0];
    const left = robTree(cur.left);
    const right = robTree(cur.right);

    // Not rob this node
    const val1 = Math.max(...left) + Math.max(...right);
    // Rob this node, then cannot rob its child
    const val2 = cur.val + left[0] + right[0];
    return [val1, val2];
}
var rob = function(root) {
    const result = robTree(root);
    return Math.max(...result);
};
```
