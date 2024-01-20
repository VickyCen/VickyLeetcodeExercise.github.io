# Day 8 - 28 Find The Index of The First Occurrence in A String | 459 Repeated Substring Pattern

## 28 Find The Index of The First Occurrence in A String
[Description on Leetcode](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/description/)

> ### Intuition
> The description requires to find substring occurence within a string, this reminds us of using KMP algorithm. The KMP algorithm requires to generate a prefix table called next, to store the longest length of common prefix substirng & postfix substirng of index i. Remember, when talking with prefix or postfix, prefix needs to exclude the last element while postfix requires to exclude the first element.

### Approach
This is a typical problem using KMP algorithm. Please be aware of that, when generating the next table, the i starts from 1, **NOT 0**. Also, compared to brute force with time complexity of O(M * N), the time complexity can be reduced to O(m + n) using KMP algorithm.

```
/**
 * @param {string} haystack
 * @param {string} needle
 * @return {number}
 */

/*
* Time complexity：O(M + N)
* Space complexity：O(M)
*/
// Generate prefix table
function getNext(needle) {
    let next = [];
    let j = 0;
    next.push(j);

    for (let i = 1; i < needle.length; i++) {       // Note i starts from 1
        while (j > 0 && needle[j] !== needle[i]) {
            j = next[j - 1];
        }
        if (needle[j] === needle[i]) {
            j++;
        }
        next.push(j);
    }
    return next;
 
}

var strStr = function(haystack, needle) {
    if (needle.legnth === 0) return 0;   // if needle is empty or null string, we should return 0;

    const next = getNext(needle);
    let j = 0;
    for (let i = 0; i < haystack.length; i++) {
        while (j > 0 && haystack[i] !== needle[j]) {
            j = next[j - 1];
        }
        if (haystack[i] === needle[j]) {
            j++;
        }
        if (j === needle.length) {
            return i - needle.length + 1;
        }
    }
    return -1;
};
```


## 459 Repeated Substring Pattern
[Description on Leetcode](https://leetcode.com/problems/repeated-substring-pattern/description/)

### Approach
2 approaches:
1. Appending s to the end, making t = s + s, remove the first and the last element. If s is found within string t, then s has repeated substring pattern
2. KMP approach: generate the prefix table (next) for s, if len % (len - next[len - 1]) === 0 then s has repeated substring pattern. Note that we have to make sure next[len - 1] !== 0 because if next[len - 1] === 0, means s doesn't have any common longest prefix string, making (len - next[len - 1]) === len and this is not what we want.

```
/**
 * @param {string} s
 * @return {boolean}
 */

/*
* Moving & matching
* Time complexity：O(N)
* Space complexity：O(1)
*/
var repeatedSubstringPattern = function(s) {
    if (s.length <= 1) return false;
    let t = s + s;
    t = t.substring(1, t.length - 1);
    if (t.includes(s)) return true;
    return false;
};

/*
* KMP - len % (len - next[len-1]) === 0
* Time complexity：O(N)
* Space complexity：O(N)
*/
function generateNext(s) {
    let next = [];
    let j = 0;
    next.push(j);

    for (let i = 1; i < s.length; i++) {
        while (j > 0 && s[j] !== s[i]) {
            j = next[j - 1];
        }
        if (s[j] === s[i]) {
            j++;
        }
        next.push(j);
    }
    return next;
}

var repeatedSubstringPattern = function(s) {
    if (s.length === 0) return false;
    const next = generateNext(s);
    let len = s.length;

    if (next[len - 1] !== 0 && len % (len - next[len - 1]) === 0) return true;
    return false; 
};
```
