# Day 21 - 77 Combinations

## 77 Combinations
[Description on Leetcode](https://leetcode.com/problems/combinations/description/)

> ### Intuition
> Using brute force, we need to have k times of For-loop to loop into n. Obviously if k is a large number, we are not able to implement so many for-loops.
> In this case, we can consider to use backtracking of recursion. In this case, we are able to find out all the combinations without implementing many for-loops.
> Actually, this is a classic exercise for backtracking.

### Approach
1. Recursion: We need to do backtracking after each recursion. The reason is that, when we select a number, we push it into path. When we finish selecting the rest of the numbers of the path, we need to pop out the selected number so that we can select another number into same level of loop and start a new selection of using another number as a start.

```
/**
 * @param {number} n
 * @param {number} k
 * @return {number[][]}
 */
/*
* Time complexity: O(N * 2 ^ N)
* Space complexity: O(N)
*/
var combine = function(n, k) {
    let result = [];
    let path = [];

    const combineHelper = function (n, k, startIndex) {
        if (path.length === k) {
            console.log(path);
            return result.push([...path]);     // Deep clone because when creating path, it's empty array
        }
        for (let i = startIndex; i <= n - (k - path.length) + 1; i++) {
            // Originally should be for (let i = startIndex; i <= n; i++)
            path.push(i);
            combineHelper(n, k, i + 1);
            path.pop();
        }
    }

    combineHelper(n, k, 1);
    return result;
};
```
