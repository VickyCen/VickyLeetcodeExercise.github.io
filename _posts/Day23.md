# Day 23 - 39 Combination Sum | 40 Combination Sum II | 131 Palindrome Partitioning

## 39 Combination Sum
[Description on Leetcode](https://leetcode.com/problems/combination-sum/description/)

> ### Intuition
> We need to deal with a couple of things here:
> - Elements can be repeatedly chosen
> - When do we stop the recursion

### Approach
1. Sorted the candidates
2. Loop into each number and sum up the values.
3. When sum reaches to target, push into the result
4. Remember to backtrack

```js
/**
 * @param {number[]} candidates
 * @param {number} target
 * @return {number[][]}
 */
/*
* Time complexity: O(N * 2 ^ N)
* Space complexity: O(N)
*/
var combinationSum = function(candidates, target) {
    let result = [], path = [];
    candidates.sort((a, b) => a - b);    // Needs to be sorted

    const backTracking = (candidates, target, sum, startIndex) => {
        if (sum > target) return;
        if (sum === target) {
            return result.push([...path]);
        }
        for (let i = startIndex; i < candidates.length && candidates[i] <= target - sum; i++) {
            let cur = candidates[i];
            path.push(cur);
            sum += cur;
            backTracking(candidates, target, sum, i);   // Because an element can be picked multiple times, so i won't need to increment
            sum -= cur;
            path.pop();
        }
    }

    backTracking(candidates, target, 0, 0);
    return result;
};
```


## 40 Combination Sum II
[Description on Leetcode](https://leetcode.com/problems/combination-sum-ii/description/)

> ### Intuition
> It looks very similar to the last exercise, but it has 2 main differences:
> - Each elements only can be selected once in the result
> - Elements might be appears in candidates with more than once
>   This means that we need to deal with removing the duplicates. But how to do that? We can utilised an array called "used". If candidates[i] === candidates[i - 1] && used[i] === false, meaning this elements has been traverse before in the same level of path. We don't need to traverse this element value again, and we should use "continue" to jump out of this recursion.
>   
### Approach
1. Recursion: If current sum > targetSum(here is n), then we don't need to continue on the recursion because the remamining numbers won't meet the requirement. Otherwise, we remove the duplicates and push the current number to path and add up to the sum, then continue on selecting the next number. Remember to backtrack both the path and sum.

```js
/**
 * @param {number[]} candidates
 * @param {number} target
 * @return {number[][]}
 */

/*
* Time complexity: O(N * 2 ^ N)
* Space complexity: O(N)
*/
// Utilise used array to check whether element has been repeatedly selected
var combinationSum2 = function(candidates, target) {
    let result = [], path = [];
    let sum = 0;
    let len = candidates.length;
    let used = new Array(len).fill(false);
    candidates.sort((a, b) => a - b);

    const backTracking = (sum, startIndex) => {
        if (sum === target) {
            return result.push([...path]);
        }
        for (let i = startIndex; i < len && sum < target; i++) {
            let cur = candidates[i];
            if (candidates[i] + sum > target || i > 0 && cur === candidates[i - 1] && used[i - 1] === false) continue;
            
            path.push(cur);
            sum += cur;
            used[i] = true;
            backTracking(sum, i + 1);   // Elements can't be repeatedly selected, so we need to use i+1 here   
            sum -= cur;
            path.pop();
            used[i] = false;
        }
    }

    backTracking(0, 0);
    return result;
}; 


// No used array
var combinationSum2 = function(candidates, target) {
    let result = [], path = [];
    let sum = 0;
    let len = candidates.length;
    let used = new Array(len).fill(false);
    candidates.sort((a, b) => a - b);


    const backTracking = (sum, startIndex) => {
        if (sum === target) {
            return result.push([...path]);
        }
        for (let i = startIndex; i < len && candidates[i] + sum <= target; i++) {
            let cur = candidates[i];
            if (i > startIndex && cur === candidates[i - 1]) continue;
            
            path.push(cur);
            sum += cur;
            backTracking(sum, i + 1);   // Elements can't be repeatedly selected, so we need to use i+1 here   
            sum -= cur;
            path.pop();
        }
    }

    backTracking(0, 0);
    return result;
}
```

## 131 Palindrome Partitioning
[Description on Leetcode](https://leetcode.com/problems/palindrome-partitioning/)

> ### Intuition
> This exercise is hard because we need to tackle these problems:
> - how do we partition the string - we can abstract the partitioning as combination
> - how to simulate the partitioning process - we can use startIndex, the position of startIndex indicates the partition line
> - when does the recursion stop - when the partition line moves to the end of the string, which is startIndex >= str.length
> - how to represent the substring after partitioning - we can use string.substring or array.slice
> - how to check isPalindrome - we can use double pointer starting from left and right; or use a 2 dimention array to record isPal[i][j] isPalindrome or not 

### Approach
1. Recursion: similar to the combination, we use a for-loop for all elements in the string, and for each path, we use recurse to traverse all the path. Remember to back track after each recursion.
2. isPalindrome: Use double pointer. Or if s[0] == s[n-1] && s[1:n-2] isPalindrome, then s[0:n-1] isPalindrome. We can utilise a 2-dimension array to record isPal[i][j] isPalindrome or not. isPal[i][j] means s[i:j] (including inde i and j) is Palindrome or not.

```js
/**
 * @param {string} s
 * @return {string[][]}
 */

// Optimised way to check isPalindrome
// If s[0] == s[n-1] && s[1:n-2] isPalindrome, then s[0:n-1] isPalindrome
const isPalindrome = (str) => {
    const len = str.length;
    // isPal[i][j] means s[i:j] (including inde i and j) is Palindrome or not
    let isPal = new Array(len).fill(false).map(() => new Array(len).fill(false));
    for (let i = len - 1; i >= 0; i--) {
        // Loop from the end because we need to make sure when checking isPal[i] we have isPal[i+1] ready
        for (let j = i; j < len; j++) {
            if (i === j) isPal[i][j] = true;
            else if (j - i === 1) isPal[i][j] = str[i] === str[j];
            else isPal[i][j] = str[i] === str[j] && isPal[i + 1][j - 1];
        }
    }
    return isPal;
}

// const isPalindrome = (str, left, right) => {
//     while (left <= right) {
//         if (str[left++] !== str[right--]) return false;
//     }
//     return true;
// }

var partition = function(s) {
    let result = [], path = [];
    let len = s.length;
    const isPalindromeCheck = isPalindrome(s);

    const backtracking = (startIndex) => {
        if (startIndex >= len) {
            return result.push([...path]);
        }
        for (let i = startIndex; i < len; i++) {
            // if(!isPalindrome(s, startIndex, i)) continue;
            if(!isPalindromeCheck[startIndex][i]) continue;
            path.push(s.slice(startIndex, i + 1));   // Slice is left close right open range
            backtracking(i + 1);
            path.pop();
        }
    }

    backtracking(0);
    return result;
};
```
