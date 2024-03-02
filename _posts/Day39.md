# Day 39 - Karma 57 Climb Stairs with M | 322 Coin Change | 279 Perfect Squares

## Karma 57 Climb Stairs with M
[Description on Leetcode](https://kamacoder.com/problempage.php?pid=1067)

> ### Intuition
> Stairs n is like the bag size, then the item is 1...m, and can choose multiple times, so this is complete bag weight problem again.
> Also be aware of that this is to culculate the number of sequences here. Because climbing 5 stairs in total, 2 + 2 + 1 and 2 + 1 + 2 is considered to be a different way. So we need to put the bag size (n) in the outer for-loop, then iterate items (m) in the inner for-loop.
 
### Approach
1. Dynamic Programming - 1 dimension dp array, using dp[i] += dp[i - j];

```
const rl = require('readline').createInterface({ input: process.stdin });
var iter = rl[Symbol.asyncIterator]();
const readline = async () => (await iter.next()).value;

/*
* Dynamic Programming - 1 dimension dp array
* Time Complexity: O(M * N)
* Space Complexity: O(N)
*/
function climbStairs(n, m) {
    let dp = new Array(n + 1).fill(0);
    dp[0] = 1;
    
    for (let i = 1; i <= n; i++) {
        for (let j = 1; j <= m; j++) {
            if (i >= j) {
                dp[i] += dp[i - j];
            }
        }
    }
    return dp[n];
}

async function main() {
    const line = await readline();
    const input  = line.split(' ').map(Number);
    const n = input[0];
    const m = input[1];
    
    console.log(climbStairs(n, m));
}

main();
```


## 322 Coin Change
[Description on Leetcode](https://leetcode.com/problems/coin-change/description/)

> ### Intuition
> This is a complete bag weight problem. We can define dp[j] to be the minimum number of coins required for making change amount of j. So dp[j] = Math.min(dp[j], dp[j - coins[i]] + 1), meaning we can make amount dp[j - coins[i]] then plus 1 coins[i] to make the amount of j. And because we are checking the minimum number instead of the total number of combinations or sequences, so the outer / inner for-loop can be either amount or coins array iteration.
> Also, because we are checing the minimum amount, so we should initialise the dp array to be Infinity.

### Approach
1. Dynamic Programming - 1 dimension dp array, using dp[j] = Math.min(dp[j], dp[j - coins[i]] + 1).

```
/**
 * @param {number[]} coins
 * @param {number} amount
 * @return {number}
 */

/*
* Dynamic Programming - 1 dimension dp array
* Time Complexity: O(amount * N) where n is the length of coins array
* Space Complexity: O(amount)
*/
var coinChange = function(coins, amount) {
    let dp = new Array(amount + 1).fill(Infinity);
    dp[0] = 0;

    for (let i = 0; i < coins.length; i++) {
        for (let j = coins[i]; j <= amount; j++) {
            dp[j] = Math.min(dp[j], dp[j - coins[i]] + 1);
        }
    }

    return dp[amount] === Infinity? -1 : dp[amount];
};
```


## 279 Perfect Squares
[Description on Leetcode](https://leetcode.com/problems/perfect-squares/description/)

> ### Intuition
> This is similar to last exercise. The n is the bag size, and for each i where i * i <= n, it's the item. So this exercise is the complete bag weight problem requiring to return the minimum amount of items being put in bag size n.

### Approach
1. Dynamic Programming - 1 dimension dp array, using dp[j] = Math.min(dp[j], dp[j - i * i] + 1).

```
/**
 * @param {number} n
 * @return {number}
 */

/*
* Dynamic Programming - 1 dimension dp array
* Time Complexity: O(n * √n)
* Space Complexity: O(n)
*/
var numSquares = function(n) {
    let dp = new Array(n + 1).fill(Infinity);
    dp[0] = 0;

    for (let j = 1; j <= n; j++) {
        for (let i = 0; i * i <= j; i++) {
            dp[j] = Math.min(dp[j], dp[j - i * i] + 1);
        }
    }
    return dp[n];
};
```
