# Day 43 - 123 Best Time to Buy And Sell Stock III | 188 Best Time to Buy And Sell Stock IV


## 123 Best Time to Buy And Sell Stock III
[Description on Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/description/)

> ### Intuition
> Compared to previous stock buy and sell exercise, this exercise is much harder. But as it's limited to sell at most 2 transactions, for each ith day, the state of cash can be divided into 5 states:
> - 0: no operation
> - 1: first time to buy stock
> - 2: first time to sell stock
> - 3: second time to buy stock
> - 4: second time to sell stock.
> 
> And for each i, j, dp[i][j] means the max value of cash with state j. So dp[i][1] as an example, it can be derived from 2 ways:
> - On i-1th day, it still deosn't have any operations, then on ith day, it first time to buy stock. So dp[i][1] = dp[i - 1][0] - prices[i]
> - On i-1th day, it already first buy stocks, and on ith day, it remains the same state. So dp[i][1] = dp[i - 1][1].
> Therefore, dp[i][1] = Math.max(dp[i - 1][0] - prices[i], dp[i - 1][1]).
> Similarly, dp[i][2] = Math.max(dp[i - 1][1] + prices[i], dp[i - 1][2]); dp[i][3] = Math.max(dp[i - 1][2] - prices[i], dp[i - 1][3]); dp[i][4] = Math.max(dp[i - 1][3] + prices[i], dp[i - 1][4]).
> And the final result of maximum cash value should be dp[prices.length - 1][4]. Because the second time to sell stock must earn more cash than buying stock. And dp[prices.length - 1][4] also includes the case of dp[prices.length - 1][2]. You can think of after first selling stock, it buy then sell on the same day to achieve second time of selling the stock.

### Approach
1. Dynamic Programming - dp[i][j] means the max value of cash with state j. j represents 5 possible state on ith day.

```
/**
 * @param {number[]} prices
 * @return {number}
 */

/* 
* Dynamic Programming - dp[i][0] means on i-th day, the max cash of state j.
* Time complexity; O(N)
* Spcace complexity: O(N)
*/
var maxProfit = function(prices) {
    if (prices.length <= 1) return 0;
    let dp = new Array(prices.length).fill().map(() => new Array(5).fill(0));
    dp[0][1] = dp[0][3] = -prices[0];

    for (let i = 1; i < prices.length; i++) {
        dp[i][0] = dp[i - 1][0];
        dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] - prices[i]);
        dp[i][2] = Math.max(dp[i - 1][2], dp[i - 1][1] + prices[i]);
        dp[i][3] = Math.max(dp[i - 1][3], dp[i - 1][2] - prices[i]);
        dp[i][4] = Math.max(dp[i - 1][4], dp[i - 1][3] + prices[i]);
    }
    return dp[prices.length - 1][4];
};

/* 
* Dynamic Programming - Optimised space
* Time complexity; O(N)
* Spcace complexity: O(1)
*/
var maxProfit = function(prices) {
    if (prices.length <= 1) return 0;
    let dp = new Array(5).fill(0);
    dp[1] = dp[3] = -prices[0];

    for (let i = 1; i < prices.length; i++) {
        dp[0] = dp[0];
        dp[1] = Math.max(dp[1], dp[0] - prices[i]);
        dp[2] = Math.max(dp[2], dp[1] + prices[i]);
        dp[3] = Math.max(dp[3], dp[2] - prices[i]);
        dp[4] = Math.max(dp[4], dp[3] + prices[i]);
    }
    return dp[4];
};
```


## 188 Best Time to Buy And Sell Stock IV
[Description on Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/description/)

> ### Intuition
> The solution is pretty similar to the previous one, the only difference is that we have to limit the times of selling within k times.
> If we say 0 means no operation, 1 means first time buy, 2 means first time sell, 3 means second time buy, 4 means second time sell...We found that if j is odd number, is means buying, while even numbers of j means selling the stock
> So we find that j is between [0, 2 * k + 1].
> Then for the state transition, we have:
> dp[i][j + 1] = max(dp[i - 1][j + 1], dp[i - 1][j] - prices[i]); and dp[i][j + 2] = max(dp[i - 1][j + 2], dp[i - 1][j + 1] + prices[i]);

### Approach
1. Dynamic Programming - dp[i][j] means the max value of cash with state j. j represents 5 possible state on ith day.

```
/**
 * @param {number} k
 * @param {number[]} prices
 * @return {number}
 */

/* 
* Dynamic Programming - dp[i][0] means on i-th day, the max cash of state j.
* Time complexity; O(N * k)
* Spcace complexity: O(N  * k)
*/
var maxProfit = function(k, prices) {
    let dp = new Array(prices.length).fill().map(() => Array(2 * k + 1).fill(0));

    // Initial state
    for (let j = 1; j <= 2 * k; j += 2) {
        dp[0][j] = -prices[0];
    }

    for (let i = 1; i < prices.length; i++) {
        for (let j = 0; j < 2 * k - 1; j += 2) {
            dp[i][j + 1] = Math.max(dp[i - 1][j + 1], dp[i - 1][j] - prices[i]);
            dp[i][j + 2] = Math.max(dp[i - 1][j + 2], dp[i - 1][j + 1] + prices[i]);
        }
    }
    return dp[prices.length - 1][2 * k];
};

/* 
* Dynamic Programming - Optimised space
* Time complexity; O(N * k)
* Spcace complexity: O(N)
*/
var maxProfit = function(k, prices) {
    let dp = Array(2 * k + 1).fill(0);

    // Initial state
    for (let j = 1; j <= 2 * k; j += 2) {
        dp[j] = -prices[0];
    }

    for (let i = 1; i < prices.length; i++) {
        for (let j = 1; j < 2 * k + 1; j++) {
            if (j % 2) {
                dp[j] = Math.max(dp[j], dp[j - 1] - prices[i]);
            } else {
                dp[j] = Math.max(dp[j], dp[j - 1] + prices[i]);
            }
        }
    }
    return dp[2 * k];
};
```
