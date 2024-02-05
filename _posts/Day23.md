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

```
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

```
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
> It looks very similar to the last exercise, but it has 2 main differences:
> - Each elements only can be selected once in the result
> - Elements might be appears in candidates with more than once
>   This means that we need to deal with removing the duplicates. But how to do that? We can utilised an array called "used". If candidates[i] === candidates[i - 1] && used[i] === false, meaning this elements has been traverse before in the same level of path. We don't need to traverse this element value again, and we should use "continue" to jump out of this recursion.
>   
### Approach
1. Recursion: If current sum > targetSum(here is n), then we don't need to continue on the recursion because the remamining numbers won't meet the requirement. Otherwise, we remove the duplicates and push the current number to path and add up to the sum, then continue on selecting the next number. Remember to backtrack both the path and sum.

```
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
