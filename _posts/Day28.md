# Day 28 - 122 Best Time to Buy And Sell Stock II | 55 Jump Game | 45 Jump Game II

## 122 Best Time to Buy And Sell Stock II
[Description on Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/)

> ### Intuition
> We need to understand how to divide the profits. If we buy the stock on day 0 and sell it on day 3, the profit = prices[3] - prices[0], which is equal to (prices[3] - prices[2]) + (prices[2] - prices[1]) + (prices[1] - prices[0]). And if we can come up with each day's profit, and "greedily" pick only the positive profits, then it will become the largest profits. 

### Approach
1. Greedy: loop into each day, calculate the profits of the day, only sum up with positive profits.

```
/**
 * @param {number[]} prices
 * @return {number}
 */

/* 
* Time complexity; O(N)
* Spcace complexity: O(1)
*/
var maxProfit = function(prices) {
    if (prices.length <= 1) return 0;
    let maxProfit = 0;
    for (let i = 1; i < prices.length; i++) {
        if (prices[i] - prices[i - 1] > 0) maxProfit += prices[i] - prices[i - 1];
    }
    return maxProfit;
};
```


## 55 Jump Game
[Description on Leetcode](https://leetcode.com/problems/jump-game/description/)

> ### Intuition
> What we actually care is the cover range (largest distance it can take on index i). If the cover range eventually >= nums.length - 1, meaning it can reach the end of the array. So our goal is to loop into the array and get the largest cover range for the i-th element.

### Approach
1. Greedy: find out the largest distance range as cover, loop into the elements within the curDistance and update the cover. If eventually cover >= nums.length - 1 meaning it can reach the end.

```
/**
 * @param {number[]} nums
 * @return {boolean}
 */

/* 
* Time complexity; O(N)
* Spcace complexity: O(1)
*/
var canJump = function(nums) {
    if (nums.length === 1) return true;

    let cover = 0;

    for (let i = 0; i <= cover; i++) {
        cover = Math.max(cover, i + nums[i]);
        if (cover >= nums.length - 1) return true;
    }
    return false;
};
```

## 45 Jump Game II
[Description on Leetcode](https://leetcode.com/problems/jump-game-ii/description/)

> ### Intuition
> The thoughts is very similar to last exercise, but we need to store curDistance(the current largest distance) and nextDistance(largest distance if we take next steps). Because if curDistance < nums.length - 1, we have to take at least one more step. So we have to update nextDistance and when i === curDistance, meaning we've reached current largest distance, we need to check if we make it to the end of the array, if we haven't, we need to add 1 jump.

### Approach
1. Greedy: We loop into the array and calculate the nextDistance. If i === curDistance and we haven't made to the destination, we have to take one more jump. So we have to update the curDistance = nextDistance.
2. The simplified version is similar to that. But we only care about how many jumps we've taken when we reached nums.length - 2. Because that means the final jump is either jump + 1 (the curDistance < nums.length - 1 and we have to take one more step) or jump(the curDistance >= nums.length - 1).

```
/**
 * @param {number[]} nums
 * @return {number}
 */

/* 
* Time complexity; O(N)
* Spcace complexity: O(1)
*/
var jump = function(nums) {
    if (nums.length === 1) return 0;
    let nextDistance = curDistance = 0;
    let result = 0;
    for (let i = 0; i < nums.length; i++) {
        nextDistance = Math.max(i + nums[i], nextDistance);
        if (i === curDistance) {
            result++;
            curDistance = nextDistance;
            if (nextDistance >= nums.length - 1) return result;
        }
    }
    return result;
};

// Simplified version
var jump = function(nums) {
    let nextDistance = curDistance = 0;
    let result = 0;
    for (let i = 0; i < nums.length - 1; i++) {
        // We only care about when i === nums.length - 2, how many jumps we need, because the result is eitehr jump + 1 or jump
        nextDistance = Math.max(i + nums[i], nextDistance);
        if (i === curDistance) {
            result++;
            curDistance = nextDistance;
        }
    }
    return result;
};
```
