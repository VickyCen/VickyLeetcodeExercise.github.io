# Day 48 - 583 Delete Operation for Two Strings | 72 Edit Distance

## 583 Delete Operation for Two Strings
[Description on Leetcode](https://leetcode.com/problems/delete-operation-for-two-strings/description/)

> ### Intuition
> Approach 1: We define dp[i][j] to be the minimum amount of characters that word1 and word2 needs to be deleted, where word1 ends with word1[i - 1] character, and word2 ends with word2[j - 1] character.
> if (word1[i - 1] === word2[j - 1]) dp[i][j] = dp[i - 1][j - 1] because we should keep word1[i - 1] and word2[j - 1], no deletion, so it should be equal to dp[i - 1][j - 1].
> if (word1[i - 1] === word2[j - 1]) dp[i][j] = Math.min(dp[i - 1][j] + 1, dp[i][j - 1] + 1, dp[i - 1][j - 1] + 2) because we could either delete word1[i - 1] or word2[j - 1] or both word1[i - 1] & word2[j - 1].
> In this approach, we need to understand the initial value of dp[i][0] and dp[0][j]. According to definition, dp[i][0] means word2 is empty string, for word1, we need to delete i characters to make it to be an empty string, so to make it equal to word2. So dp[i][0] = i; Similarly, dp[0][j] = j.
> Approach 2: Treat it like longest common sequence. We find word1 and word2's longest common sequence, then the rest of characters are the ones should be deleted.

### Approach
1. Dynamic Programming - dp[i][j] to be the minimum amount of characters that word1 and word2 needs to be deleted, where word1 ends with word1[i - 1] character, and word2 ends with word2[j - 1] character.
2. Dynamic Programming - dp[i][j] to be the longest common sequences of word1 and word2, then use the length of word1 and word2 minus the longest common sequence length.

```
/**
 * @param {string} word1
 * @param {string} word2
 * @return {number}
 */

/* 
* Dynamic Programming - dp[i][j] to be the minimum amount of characters that word1 and word2 needs to be deleted, where word1 ends with word1[i - 1] character, and word2 ends with word2[j - 1] character.
* Time complexity; O(M * N)
* Spcace complexity: O(M * N)
*/
var minDistance = function(word1, word2) {
    const [m , n] = [word1.length , word2.length];
    let dp = Array(m + 1).fill().map(() => Array(n + 1).fill(0));
    for (let i = 0; i <= m; i++) {
        dp[i][0] = i;
    }
    for (let j = 0; j <= n; j++) {
        dp[0][j] = j;
    }

    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (word1[i - 1] === word2[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1];
            } else {
                dp[i][j] = Math.min(dp[i - 1][j] + 1, dp[i][j - 1] + 1, dp[i - 1][j - 1] + 2);
            }
        }
    }
    return dp[m][n];
};

/* 
* Dynamic Programming - dp[i][j] to be the longest common sequences of word1 and word2, then use the length of word1 and word2 minus the longest common sequence length.
* Time complexity; O(M * N)
* Spcace complexity: O(M * N)
*/
var minDistance = function(word1, word2) {
    const [m , n] = [word1.length , word2.length];
    let dp = Array(m + 1).fill().map(() => Array(n + 1).fill(0));

    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (word1[i - 1] === word2[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }
    return m + n - dp[m][n] * 2;
};
```


## 72 Edit Distance
[Description on Leetcode](https://leetcode.com/problems/edit-distance/description/)

> ### Intuition
> We define dp[i][j] to be the number of operations that makes word1 (ends with word1[i - 1] character) equal to word2 (ends with word2[j - 1] character).
> So if (word1[i - 1] === word2[j - 1]) dp[i][j] = dp[i - 1][j - 1] because we don't need to do any operation.
> If (word1[i - 1] !== word2[j - 1]), we need operations of delete / add / replace:
> Delete: we can delete word1[i - 1], so dp[i][j] = dp[i - 1][j]; we can also delete word2[j - 1], so dp[i][j] = dp[i][j - 1];
> Add: it's same as delete - example: word1 = "ad", word2 = "a", add "d" to word2 is actually same as delete "d" from word1;
> Replace: we can repleace either word1[i - 1] or word2[j - 1], so we need 1 operation of replace, to make word1[i - 1] === word2[j - 1]. Previously we say if (word1[i - 1] === word2[j - 1]) dp[i][j] = dp[i - 1][j - 1], so in this case dp[i][j] = dp[i - 1][j - 1] + 1 (1 operation of replace).

### Approach
1. Dynamic Programming - dp[i][j] to be the number of operations that makes word1 (ends with word1[i - 1] character) equal to word2 (ends with word2[j - 1] character).

```
/**
 * @param {string} word1
 * @param {string} word2
 * @return {number}
 */

/* 
* Dynamic Programming - dp[i][j] to be the number of operations that makes word1 (ends with word1[i - 1] character) equal to word2 (ends with word2[j - 1] character).
* Time complexity; O(M * N)
* Spcace complexity: O(M * N)
*/
var minDistance = function(word1, word2) {
    const [m, n] = [word1.length, word2.length];
    let dp = Array(m + 1).fill().map(() => Array(n + 1).fill(0));
    for (let i = 0; i <= m; i++) {
        dp[i][0] = i;
    }
    for (let j = 0; j <= n; j++) {
        dp[0][j] = j;
    }

    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (word1[i - 1] === word2[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1];
            } else {
                dp[i][j] = Math.min(dp[i - 1][j] + 1, dp[i][j - 1] + 1, dp[i - 1][j - 1] + 1);
            }
        }
    }
    return dp[m][n];
};
```
