# Day 24 - 93 Restore IP Addresses | 78 Subsets | 90 Subsets II

## 93 Restore IP Addresses
[Description on Leetcode](https://leetcode.com/problems/restore-ip-addresses/submissions/501064981/)

> ### Intuition
> It is very similiar to Palindrome. We need to utilise startIndex to indication the partition position. Also, we need to use numPoint to indicate how many numbers in the IP address we have deal with. Numpoint === 4 means we have processed 4 numbers in the IP address.

### Approach
1. Recurse and backtrack all the number into the string
2. When we've processed 4 numpoints and all the number is valid IP address and it reaches s.end, we can stop recursion and record into the result.
3. Check whether this number is a valid address number:
   - number > 1 digit && number[0] === 0 -- invalid
   - number < 0 || number < 255 -- invalid

```
/**
 * @param {string} s
 * @return {string[]}
 */

/* 
* Time complexity: O(3 ^ 4)
* Space complexity: O(N)
*/
const isValid = (str) => {
    if (str[0] === '0' && str.length > 1) return false;
    else if (+str < 0 || +str > 255) return false;
    return true;
}
var restoreIpAddresses = function(s) {
    let result = [], path = [];
    const backTracking = (startIndex, numPoint) => {
        if (numPoint > 4) return;
        if (numPoint === 4 && startIndex === s.length) {
            return result.push(path.join("."));
        }
        for (let i = startIndex; i < s.length; i++) {
            let str = s.slice(startIndex, i + 1);
            if (isValid(str)) {
                path.push(str);
                numPoint++;
                backTracking(i + 1, numPoint);
                numPoint--;
                path.pop();
            }
        }
    }

    backTracking(0, 0);
    return result;
};
```


## 78 Subsets
[Description on Leetcode](https://leetcode.com/problems/subsets/description/)

> ### Intuition
> Different from the last exercise and the combinations exercies, here requires to record all the result of each nodes (including leaf nodes and branches nodes), while last exercise requires to only record all its leaf nodes.
> So when we push into a node into path, we need to do the push when the recursion starts, to make sure this nodes has been recorded.

### Approach
1. Use recursion and backtracking to traverse all the nodes. Record all the path into results; Make sure the condition to stop the recursion is startIndex > nums.length instead of startIndex >= nums.length because we don't want to miss the last element added into the result set.

```
/**
 * @param {number[]} nums
 * @return {number[][]}
 */

/*
* Time complexity: O(N * 2 ^ N)
* Space complexity: O(N)
*/
var subsets = function(nums) {
    let result = [], path = [];
    
    const backTracking = (startIndex) => {
        if (startIndex > nums.length) return;    // Use > instead of >= here, because we don't want to miss the last element pushed into result;
        result.push([...path]);

        for (let i = startIndex; i < nums.length; i++) {
            path.push(nums[i]);
            backTracking(i + 1);
            path.pop();
        }
    }

    backTracking(0);
    return result;
};
```

## 90 Subsets II
[Description on Leetcode](https://leetcode.com/problems/subsets-ii/)

> ### Intuition
> This is similiar to last exercise, but this exercise mentions the elements within nums might be repeated. So we need to consider to remove the duplicates in the same level tree during traversal. Actually, this exercise is a combination of Leetcode 78 Subsets and Leetcode 40 Combination Sum II

### Approach
1. Recursion: traverse all the elements and add all the path into result set
2. Remove duplicates if i > startIndex && nums[i] === nums[i - 1]

```
/**
 * @param {number[]} nums
 * @return {number[][]}
 */

/*
* Time complexity: O(N * 2 ^ N)
* Space complexity: O(N)
*/
var subsetsWithDup = function(nums) {
    let result = [], path = [];
    nums.sort((a, b) => a - b);

    const backTracking = (startIndex) => {
        result.push([...path]);
        if (startIndex >= nums.length) return;
        for (let i = startIndex; i < nums.length; i++) {
            if (i > startIndex && nums[i] === nums[i - 1]) continue;

            path.push(nums[i]);
            backTracking(i + 1); 
            path.pop();
        }
    } 
    backTracking(0);
    return result;
};
```
