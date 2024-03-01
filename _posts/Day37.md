# Day 37 - 1049 Last Stone Weight II | 494 Target Sum | 474 Ones And Zeroes

## 1049 Last Stone Weight II
[Description on Leetcode](https://leetcode.com/problems/last-stone-weight-ii/description/)

> ### Intuition
> We will need to divide the stones into 2 sets with weight as much similar as possible. For each stones[i], its weight is stones[i] and value is stones[i]. So we can define dp[j] = Math.max(dp[j], dp[j - stones[i]] + stones[i]).
 
### Approach
1. Dynamic Programming - 1 dimension dp array, using dp[j] = Math.max(dp[j], dp[j - stones[i]] + stones[i]) to make the sum of stone's values to equal to Math.floor(sum / 2).

```
/**
 * @param {number[]} stones
 * @return {number}
 */

/*
* Dynamic Programming - 1 dimension dp array
* Time Complexity: O(M * N) where M is the sum of the stones' weights and n is the amount of stones
* Space Complexity: O(M)
*/
var lastStoneWeightII = function(stones) {
    const sum = stones.reduce((a, b) => a + b);
    const target = Math.floor(sum / 2);
    let dp = new Array(target + 1).fill(0);

    for (let i = 0; i < stones.length; i++) {
        for (let j = target; j >= stones[i]; j--) {
            dp[j] = Math.max(dp[j], dp[j - stones[i]] + stones[i]);
        }
    }
    return sum - dp[target] - dp[target];
};
```


## 494 Target Sum
[Description on Leetcode](https://leetcode.com/problems/target-sum/description/)

> ### Intuition
> If the target exists, we divide the array into a left and right partition, that means we have leftSum - rightSum = target. And we can also calculate the sum of the elements of the array, so leftSum + rightSum = sum, so we have leftSum = (target + sum) / 2. Therefore, the exercise can be converted to: finding out how many ways to fill in bag weight of leftSum => dp[j] += dp[j - nums[i]].
> Also, we need to handle these cases:
> if (Math.abs(S) > sum) return 0;   // the target is larger than the sum
> if ((target + sum) % 2)  return 0;  // leftSum shouldn't be non-integer, as all the elements are integer.
> Also, dp[0] = 1 because take array [0]，target = 0 as an example, no matter whether we choose + or -, there is 1 way to make target = 0 from array of [0].

### Approach
1. Dynamic Programming - 1 dimension dp array, using dp[j] += dp[j - nums[i]]. And dp[j] means there are dp[j] ways to fill the bag with size j.
2. BackTracking - iterate all posible combination that the sum equal to leftSum.

```
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */

/*
* Backtracking - it will timeout
*/
var findTargetSumWays = function(nums, target) {
    let result = [];
    let path = [];

    const backTracking = (array, targetSum, curSum, startIndex) => {
        if (curSum === targetSum) {
            result.push(path);
        }

        for (let i = startIndex; i < array.length; i++) {
            curSum += array[i];
            path.push(array[i]);
            backTracking(array, targetSum, curSum, i + 1);
            path.pop();
            curSum -= array[i];
        }
    }

    const sum = nums.reduce((a, b) => a + b);
    const leftSum = (target + sum) / 2;

    if (Math.abs(target) > sum) return 0;     
    if ((target + sum) % 2) return 0;
    nums.sort((a, b) => a - b);
    backTracking(nums, leftSum, 0, 0);
    return result.length;
}


/*
* Dynamic Programming - 1 dimension dp array
* Time Complexity: O(M * N) where M is the leftSum and n is the length of nums array
* Space Complexity: O(M)
*/
var findTargetSumWays = function(nums, target) {
    const sum = nums.reduce((a, b) => a + b);
    if (Math.abs(target) > sum) return 0;     
    if ((target + sum) % 2) return 0;

    const leftSum = (target + sum) / 2;
    let dp = new Array(leftSum + 1).fill(0);
    dp[0] = 1;

    for (let i = 0; i < nums.length; i++) {
        for (let j = leftSum; j >= nums[i]; j--) {
            dp[j] += dp[j - nums[i]];
        }
    }
    return dp[leftSum];
};
```


## 474 Ones And Zeroes
[Description on Leetcode](https://leetcode.com/problems/ones-and-zeroes/description/)

> ### Intuition
> This is actually a bagWeight Problem, and we just have 2 bags, bag for 0 with size m, and bag for 1 with size n.
> We can define dp[i][j] to be the size of the largest subset of strs that there are at most m 0's and n 1's in the subset.
> We can set 2 variables, zeroNum menas the number of 0 within previous strings that has been iterated; oneNum menas the number of 1 within previous strings that has been iterated. zeroNum and oneNum are like weights of item. Then the 0s(m) and 1s(n) in the whole strs are like the value of items.
> dp[i][j] = Math.max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1).

### Approach
1. Dynamic Programming - 1 dimension dp array, using dp[i][j] = Math.max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1).

```
/**
 * @param {string[]} strs
 * @param {number} m
 * @param {number} n
 * @return {number}
 */

/*
* Dynamic Programming - 1 dimension dp array
* Time Complexity: O(M * N)
* Space Complexity: O(M)
*/
var findMaxForm = function(strs, m, n) {
    let dp = new Array(m + 1).fill().map(() => new Array(n + 1).fill(0));
    let zeroNum, oneNum;

    for (let str of strs) {
        zeroNum = oneNum = 0;
        for (let c of str) {
            if (c === '0') zeroNum++;
            else oneNum++;
        }

        for (let i = m; i >= zeroNum; i--) {
            for (let j = n; j >= oneNum; j--) {
                dp[i][j] = Math.max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1);
            }
        }
    }
    return dp[m][n];
};
```
