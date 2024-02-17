# Day 29 - 1005 Maximize Sum of Array after K Negations | 134 Gas Station | 135 Candy

## 1005 Maximize Sum of Array after K Negations 
[Description on Leetcode](https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/description/)

> ### Intuition
> An intuitive approach is that we should negate the negative numbers as much as possible. After all numbers become positive numbers, if k still > 0, we keep negate minimal numbers till k exhausts.

### Approach
1. Greedy: sort the nums array by their absolute values, descending
2. From left to right if there are any negative numbers, negate them until k === 0
3. If k still > 0, we keep negating the minimal numbers (because all the numbers are now positive and make the minimal number to be negative can maximise the sum) until k is exhausts.

```
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */

/* 
* Time complexity; O(N)
* Spcace complexity: O(1)
*/
var largestSumAfterKNegations = function(nums, k) {
    nums.sort((a, b) => Math.abs(b) - Math.abs(a));

    for (let i = 0; i < nums.length; i++) {
        if (nums[i] < 0 && k > 0) {
            nums[i] = -nums[i];
            k--;
        }
    }

    while (k) {
        nums[nums.length - 1] = -nums[nums.length - 1];
        k--;
    }
    return nums.reduce((acc, cur) => acc + cur);
};
```


## 134 Gas Station
[Description on Leetcode](https://leetcode.com/problems/gas-station/description/)

> ### Intuition
> What we actually care is the cover range (largest distance it can take on index i). If the cover range eventually >= nums.length - 1, meaning it can reach the end of the array. So our goal is to loop into the array and get the largest cover range for the i-th element.

### Approach
1. Brute force: loop into gas, for each i, calculate the rest[i] = gas[i] - cost[i] and see if the sum of rest > 0.
2. Greedy 1: loop into gas, and calculate sum of gas and the sum of cost, if sum(gas) > sum(cost) then return -1, because the curcuit can't be made. Then from 0 to gas.length - 1, calculate the sum of rest, if sum(rest) >= 0, then return 0, because the circuit can be made if the car starts from point 0. Otherwise, we iterate from gas.length - 1 to 0, and calculate the rest[i] again and sum rest. If sum(rest) from end >= sum(rest) from 0, then the current i is the start point.
3. Greedy 2: We calculate the curSum of rest, if curSum < 0, then means the start point should not be in [0, i], we should start calculate the curSum again from i+1.

```
/**
 * @param {number[]} gas
 * @param {number[]} cost
 * @return {number}
 */

/* Brute force
* Time complexity; O(N ^ 2)
* Spcace complexity: O(1)
*/
var canCompleteCircuit = function(gas, cost) {
    for (let i = 0; i < gas.length; i++) {
        let rest = gas[i] - cost[i];
        let index = (i + 1) % gas.length;
        while (rest > 0 && index !== i) {
            rest += gas[i] - cost[i];
            index = (i + 1) % gas.length;
        }
        if (rest >= 0) return i;
    }
    return -1;
};

/* Greedy 1
* Time complexity; O(N)
* Spcace complexity: O(1)
*/
var canCompleteCircuit = function(gas, cost) {
    let curSum = 0;
    let min = Infinity;   // min is the car's minimum oil capacity from start point 0
    for (let i = 0; i < gas.length; i++) {
        let rest = gas[i] - cost[i];
        curSum += rest;
        if (curSum < min) {
            min = curSum;
        }
    }
    if (curSum < 0) return -1;    // Can't make the circuit
    if (min >= 0) return 0;    // Meaning start point as 0 can make the curcuit

    for (let i = gas.length - 1; i >= 0; i--) {
        let rest = gas[i] - cost[i];
        min += rest;

        if (min >= 0) return i;
    }
    return -1;
}; 

/* Greedy 2: calculate curSum, if curSum < 0, then start point at least should be i + 1
* Time complexity; O(N)
* Spcace complexity: O(1)
*/
var canCompleteCircuit = function(gas, cost) {
    let curSum = 0;
    let start = 0;
    let totalSum = 0;
    for (let i = 0; i < gas.length; i++) {
        let rest = gas[i] - cost[i];
        curSum += rest;
        totalSum += rest;
        if (curSum < 0) {
            curSum = 0;
            start = i + 1;
        }
    }
    return totalSum < 0 ? -1 : start;
}; 
```

## 135 Candy
[Description on Leetcode](https://leetcode.com/problems/candy/description/)

> ### Intuition
> We can use greedy algorithm. The main thing we should know is, we should first iterate elements from **start to end**, to compare rating[i - 1] and rating[i]. If rating[i - 1] < rating[i], then candy[i] = candy[i] + 1;
> Then we have to iterate elements from **end to start**, to deal with rating[i] > rating[i = 1]. Why we need to iterate from end to start the second time? Because, for example when we decide candy[4], we compare rating[4] and rating[5], but we also need to depend on candy[5] and candy[6] because *Children with a higher rating get more candies than their neighbors* as mentioned in the description.
> Also, remember, when initialise candy array, the initial value should be 1 instead of 0. Because 1 kid will have at least 1 candy.

### Approach
1. Greedy: Initilaise the candy array with fill in 1
2. Iterate the rating from left to right, if (rating[i - 1] < rating[i]) candy[i] = candy[i - 1] + 1;
3. Iterate the rating from right to left, if (rating[i] > rating[i + 1]) candy[i] = Math.max(candy[i], candy[i + 1] + 1); Use Math.max here to ensure *Children with a higher rating get more candies than their neighbors*!
4. Calculate the sum of candies.

```
/**
 * @param {number[]} ratings
 * @return {number}
 */

/* 
* Time complexity; O(N)
* Spcace complexity: O(N)
*/
var candy = function(ratings) {
    let candyResult = new Array(ratings.length).fill(1);

    for (let i = 1; i < ratings.length; i++) {
        if (ratings[i] > ratings[i - 1]) candyResult[i] = candyResult[i - 1] + 1;
    }

     for (let j = ratings.length - 2; j >= 0; j--) {
        if (ratings[j] > ratings[j + 1]) candyResult[j] = Math.max(candyResult[j], candyResult[j + 1] + 1);
    }
    return candyResult.reduce((acc, cur) => acc + cur);
};
```
