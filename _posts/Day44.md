# Day 44 - 309 Best Time to Buy And Sell Stock with Cooldown | 714 Best Time to Buy And Sell Stock with Transaction Fee

## 309 Best Time to Buy And Sell Stock with Cooldown
[Description on Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/)

> ### Intuition
> Pretty similar to LeetCode 122 but here plus the cooldown. Actually for each i-th day, we can divive the state of cash into 4 states:
> 0: Have brought the stock and keep holding the stock
> 1: Have sold the stock previously and has passed the cooldown
> 2: Sell the stock today
> 3: During cooldown
> We define dp[i][j] to be the max amount of cash with state j on i-th day.
> So dp[i][0] can be derived from: 1) On i-1th day, it's already held the stock dp[i - 1][0]; 2) On i-1th it has sold the sotck and passed the cooldown, then buy the stock on i-th day: dp[i - 1][1] - prices[i]; 3) On i-1th day it's the cooldown, then buy the stock on i-th day: dp[i - 1][3] - prices[i].
> Similarly, dp[i][1] can be derived from: 1) On i-1th day, it's already sold the stock and passed cooldown, continue on ith day: dp[i - 1][1]; 2) On i-1th day, it's during cooldown, so ith day has just passed cooldown without buying any stock: dp[i - 1][3].
> dp[i][2] = dp[i - 1][0] + prices[i] - held the stock on previous day then sell today
> dp[i][3] = dp[i - 1][2] - stock was sold on previous day

### Approach
1. Dynamic Programming - dp[i][j] means the max value of cash with state j. j represents 5 possible state on ith day.

```
/**
 * @param {number[]} prices
 * @return {number}
 */

/* 
* Dynamic Programming - dp[i][j] means on i-th day, the max cash of state j.
* Time complexity; O(N)
* Spcace complexity: O(N)
*/
var maxProfit = function(prices) {
    if (prices.length <= 1) return 0;
    else if (prices.length === 2) return Math.max(0, prices[1] - prices[0]);

    let n = prices.length;
    let dp = new Array(n).fill().map(() => new Array(4).fill(0));
    dp[0][0] = -prices[0];

    for (let i = 1; i < n; i++) {
        dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] - prices[i], dp[i - 1][3] - prices[i]);
        dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][3]);
        dp[i][2] = dp[i - 1][0] + prices[i];
        dp[i][3] = dp[i - 1][2];
    }
    return Math.max(dp[n - 1][1], dp[n - 1][2], dp[n - 1][3]);
};


/* 
* Dynamic Programming - Optimised space
* Time complexity; O(N)
* Spcace complexity: O(1)
*/
var maxProfit = function(prices) {
    if (prices.length <= 1) return 0;
    else if (prices.length === 2) return Math.max(0, prices[1] - prices[0]);

    let n = prices.length;
    let dp = new Array(4).fill(0);
    dp[0] = -prices[0];

    for (let i = 1; i < n; i++) {
        let temp1 = dp[0];
        let temp2 = dp[2];
        dp[0] = Math.max(dp[0], dp[1] - prices[i], dp[3] - prices[i]);
        dp[1] = Math.max(dp[1], dp[3]);
        dp[2] = temp1 + prices[i];
        dp[3] = temp2;
    }
    return Math.max(...dp);
};
```


## 714 Best Time to Buy And Sell Stock with Transaction Fee
[Description on Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/description/)

> ### Intuition
> It's very similar to Leetcode 122 as well, the only difference is that for every time selling the stock, we need to minus the fee.
> We define dp[i][0] to be on i-th day, the max value of cash when holding stock, and dp[i][1] to be on i-th day, the max value of cash when not having stock.
> So dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] - prices[i]) while dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] + prices[i] - fee) - need to deduct the fee.

### Approach
1. Dynamic Programming - dp[i][0] means on i-th day, the max cash of either holding the stock or sell it

```
/**
 * @param {number[]} prices
 * @param {number} fee
 * @return {number}
 */

/* 
* Dynamic Programming - dp[i][0] / dp[i][1] means on i-th day, the max cash of either holding the stock or sell it
* Time complexity; O(N)
* Spcace complexity: O(N)
*/
var maxProfit = function(prices, fee) {
    let dp = new Array(prices.length).fill().map(() => new Array(2).fill(0));
    dp[0][0] = -prices[0];   // dp[0][1] = 0;

    for (let i = 1; i < prices.length; i++) {
        dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] - prices[i]);
        dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] + prices[i] - fee);
    }
    // return Math.max(dp[prices.length - 1][0], dp[prices.length - 1][1]);
    return dp[prices.length - 1][1];
};

/* 
* Dynamic Programming - Optimised space
* Time complexity; O(N)
* Spcace complexity: O(1)
*/
var maxProfit = function(prices, fee) {
    let dp = new Array(2).fill(0);
    dp[0] = -prices[0];   // dp[0][1] = 0;

    for (let i = 1; i < prices.length; i++) {
        dp[0] = Math.max(dp[0], dp[1] - prices[i]);
        dp[1] = Math.max(dp[1], dp[0] + prices[i] - fee);
    }
    // return Math.max(dp[prices.length - 1][0], dp[prices.length - 1][1]);
    return dp[1];
};
```
