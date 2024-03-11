# Day 49 - 647 Palindromic Substrings | 516 Longest Palindromic Subsequence

## 647 Palindromic Substrings
[Description on Leetcode](https://leetcode.com/problems/palindromic-substrings/description/)

> ### Intuition
> Approach 1: We define dp[i][j] using a boolean to indicate whether string[i...j] is a Palindromic Substrings, and we find that if string[i + 1...j - 1] is Palindromic Substring && s[i] === s[j], then string[i...j] is Palindromic Substring.
> So we know that if s[i] !== s[j], dp[i][j] = false (it must not be the Palindromic Substring)
> if s[i] === s[j], 3 cases we should consider: 1 single element ("a"). 2 elements ("aa") and 3 or more elements ("acba"). 
> if (i === j || j - i === 1), dp[i][j] = true;
> else dp[i][j] = dp[i + 1][j - 1] because it relies on string[i + 1...j - 1] is Palindromic Substring or not.
> Be aware that dp[i][j] depends on dp[i + 1][j - 1], so it needs to loop from bottom to top, left to right, to make sure dp[i + 1][x] is calculated before dp[i][x].

### Approach
1. Dynamic Programming - dp[i][j] using a boolean to indicate whether string[i...j] is a Palindromic Substrings.
2. 2 Pointers - for each s[i], it can be treated as the central elements, or one of the central element (i & i + 1) to check if the substring is Palindromic 

```
/**
 * @param {string} s
 * @return {number}
 */

/* 
* Dynamic Programming - dp[i][j] using a boolean to indicate whether string[i...j] is a Palindromic Substrings
* Time complexity; O(N ^ 2)
* Spcace complexity: O(N ^ 2)
*/
var countSubstrings = function(s) {
    const len = s.length;
    let dp = new Array(len).fill().map(() => new Array(len).fill(false));
    console.log(dp[0][0])
    let result = 0;

    for (let i = len - 1; i >= 0; i--) {
        for (let j = i; j < len; j++) {
            if (s[i] === s[j] && (j - i <= 1 || dp[i + 1][j - 1])) {
                dp[i][j] = true;
                result++;
            }
        }
    }
    return result;
};

/* 
* 2 Pointers - for each s[i], it can be treated as the central elements, or one of the central element (i & i + 1) to check if the substring is Palindromic 
* Time complexity; O(N ^ 2)
* Spcace complexity: O(1)
*/
var countSubstrings = function(s) {
    let result = 0;
    for (let i = 0; i < s.length; i++) {
        result += extend(s, i, i); // i as central element
        result += extend(s, i, i + 1); // i amd i + 1 as central element
    }
    return result;
};
function extend(str, left, right) {
    let res = 0;
    while (left >= 0 && right < str.length && str[left] === str[right]) {
        res++;
        left--;
        right++;
    }
    return res;
}
```


## 516 Longest Palindromic Subsequence
[Description on Leetcode](https://leetcode.com/problems/longest-palindromic-subsequence/)

> ### Intuition
> We define dp[i][j] to be the number of operations that makes word1 (ends with word1[i - 1] character) equal to word2 (ends with word2[j - 1] character), as we assume that if string[i + 1... j - 1] is Palindromic sequence && s[i] === s[j], then string[i...j] is also Palindromic sequence. So we can know that if s[i] === s[j], dp[i][j] = dp[i + 1][j - 1] + 2 (the sequence length need to plus s[i] and s[j]). If s[i] !== s[j], we know that string[i...j] is not Palindromic sequence, so we need to check the longest length of Palindromic sequence between dp[i + 1][j] and dp[i][j - 1], because we can only either add s[i] or s[j] to the sequence.
> Be aware that dp[i][j] depends on dp[i + 1][j - 1], so it needs to loop from bottom to top, left to right, to make sure dp[i + 1][x] is calculated before dp[i][x].


### Approach
1. Dynamic Programming - dp[i][j] to be the number of operations that makes word1 (ends with word1[i - 1] character) equal to word2 (ends with word2[j - 1] character).

```
/**
 * @param {string} s
 * @return {number}
 */

/* 
* Dynamic Programming - dp[i][j] to be the longest palindromic subsequence of string [i...j], string starts from s[i] and ends with s[j].
* Spcace complexity: O(N ^ 2)
*/
var longestPalindromeSubseq = function(s) {
    const len = s.length;
    let dp = new Array(len).fill().map(() => new Array(len).fill(0));

    for (let i = 0; i < len; i++) {
        dp[i][i] = 1;
    }

    for (let i = len; i>= 0; i--) {
        for (let j = i + 1; j < len; j++) {
            if (s[i] === s[j]) {
                dp[i][j] = dp[i + 1][j - 1] + 2;
            } else {
                dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
            }
        }
    }
    return dp[0][len - 1];
};
```
