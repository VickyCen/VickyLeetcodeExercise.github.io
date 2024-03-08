# Day 47 - 392 Is Subsequence | 115 Distinct Subsequences

## 392 Is Subsequence
[Description on Leetcode](https://leetcode.com/problems/is-subsequence/description/)

> ### Intuition
> We define d[i][j] to be the longest common subsequence of text1[0...i - 1] and text2[0...j - 1]. If text1[i - 1] === text2[j - 1], dp[i][j] = dp[i - 1][j - 1] + 1 because we have 1 more character to add into the longest common subsequence; if text1[i - 1] !== text2[j - 1], dp[i][j] = dp[i][j - 1] because we want to "remove" the t[j - 1] element to match s.
> Be aware that we want to know whether s is t's subsequence, so s.length <= t.length.

### Approach
1. Dynamic Programming - d[i][j] means the longest common subsequence of s[0...i - 1] and t[0...j - 1];
2. Two pointers: one pointer (slow) loop into s, the other pointer (fast) loop into t. Keep moving fast, and only move slow when s[slow] === t[fast].

```
/**
 * @param {string} s
 * @param {string} t
 * @return {boolean}
 */

/* 
* Dynamic Programming - d[i][j] means the longest common subsequence of text1[0...i - 1] and text2[0...j - 1]
* Time complexity; O(M * N)
* Spcace complexity: O(M * N)
*/
var isSubsequence = function(s, t) {
    const [m, n] = [s.length, t.length];
    let dp = new Array(m + 1).fill().map(() => new Array(n + 1).fill(0));

    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (s[i - 1] === t[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            } else {
                // dp[i][j] = Math.max(dp[i][j - 1], dp[i - 1][j]);
                dp[i][j] = dp[i][j - 1];       // Because we assume s.length <= t.length, so whenever s[i - 1] !== t[j - 1], we want to "remove" the t[j - 1].
            }
        }
    }
    return dp[m][n] === m;
};

/* 
* Two pointer - 1 pointer to check s, the other pointer to check t
* Time complexity; O(N) where N is t's length
* Spcace complexity: O(1)
*/
var isSubsequence = function(s, t) {
    let slow = fast = 0;
    while (fast < t.length) {
        if (s[slow] === t[fast]) {
            slow++;
            if (slow > s.length - 1) return true;
        } 
        fast++;
    }
    return slow > s.length - 1;
};
```


## 115 Distinct Subsequences
[Description on Leetcode](https://leetcode.com/problems/distinct-subsequences/description/)

> ### Intuition
> We define dp[i][j] to be the number of subsequences that ends with t[j - 1] character that exists in s that ends with s[i - 1] character. So if (s[i - 1] === t[j - 1]), the subsequences are coming from 2 ways:
> - Matching using s[i - 1], then dp[i][j] = dp[i - 1][j - 1];
> - Not matching using s[i - 1], then dp[i][j] = dp[i - 1][j]; For example, s：bagg and t：bag, s[2] and s[3] has same character, we can use s[2] to match t instead of using s[3], then now the dp[i][j] = dp[i - 1][j]
> Therefore, dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
> When s[i - 1] !== t[j - 1], we cannot match using s[i - 1], so dp[i][j] = dp[i - 1][j] because we need to utilise characters before s[i - 1] to get the number of matching subsequences.
> Also, for dp[0][0] = 1, meaning that s and t are both empty string, then the number of matching subsequence would be 1.
> dp[i][0] memaning no matter what s is, t is empty string, the number of matching subsequence between s and t would be 1. Because t as empty string, is always matching a string (s).
> But dp[0][j] is always 0, think about s is always empty string but t can be whatever string, t cannot be seen to matching anything of an empty string (s), so it's always 0.

### Approach
1. Dynamic Programming - dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j] or dp[i][j] = dp[i - 1][j] depending on whether s[i - 1] === t[j - 1].

```
/**
 * @param {string} s
 * @param {string} t
 * @return {number}
 */

/* 
* Dynamic Programming - dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j] or dp[i][j] = dp[i - 1][j] depending on whether s[i - 1] === t[j - 1].
* Time complexity; O(M * N)
* Spcace complexity: O(M * N)
*/
var numDistinct = function(s, t) {
    const [m, n] = [s.length, t.length];
    let dp = new Array(m + 1).fill().map(() => new Array(n + 1).fill(0));
    for (let i = 0; i <= m; i++) {
        dp[i][0] = 1;
    }

    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (s[i - 1] === t[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
            } else {
                dp[i][j] = dp[i - 1][j];
            }
        }
    }
    return dp[m][n];
};
```
