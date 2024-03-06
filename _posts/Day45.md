# Day 45 - 300 Longest Increasing Subsequence | 674 Longest Continuous Increasing Subsequence | 718 Maximum Length of Repeated Subarray

## 300 Longest Increasing Subsequence
[Description on Leetcode](https://leetcode.com/problems/longest-increasing-subsequence/description/)

> ### Intuition
> Please notice that the **Longest Increasing Subsequence** isn't necessarily to be continuous. It can consist of non-continuous arrays from nums. So if we found a nums[j] before nums[i] and nums[i] > nums[j] that means the length of Longest Increasing Subsequence should increase 1, dp[i] = Math.max(dp[i], dp[j] + 1).
> Also, we need to notice that the initial value of dp is 1. Because the minimum subsequence length is 1 (single element).

### Approach
1. Dynamic Programming - dp[i] means the length of Longest Increasing Subsequence of nums array ends with i index

```
/**
 * @param {number[]} nums
 * @return {number}
 */

/* 
* Dynamic Programming - dp[i] means the length of Longest Increasing Subsequence of nums array ends with i index
* Time complexity; O(N ^ 2)
* Spcace complexity: O(N)
*/
var lengthOfLIS = function(nums) {
    let dp = new Array(nums.length).fill(1);
    for (let i = 1; i < nums.length; i++) {
        for (let j = 0; j < i; j++) {
            if (nums[i] > nums[j]) dp[i] = Math.max(dp[i], dp[j] + 1);
        }
    }
    return Math.max(...dp);
};
```


## 674 Longest Continuous Increasing Subsequence
[Description on Leetcode](https://leetcode.com/problems/longest-continuous-increasing-subsequence/description/)

> ### Intuition
> The only difference from last exercise is **Continuous**. So instead of checking nums[i] > nums[j], we need to check nums[i] > nums[i - 1].

### Approach
1. Dynamic Programming - dp[i] means the length of Longest Continuous Increasing Subsequence of nums array ends with i index.

```
/**
 * @param {number[]} nums
 * @return {number}
 */

/* 
* Dynamic Programming - dp[i] means the length of Longest Continuous Increasing Subsequence of nums array ends with i index
* Time complexity; O(N)
* Spcace complexity: O(N)
*/
var findLengthOfLCIS = function(nums) {
    let dp = new Array(nums.length).fill(1);

    for (let i = 1; i < nums.length; i++) {
        if (nums[i] > nums[i - 1]) {
            dp[i] = dp[i - 1] + 1;
        } else {
            dp[i] = 1;   // Reset the value if there's no continuous subsequence.
        }
    }
    return Math.max(...dp);
};


/* 
* Greedy - if we find nums[i] > nums[i - 1], count++;
* Time complexity; O(N)
* Spcace complexity: O(1)
*/
var findLengthOfLCIS = function(nums) {
    if (nums.length === 0) return 0;
    let count = 1;
    let result = 1;
    for (let i = 1; i < nums.length; i++) {
        if (nums[i] > nums[i - 1]) {
            count++;
            result = Math.max(result, count);
        } else {
            count = 1;
        }
    }
    return result;
};
```


## 718 Maximum Length of Repeated Subarray
[Description on Leetcode](https://leetcode.com/problems/maximum-length-of-repeated-subarray/description/)

> ### Intuition
> We can define dp[i][j] to be the Maximum Length of Repeated Subarray between array ends with A[i - 1] and array ends with B[j - 1]. So if (A[i] === B[j]) dp[i][j] = dp[i - 1][j - 1] + 1. Note that A ends with i - 1, not i. And B ends with j - 1, not j.
> Please note that dp[i][0] and dp[0][j] doesn't have any meaning here, because we can't define the length of subarray for elements A[-1] or B[-1]. So the value of dp[i][0] and dp[0][j] is more like an initial value 0 without any meaning, but can make it meaningful for dp[1][1] = dp[0][0] + 1.

### Approach
1. Dynamic Programming - dp[i][j] to be the Maximum Length of Repeated Subarray between array ends with A[i - 1] and array ends with B[j - 1]

```
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number}
 */

/* 
* Dynamic Programming - dp[i][j] to be the Maximum Length of Repeated Subarray between array ends with A[i - 1] and array ends with B[j - 1]
* Time complexity; O(M * N) where M is A's length and N is B's length
* Spcace complexity: O(M * N)
*/
var findLength = function(nums1, nums2) {
    const [m ,n] = [nums1.length, nums2.length];

    let dp = new Array(m + 1).fill().map(() => new Array(n + 1).fill(0));
    let result = 0;

    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (nums1[i - 1] === nums2[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            }
            if (dp[i][j] > result) result = dp[i][j];
        }
    }
    return result;
};


/* 
* Dynamic Programming - Optimised space
* Time complexity; O(M * N) where M is A's length and N is B's length
* Spcace complexity: O(N)
*/
var findLength = function(nums1, nums2) {
    const [m ,n] = [nums1.length, nums2.length];

    let dp = new Array(n + 1).fill(0);
    let result = 0;

    for (let i = 1; i <= m; i++) {
        for (let j = n; j > 0; j--) {   // Need to iterate nums2 from end to start, to ensure the previous dp value won't be overwritten
            if (nums1[i - 1] === nums2[j - 1]) {
                dp[j] = dp[j - 1] + 1;
            } else {
                dp[j] = 0;    // Also need to update the value when there's no matching
            }
            if (dp[j] > result) result = dp[j];
        }
    }
    return result;
};
```
