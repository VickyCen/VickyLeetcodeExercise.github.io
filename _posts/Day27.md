# Day 27 - 455 Assign Cookies | 376 Wiggle Subsequence | 53 Maximum Subarray

## 455 Assign Cookies
[Description on Leetcode](https://leetcode.com/problems/assign-cookies/description/)

> ### Intuition
> With greedy algorithm, we can think about the solution in this way:
> If we sort both the kids and cookies from small to large.
> the large cookie can cater both large content kid or small content kid, because we want to maximise the amount of kids who can get the cookies, so when we have a large cookie, we should prioritise it to the large content kid.

### Approach
1. Sort s and g.
2. Loop into kids from the end of the array(we need to loop into kids instead of cookies), then loop into cookies and find a cookie[j] >= kids[i], and give it to kids[i].

```
/**
 * @param {number[]} g
 * @param {number[]} s
 * @return {number}
 */

/*
* Time complexity; O(NlogN)
* Space complexity: O(1)
*/
var findContentChildren = function(g, s) {
    s.sort();
    g.sort();
    let result = 0;
    let index = s.length - 1;
    for (let i = g.length - 1; i >= 0; i--) {
        if (index >= 0 && s[index] >= g[i]) {
            result++;
            index--;
        }
    }
    return result;
};
```


## 376 Wiggle Subsequence
[Description on Leetcode](https://leetcode.com/problems/wiggle-subsequence/description/)

> ### Intuition
> We can calculate the prediff(diff between current element and previous element), and curdiff(diff between current element and next element). If (prediff >= 0 && curdiff < 0 || prediff <= 0 && curdiff > 0), we know there's a wiggle, then we count the wiggles. But we need to handle a few things here:
> - Deal with the start and the end element with the count. We shouldn't miss to count them - we can set result = 1 in initialisation
> - Handle flat subsequence: consider [1, 2, 2, 2, 1], we beed to jump from 1 - 3. - we can make the check condition to include prediff = 0 case
> - Handle the monoton3： [1, 2, 2, 3] - we only update the prediff when we find the wiggle.

### Approach
1. Greedy: iterate into the array, let result = 0 as start. If (prediff >= 0 && curdiff < 0 || prediff <= 0 && curdiff > 0), there should be a wiggle, and we should count it.

```
/**
 * @param {number[]} nums
 * @return {number}
 */

/* 
* Time complexity: O(N)
* Space complexity: O(1) 
*/
var wiggleMaxLength = function(nums) {
    if (nums.length <= 1) return nums.length;
    let result = 1;
    let prediff = curdiff = 0;
    for (let i = 0; i < nums.length; i++) {
        curdiff = nums[i + 1] - nums[i];
        if (prediff >= 0 && curdiff < 0 || prediff <= 0 && curdiff > 0) {
            result++;
            prediff = curdiff;
        }
    }
    return result;
};
```

## 53 Maximum Subarray
[Description on Leetcode](https://leetcode.com/problems/maximum-subarray/description/)

> ### Intuition
> It's a typical greedy algorithm exercise. To get the maximum subarray sum, we need to ensure the the sum should not be < 0.
> But we need to be careful about a few things:
> - We need to be aware of the input can hava all of the negative number. So when comparing the sum, we need to start from -Infinity.
> - When we iterate and meet a negative number, it doesn't necessarily mean we should drop it. Consider [1, -3, 4, -1, 3, 2, 7], when we get 4 then -1, we shouldn't drop -1 and let the count start from 0, because at the moment the count is still positive number. If we drop -1 and start the count = 0, we'll miss the subarray [4, -1, 3, 2]. We only drop the number when the count is < 0. This is quite important.

### Approach
1. Greedy: start the result from -Infinity, and the count = 0. Loop into each number can calculate the sum. When count < 0, drop it and make count = 0 again. Record the maximum value between result and count into each loop

```
/**
 * @param {number[]} nums
 * @return {number}
 */

/* 
* Time complexity: O(N)
* Space complexity: O(1) 
*/
var maxSubArray = function(nums) {
    let result = -Infinity;
    let count = 0;
    for (let i = 0; i < nums.length; i++) {
        count += nums[i];
        if (count > result) {
            result = count;
        }
        if (count < 0) count = 0;
    }
    return result;
};
```
