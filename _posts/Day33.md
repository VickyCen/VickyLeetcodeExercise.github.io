# Day 33 - 509 Fibonacci Number | 70 Climbing Stairs | 746 Min Cost Climbing Stairs

## 509 Fibonacci Number
[Description on Leetcode](https://leetcode.com/problems/fibonacci-number/description/)

> ### Intuition
> We know the initial state of dp array is dp[0] = 0, dp[1] = 1. And the state transition is dp[i] = dp[i - 1] + dp[i - 1].

### Approach
1. Dynamic Programming: based on dp[i] = dp[i - 1] + dp[i - 1]

```
/**
 * @param {number} n
 * @return {number}
 */

/* 
* Dynamic Programming - using dp array
* Time Complexity: O(N)
* Space Complexity: O(N)
*/
var fib = function(n) {
    let dp = new Array(n + 1);
    dp[0] = 0;
    dp[1] = 1;

    for (let i = 2; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    return dp[n];
};

/* 
* Dynamic Programming - do not use full array
* Time Complexity: O(N)
* Space Complexity: O(1)
*/
var fib = function(n) {
    if (n <= 1) return n;
    let dp = new Array(2);
    dp[0] = 0;
    dp[1] = 1;
    for (let i = 2; i <= n; i++) {
        const sum = dp[0] + dp[1];
        dp[0] = dp[1];
        dp[1] = sum;
    }
    return dp[1];
};

/* 
* Recursion
* Time Complexity: O(2 ^ N)
* Space Complexity: O(N)
*/
var fib = function(n) {
    if (n <= 1) return n;
    return fib(n - 1) + fib(n - 2);
};
```


## 70 Climbing Stairs
[Description on Leetcode](https://leetcode.com/problems/climbing-stairs/description/)

> ### Intuition
> We determine that dp[i] means climbing the i-th stair, there are dp[i] methods. So the state transition dp[i] = dp[i - 1] + dp[i - 2] because it means when climbing on the i-1th stair, we climb 1 more step, or climbing on the i-2th stair, we climb 2 stairs with 1 step.
> But we should understand that dp[0] equal to o or 1? Actually, that's meaningfulless to discuss dp[0] because the description mentions that n is positive integer. So n won't be able to equal to 0.

### Approach
1.  Dynamic Programming: based on dp[i] = dp[i - 1] + dp[i - 1].

```
/**
 * @param {number} n
 * @return {number}
 */

/* 
* Dynamic Programming - using dp array
* Time Complexity: O(N)
* Space Complexity: O(N)
*/
var climbStairs = function(n) {
    let dp = [null, 1, 2];
    for (let i = 3; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    return dp[n];
};

/* 
* Dynamic Programming - Not using dp array
* Time Complexity: O(N)
* Space Complexity: O(1)
*/
var climbStairs = function(n) {
    if (n <= 2) return n;
    let dp = new Array(3);
    dp[1] = 1;
    dp[2] = 2;
    for (let i = 3; i <= n; i++) {
        const sum = dp[1] + dp[2];
        dp[1] = dp[2];
        dp[2] = sum;
    }
    return dp[2];
};
```

## 746 Min Cost Climbing Stairs
[Description on Leetcode](https://leetcode.com/problems/min-cost-climbing-stairs/description/)

> ### Intuition
> According to the description, we can determine that dp[0] = dp[1] = 0. And the state transition dp[i] = Math.min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]).
>

### Approach
1.  Dynamic Programming: based on dp[i] = Math.min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]).

```
/**
 * @param {number[]} cost
 * @return {number}
 */

/* 
* Dynamic Programming - using dp array
* Time Complexity: O(N)
* Space Complexity: O(N)
*/
var minCostClimbingStairs = function(cost) {
    let dp = new Array(cost.length + 1);
    dp[0] = dp[1] = 0;
    
    for (let i = 2; i <= cost.length; i++) {
        dp[i] = Math.min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2])
    }
    return dp[cost.length];
};

/* 
* Dynamic Programming - Not using dp array
* Time Complexity: O(N)
* Space Complexity: O(1)
*/
var minCostClimbingStairs = function(cost) {
    let dp = new Array(2);
    dp[0] = dp[1] = 0;
    
    for (let i = 2; i <= cost.length; i++) {
        const dpi = Math.min(dp[1] + cost[i - 1], dp[0] + cost[i - 2]);
        dp[0] = dp[1];
        dp[1] = dpi;

    }
    return dp[1];
};
```
