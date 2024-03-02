# Day 38 - Karma 52 Weight Bag Problem - Complete | 518 Coin Change II | 377 Combination Sum IV

## Karma 52 Weight Bag Problem - Complete
[Description on Leetcode](https://kamacoder.com/problempage.php?pid=1052)

> ### Intuition
> This is also another type of bag weight problem, but unlike the previous one, for each type of item, it can have infinite number, so each type of items can be selected multiple times. Because of that, in the for-loop, we need to iterate from start to end, instead of end to start.
> The most important thing here is to understand the difference between this problem and the 01 bag weight problem. 01 bag weight problem only allows 1 type of item to be selected 1 time. But the complete bag weight problem allows 1 type of item to be selected multiple times / infinite times.
 
### Approach
1. Dynamic Programming - 1 dimension dp array, using dp[j] = Math.max(dp[j], dp[j - weight[i]] + value[i]), and iterate bag size first or items first are both okay for this type of bag weight problem.

```
const rl = require('readline').createInterface({ input: process.stdin });
var iter = rl[Symbol.asyncIterator]();
const readline = async () => (await iter.next()).value;

// Iterate items first, then iterate bag size
function test_completePack1(bagWeight, weight, value) {
    let dp = new Array(bagWeight + 1).fill(0);
    for (let i = 0; i < weight.length; i++) {
        for (let j = weight[i]; j <= bagWeight; j++) {
            dp[j] = Math.max(dp[j], dp[j - weight[i]] + value[i]);
        }
    }
    return dp[bagWeight];
}

// Iterate bag size first, then iterate items
function test_completePack2(bagWeight, weight, value) {
    let dp = new Array(bagWeight + 1).fill(0);
    for (let j = 0; j <= bagWeight; j++) {
        for (let i = 0; i < weight.length; i++) {
            if (j >= weight[i]) {
                dp[j] = Math.max(dp[j], dp[j - weight[i]] + value[i]);
            }
        }
    }
    return dp[bagWeight];
}

async function main(){
    let line = await readline();
    const num  = line.split(' ').map(Number);
    const bagWeight = num[1];
    let items = num[0];
    let weight = [];
    let value = [];
    for (let i = 0; i < items; i++) {
        line = await readline();
        let item = line.split(' ').map(Number);
        weight.push(item[0]);
        value.push(item[1]);
    }
    
    console.log(test_completePack2(bagWeight, weight, value));
}

main();
```


## 518 Coin Change II
[Description on Leetcode](https://leetcode.com/problems/coin-change-ii/description/)

> ### Intuition
> So the approach is quite similar to last exercise. We can define dp[j] to be the number combinations that can make up to the amount.
> However, we need to determine for the outer for-loop, what factors we should iterate, as well as for the inner for-loop.
> Because in this exercise, it requires to get the number of combinations, which means the sequence of the element into 1 combination is not a matter. For example, 5 = 2 + 2 + 1 and 5 = 2 + 1 + 2, where we think [2, 2, 1] and [2, 1, 2] are the same combination. Therefore, for outer for-loop we need to iterate the coins array, and for the inner for-loop, we need to iterate the bag size (amount in this case). To ensure dp[j] is calcaulting the number of combinations, not the number of sequences.

### Approach
1. Dynamic Programming - 1 dimension dp array, using dp[j] += dp[j - coins[i]].

```
/**
 * @param {number} amount
 * @param {number[]} coins
 * @return {number}
 */

/*
* Dynamic Programming - 1 dimension dp array
* Time Complexity: O(amount * N) where n is the length of coins array
* Space Complexity: O(amount)
*/
var change = function(amount, coins) {
    let dp = new Array(amount + 1).fill(0);
    dp[0] = 1;

    for (let i = 0; i < coins.length; i++) {
        for (let j = coins[i]; j <= amount; j++) {
            dp[j] += dp[j - coins[i]];
        }
    }
    return dp[amount];
};
```


## 377 Combination Sum IV
[Description on Leetcode](https://leetcode.com/problems/combination-sum-iv/description/)

> ### Intuition
> Note that **<em>"different sequences are counted as different combinations."</em>**, so it acutally requires us to calcualte the number of the sequeces, instead of the pure combinations. So the factors to be iterate in outer and inner for-loop should be different from last exercise. To ensure it's calculating the sequences, we need to iterate the bag weight (sum) in outer for-loop, and iterate the items (nums) in inner for-loop.

### Approach
1. Dynamic Programming - 1 dimension dp array, using dp[j] += dp[j - nums[i]].

```
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */

/*
* Dynamic Programming - 1 dimension dp array
* Time Complexity: O(target * N) where N is the length of the nums array
* Space Complexity: O(target)
*/
var combinationSum4 = function(nums, target) {
    let dp = new Array(target + 1).fill(0);
    dp[0] = 1;

    for (let j = 0; j <= target; j++) {
        for (let i = 0; i < nums.length; i++) {
            if (j >= nums[i]) {
                dp[j] += dp[j - nums[i]];
            }
        }
    }
    return dp[target];
};
```
