# Day 42 - 121 Best Time to Buy And Sell Stock | 122 Best Time to Buy And Sell Stock II 


## 121 Best Time to Buy And Sell Stock
[Description on Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/)

> ### Intuition
> There are multiple ways to solve it: brute force to iterate each prices[i]; greedy algorithm to buy at lowest prices then sell at highest prices; and dynamic programming.
> Particularly for dynamic programing, dp[i][0] means on i-th day, the max cash of keeping the stock and not sell; dp[i][1] means on i-th day, the max cash of selling the stock. 

### Approach
1. Brute Force - will be timed out
2. Greedy, finding the lowset and highest price. Then buy stock at lowest and sell it at highest.
3. Dynamic Programming - 1 dimension dp array, using dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i]).

```
/**
 * @param {number[]} prices
 * @return {number}
 */

/* Brute Force - will time out
* Time Complexity: O(N ^ 2)
* Space Complexity: O(1)
*/
var maxProfit = function(prices) {
    let result = 0;
    for (let i = 0; i < prices.length; i++) {
        for (let j = i + 1; j < prices.length; j++) {
            result = Math.max(result, prices[j] - prices[i]);
        }
    }
    return result;
};

/* Greedy
* Time Complexity: O(N ^ 2)
* Space Complexity: O(1)
*/
var maxProfit = function(prices) {
    let result = 0;
    let low = Infinity;
    for (let i = 0; i < prices.length; i++) {
        low = Math.min(low, prices[i]);
        result = Math.max(result, prices[i] - low);
    }
    return result;
};

/* Dynamic Programming - dp[i][0] means on i-th day, the max cash of keeping the stock and not sell; dp[i][1] means on i-th day, the max cash of selling the stock. 
* dp[i][0] can be derived from: a) keep it on i - 1th day and continue on keep; previously not buying any stock then buy the stock on ith day.
* dp[i][0] can be derived from: a) keep the cash on i - 1th day and continue on keeping the cash; have stock on i-1th day then sell it on ith day.
* Time Complexity: O(N)
* Space Complexity: O(N)
*/
var maxProfit = function(prices) {
    let dp = new Array(prices.length).fill([0, 0]);
    dp[0][0] = - prices[0];
    dp[0][1] = 0;

    for (let i = 1; i < prices.length; i++) {
        dp[i][0] = Math.max(dp[i - 1][0], - prices[i]);
        dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] + prices[i]);
    }
    return dp[prices.length - 1][1];
};

/* Dynamic Programming - using rolling array to save space
* Using a 2 * 2 array to save the result
* Time Complexity: O(N)
* Space Complexity: O(1)
*/
var maxProfit = function(prices) {
    let dp = new Array(2).fill([0, 0]);
    dp[0][0] = - prices[0];
    dp[0][1] = 0;

    for (let i = 1; i < prices.length; i++) {
        dp[i % 2][0] = Math.max(dp[(i - 1) % 2][0], - prices[i]);
        dp[i % 2][1] = Math.max(dp[(i - 1) % 2][1], dp[(i - 1) % 2][0] + prices[i]);
    }
    return dp[(prices.length - 1) % 2][1];
};
```


## 122 Best Time to Buy And Sell Stock II 
[Description on Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/)

> ### Intuition
> This is very similar to last exerice. The only difference is that you can buy and sell stocks multiple times.
> 

### Approach
1. Greedy - finding the positive price difference, accumulate those positive profits to reach maximum profits.
2. Dynamic Programming - 1 dimension dp array, using dp[j] = Math.min(dp[j], dp[j - coins[i]] + 1).

```
/**
 * @param {number[]} prices
 * @return {number}
 */

/* 
* Greedy - find the positive value differentce then accumulate the profits, it will become the maximum profits
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

/* 
* Dynamic Programming - - dp[i][0] means on i-th day, the max cash of keeping the stock and not sell; dp[i][1] means on i-th day, the max cash of selling the stock. 
* dp[i][0] can be derived from: a) keep it on i - 1th day and continue on keep; having cash on i-1th day then buy the stock on ith day.
* dp[i][0] can be derived from: a) keep the cash on i - 1th day and continue on keeping the cash; have stock on i-1th day then sell it on ith day.
* Time Complexity: O(N)
* Space Complexity: O(N)
*/
var maxProfit = function(prices) {
    let dp = new Array(prices.length).fill([0 ,0]);
    dp[0][0] = -prices[0];
    dp[0][1] = 0;

    for (let i = 1; i < prices.length; i++) {
        dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] - prices[i]);
        dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] + prices[i]);
    }
    return dp[prices.length - 1][1];
};

/* 
* Dynamic Programming - using rolling array to save space
* Using a 2 * 2 array to save the result
* Time Complexity: O(N)
* Space Complexity: O(N)
*/
var maxProfit = function(prices) {
    let dp = new Array(2).fill([0 ,0]);
    dp[0][0] = -prices[0];
    dp[0][1] = 0;

    for (let i = 1; i < prices.length; i++) {
        dp[i % 2][0] = Math.max(dp[(i - 1) % 2][0], dp[(i - 1) % 2][1] - prices[i]);
        dp[i % 2][1] = Math.max(dp[(i - 1) % 2][1], dp[(i - 1) % 2][0] + prices[i]);
    }
    return dp[(prices.length - 1) % 2][1];
};
```
