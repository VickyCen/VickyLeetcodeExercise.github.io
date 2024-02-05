# Day 22 - 216 Combination Sum III | 17 Letter Combinations of A Phone Number

## 216 Combination Sum III
[Description on Leetcode](https://leetcode.com/problems/combination-sum-iii/description/)

> ### Intuition
> Very similar to combination, but here we need to pass the target sum as one of the argument in the backtracking function. Remember to backtrack the path by popping out and reduce the current value from sum after each recursion.

### Approach
1. Recursion: If current sum > targetSum(here is n), then we don't need to continue on the recursion because the remamining numbers won't meet the requirement. Otherwise, we push the current number to path and add up to the sum, then continue on selecting the next number. Remember to backtrack both the path and sum.

```
/**
 * @param {number} k
 * @param {number} n
 * @return {number[][]}
 */
/*
* Time complexity: O(N * 2 ^ N)
* Space complexity: O(N)
*/
var combinationSum3 = function(k, n) {
    let result = [];
    let path = [];
    const combinationHelper = (k, targetSum, sum, startIndex) => {
        if (targetSum < sum) return;
        if (path.length === k) {
            if (targetSum === sum) {
                console.log(path);
                return result.push([...path]);
            }
        }
        for (let i = startIndex; i <= 9 - (k - path.length) + 1; i++) {
            path.push(i);
            sum += i;
            combinationHelper(k, targetSum, sum, i + 1);
            sum -= i;
            path.pop();
        }
    }
    combinationHelper(k, n, 0, 1);
    return result;
};
```


## 17 Letter Combinations of A Phone Number
[Description on Leetcode](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/)

> ### Intuition
> Here we need to address 3 problems:
> 1. mapping the numbers to letters - using hash map (array / map)
> 2. traverse all the possible letters - don't do force brute as we can't write many for-loop. Instead, we use recursion and backtracking for all possible path
> 3. handle 0, 1,*,# special case - mapping them to empty string (no letter will be selected)

### Approach
1. Recursion: If current sum > targetSum(here is n), then we don't need to continue on the recursion because the remamining numbers won't meet the requirement. Otherwise, we push the current number to path and add up to the sum, then continue on selecting the next number. Remember to backtrack both the path and sum.

```
/**
 * @param {string} digits
 * @return {string[]}
 */

/*
* Time complexity: O(3 ^ M * 4 ^ N) where m is the amount of numbers that corelates to 3 letters and n is the amount of numbers that corelates to 4 letters
* Space complexity: O(3 ^ M * 4 ^ N)
*/
var letterCombinations = function(digits) {
    let result = [];
    let path = [];

    const map = {
        "0": "",
        "1": "",
        "2": "abc",
        "3": "def",
        "4": "ghi",
        "5": "jkl",
        "6": "mno",
        "7": "pqrs",
        "8": "tuv",
        "9": "wxyz",
        "#": "",
        "*": "",
    }

    const backTracking = (string, k, index) => {
        if (path.length === k) return result.push(path.join(""));

        for (const letter of map[string[index]]) {
            path.push(letter);
            backTracking(string, k, index + 1);
            path.pop();
        }
    }

    let k = digits.length;
    if (k === 0) return [];
    backTracking(digits, k, 0);
    return result;
};
```
