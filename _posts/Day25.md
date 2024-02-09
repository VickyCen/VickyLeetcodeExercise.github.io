# Day 25 - 491 Non Decreasing Subsequences | 46 Permutations | 47 Permutations II

## 491 Non Decreasing Subsequences
[Description on Leetcode](https://leetcode.com/problems/non-decreasing-subsequences/description/)

> ### Intuition
> We need to fully understand the description here. We don't need to sort the nums array, because if we sort it, the sequence of elements will changes, and this is not what the description requires.
> In order to remove duplicate subsequences in the result, instead of sorting the array, we can leverage the used array / set to record whether a value has been added to the result.
> We can use both set and array here. But array is one of the implementation of hash map, since the description mentions element values is [=100, 100], the max length is fixed, so we can use array. And array is generally more efficient than set in most cases.

### Approach
1. Recursion - use an array / set called used to record whether this element has been added into the result
2. Remember to backtrack the path but not the used array/set. Because the used array/set is initialised in each level, so it won't store the information from last level recursion.

```
/**
 * @param {number[]} nums
 * @return {number[][]}
 */

/* 
* Time complexity: O(N * 2^N)
* Space complexity: O(N)
*/
/* Array as used is efficient than using set here */
var findSubsequences = function(nums) {
    let result = [], path = [];

    const backTracking = (startIndex) => {
        if (path.length > 1            ) {
            result.push([...path]);
        } 
        const used = new Array(201).fill(false);    // The value can be a negative number but array index can't be negative. Here's a trick, we + 100 for the value so we can map the value even to a negative number
        for (let i = startIndex; i < nums.length; i++) {
            if (path.length && nums[i] < path[path.length - 1] || used[nums[i] + 100] === true) continue;
            path.push(nums[i]);
            used[nums[i] + 100] = true;
            backTracking(i + 1);
            path.pop();   // We don't need to backtrack the used array here. Please notice the used array is intialised before each recursion for-loop, so basically it will be initialised for each level of the tree, and garbage collected after the level of recursion. So we don't even bother by the backtracking the used array here.
        }

    }

    backTracking(0);
    return result;
};

// Use set to remove duplicate
var findSubsequences = function(nums) {
    let result = [], path = [];

    const backTracking = (startIndex) => {
        if (path.length > 1            ) {
            result.push([...path]);
        } 
        const set = new Set();
        for (let i = startIndex; i < nums.length; i++) {
            if (path.length && nums[i] < path[path.length - 1] || set.has(nums[i])) continue;
            path.push(nums[i]);
            set.add(nums[i]);
            backTracking(i + 1);
            path.pop();
        }

    }

    backTracking(0);
    return result;
};
```


## 46 Permutations
[Description on Leetcode](https://leetcode.com/problems/permutations/description/)

> ### Intuition
> The description mentioned "distinct integers", this is an important information. This means we don't need to consider duplicate removal.
> But we still need a used array. This is not used for duplicate removal, it's to be used for checking whether an elememnt has been picked in the path.
> And permutations' length is equal to nums' length

### Approach
1. Recursion - used array to record whether this element has been picked into the current permutation. And we don't need startIndex here because in permutations, different sequence of elements means different permutation. For example, [1, 2] and [2, 1] are different permutation.

```
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
/*
* Time complexity: O(N!)
* Space complexity: O(N)
*/
var permute = function(nums) {
    let result = [], path = [];
    nums.sort((a, b) => a - b);
    let used = new Array(nums.length).fill(0);

    const backTracking = (used) => {
        if (path.length === nums.length) return result.push([...path]);

        for (let i = 0; i < nums.length; i++) {
            if (used[i] === 1) continue;
            path.push(nums[i]);
            used[i] = 1;
            backTracking(used);
            used[i] = 0;
            path.pop();
        }
    }
    backTracking(used);
    return result;
}; 
```

## 47 Permutations II
[Description on Leetcode](https://leetcode.com/problems/permutations-ii/description/)

> ### Intuition
> The description mentions "that might contain duplicates" and requires to "all possible **unique** permutations". So basically we need to remove duplicates.
> Again, we can leverage a "used" array here to check whehter same value element is selected in the current permutation and compare nums[i] === nums[i - 1] to remove duplicates. Using either used[i - 1] === false or used[i - 1] === true will work. While used[i - 1] === false means remove duplicates in the same level, used[i - 1] === true means remove duplicates of a path. And if we draw a graph to analyse, we will find that using used[i - 1] === false to remove duplicates in the same level is much efficient than the other approach. Here we'll use used[i - 1] === false to solve the problem.

### Approach
1. Recursion - similar to last exercise, but the difference here is that we have to remove duplicates here.

```
/**
 * @param {number[]} nums
 * @return {number[][]}
 */

/*
* Time complexity: O(N! * N)
* Space complexity: O(N)
*/
var permuteUnique = function(nums) {
    let result = [], path = [];
    nums.sort((a, b) => a - b);

    const used = new Array(nums.length).fill(false);

    const backTracking = (used) => {
        if (path.length === nums.length) return result.push([...path]);

        for (let i = 0; i < nums.length; i++) {
            if (i > 0 && nums[i] === nums[i - 1] && used[i - 1] === false) continue;
            if (used[i] === false) { // this condition is important, we can't miss it because we need to make sure same element can't be picked twice
                path.push(nums[i]);
                used[i] = true;
                backTracking(used);
                used[i] = false;
                path.pop();
            }
        }
    }

    backTracking(used);
    return result;
};
```
