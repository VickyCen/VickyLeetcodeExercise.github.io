# Day 36 - Kamacode 46 Weight Bag Problem | 416 Partition Equal Subset Sum

## Kamacode 46 Weight Bag Problem
[Description on Leetcode](https://kamacoder.com/problempage.php?pid=1046)

> ### Intuition
> Define dp[i][j]: dp[i][j] means selecting items from [0, i] for bag size j, the maximum value is dp[i][j].
> If weight[i] > j, means the item i has larger than the bag size j, we cannot put item into j. So dp[i][j] = dp[i - 1][j] in this case.
> Otherwise, dp[i][j] can be dereived from 2 ways:
> - Not select i, so dp[i - 1][j]
> - Select i, the maxmimum value in this case is dp[i - 1][j - weight[i]] + value[i].
> Then we compare the value between these cases and get the max value for each dp[i][j].
> Be aware of that, when j = 0, meaning the bag size is 0, dp[i][j] = 0 because we can't put any item into the bag.
> When i = 0, as long as bag size j >= weight[i], we can put item 0, and dp[i][j] = value[0] because we can only selecting item 0 at most.

### Approach
1. Dynamic Programming - 2 dimension dp array. dp[i][j] means selecting items from [0, i] for bag size j, the maximum value is dp[i][j].
2. Dynamic Programming - 1 dimension dp array. Because we can copy dp[i - 1][j] dp[i][j] then calcaulting the overal max value when bag size is j, selecting from item [0, i]. So dp[j] means for bag size j, the maximum value no matter how to select items.

```
const rl = require("readline").createInterface({ input: process.stdin });
var iter = rl[Symbol.asyncIterator]();
const readline = async () => (await iter.next()).value;

/*
* Dynamic Programming - 2 dimension dp array
* Time Complexity: O(M * N) where M is the bag size and N is the number items can select
* Space Complexity: O(M * N)
*/
// dp[i][j] means selecting items from [0, i] for bag size j, the maximum value is dp[i][j]
function testWeightBagProblem(size, weight, value) {
    const len = weight.length;
    let dp = Array(len).fill().map(() => Array(size + 1).fill(0));
    
    // Initialise dp
    for (let j = weight[0]; j <= size; j++) {
        dp[0][j] = value[0];
    }
    
    for (let i =  1; i < len; i++) {  // Iterate items, starting from 1
        for (j = 0; j <= size; j++) {   // Iterate bag size
            if (weight[i] > j) {
                dp[i][j] = dp[i - 1][j];   // if item i is too large (larger than bag size)
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);   // Selecting i or not select i, comparing the maximum value
            }
        }
    }
    return dp[len - 1][size];
}

/*
* Dynamic Programming - 1 dimension dp array
* Time Complexity: O(M * N) where M is the bag size and N is the number items can select
* Space Complexity: O(M)
*/
// dp[j] means for bag size j, the maximum value no matter how to select items.
function testWeightBagProblem(size, weight, value) {
    const len = weight.length;
    let dp = Array(size + 1).fill(0);   // Initialise dp
    
    for (let i =  1; i < len; i++) {  // Iterate items, starting from 0
        for (j = size; j >= weight[i]; j--) {   // Iterate bag size from largest size. Because we don't want to repeat the selection of item [0, i]
            dp[j] = Math.max(dp[j], dp[j - weight[i]] + value[i]);   // Selecting i or not select i, comparing the maximum value
        }
    }
    return dp[size];
}

async function main(){
    let line = await readline();
    const num  = line.split(' ').map(Number);
    const size = num[1];
    line = await readline();
    const weight = line.split(' ').map(Number);
    line = await readline();
    const value = line.split(' ').map(Number);
    
    console.log(testWeightBagProblem(size, weight, value));
}

main();
```


## 416 Partition Equal Subset Sum
[Description on Leetcode]([https://leetcode.com/problems/partition-equal-subset-sum/description/])

> ### Intuition
> So if there are equal subset sum, for each subset, the sum of the subset should be sum / 2. So we set our target = sum / 2.
> Based on the same weight bag problem approaches, think of: the bag size is sum / 2, and we are selecting from items (the elements in nums array). For each item, the weight and value of the item is same - nums[i].
> In that case, we focus on finding dp[tartget] === target, that means for a bag size of sum / 2, we find a way to selecting items from nums array, making it the maximum value (the sum of nums[i] selected in the bag) to be equal to sum / 2.

### Approach
1. Dynamic Programming - 1 dimension dp array. We check if we can find dp[tartget] === target where target = sum / 2.

```
/**
 * @param {number[]} nums
 * @return {boolean}
 */

/*
* Dynamic Programming - 1 dimension dp array
* Time Complexity: O(M * N) where M is the bag size and N is the number items can select
* Space Complexity: O(M)
*/
var canPartition = function(nums) {
    const sum = nums.reduce((a, b) => a + b);
    if (sum % 2 !== 0) return false;   // we know that the sum of each subsets won't be equal t0 sum / 2.
    const target = sum / 2;
    let dp = Array(target + 1).fill(0);   // Because the nums only contains positive number, we can initialise with 0 because Math.max can get the right positive value comparing to 0.
    // If the array contains both negative and positive numbers, then we need to initialise with -Infinity.
    for (let i = 0; i < nums.length; i++) {
        for (let j = target; j >= nums[i]; j--) {
            dp[j] = Math.max(dp[j], dp[j - nums[i]] + nums[i]);
        }
    }
    return dp[target] === target;
};
```
