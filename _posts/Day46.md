# Day 46 - 1143 Longest Common Subsequence | 1035 Uncrossed Lines | 53 Maximum Subarray

## 1143 Longest Common Subsequence
[Description on Leetcode](https://leetcode.com/problems/longest-common-subsequence/description/)

> ### Intuition
> We define d[i][j] to be the longest common subsequence of text1[0...i - 1] and text2[0...j - 1]. If text1[i - 1] === text2[j - 1], dp[i][j] = dp[i - 1][j - 1] + 1 because we have 1 more character to add into the longest common subsequence; if text1[i - 1] !== text2[j - 1], dp[i][j] = Math.max(dp[i - 1][j], dp[i][j -  1]) because we need to reuse the longest common subsequence from previous text strings. 

### Approach
1. Dynamic Programming - d[i][j] means the longest common subsequence of text1[0...i - 1] and text2[0...j - 1]

```
/**
 * @param {string} text1
 * @param {string} text2
 * @return {number}
 */

/* 
* Dynamic Programming - d[i][j] means the longest common subsequence of text1[0...i - 1] and text2[0...j - 1]
* Time complexity; O(M * N)
* Spcace complexity: O(M * N)
*/
var longestCommonSubsequence = function(text1, text2) {
    const [m, n] = [text1.length, text2.length];

    let dp = new Array(m + 1).fill().map(() => new Array(n + 1).fill(0));
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (text1[i - 1] === text2[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }
    return dp[m][n];
};
```


## 1035 Uncrossed Lines
[Description on Leetcode](https://leetcode.com/problems/uncrossed-lines/description/)

> ### Intuition
> The question requires to find A[i] == B[j] and the lines can't be crossed. So if we find common subsequences and maintain elements' position within the arrays, the lines won't be cross anyway. So this question is actually asking to find the longest common subsequence - same as last exercise.

### Approach
1. Dynamic Programming - d[i][j] means the longest common subsequence of nums1[0...i - 1] and nums2[0...j - 1]

```
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number}
 */

/* 
* Dynamic Programming - d[i][j] means the longest common subsequence of nums1[0...i - 1] and nums2[0...j - 1]
* Time complexity; O(M * N)
* Spcace complexity: O(M * N)
*/
var maxUncrossedLines = function(nums1, nums2) {
    const [m, n] = [nums1.length, nums2.length];
    let dp = new Array(m + 1).fill().map(() => new Array(n + 1).fill(0));

    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (nums1[i - 1] === nums2[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }
    return dp[m][n];
};
```


## 53 Maximum Subarray
[Description on Leetcode](https://leetcode.com/problems/maximum-subarray/description/)

> ### Intuition
> For dynamic programming, we can define dp[i] to be the maximum sum of subarray ends with element nums[i]. So dp[i] is derived from either dp[i - 1] + nums[i] (sums with previous elements to be included into the subarray) or nums[i] (itself become the start of the subarray), which is dp[i] = Math.max(dp[i - 1] + nums[i], nums[i]).
> Remember to initialise dp[0] = nums[0]. And return Math.max(...dp) because dp only represents the maximum sum of subarrays ends with nums[i]. The overall maximum sum can exists for subarrays not ends with nums[nums.length - 1].

### Approach
1. Greedy - loop into the array and count the current sum (curSum). If curSum < 0, means this must not be the maximum sum of subarray, we need to reset curSum = 0 then continue on counting the current sum. Return the largest current sum so far.
2. Dynamic Programming - dp[i][j] to be the Maximum Length of Repeated Subarray between array ends with A[i - 1] and array ends with B[j - 1]

```
/**
 * @param {number[]} nums
 * @return {number}
 */

/* Greedy - finding the count > 0
* Time complexity: O(N)
* Space complexity: O(1) 
*/
var maxSubArray = function(nums) {
    let result = -Infinity;
    let count = 0;
    for (let i = 0; i < nums.length; i++) {
        count += nums[i];
        if (count > result) {
            result = count;
        }
        if (count < 0) count = 0;
    }
    return result;
};

/* 
* Dynamic Programming - dp[i] to be the maximum sum of subarray ends with element nums[i]; dp[i] = Math.max(dp[i - 1] + nums[i], nums[i]).
* Time complexity; O(N)
* Spcace complexity: O(N)
*/
var maxSubArray = function(nums) {
    let dp = new Array(nums.length);
    dp[0] = nums[0];
    let result = nums[0];

    for (let i = 1; i < nums.length; i++) {
        dp[i] = Math.max(dp[i - 1] + nums[i], nums[i]);
        result = Math.max(dp[i], result);
    }
    return result;
    //return Math.max(...dp);
};
```
