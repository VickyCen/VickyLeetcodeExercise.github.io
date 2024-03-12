# Day 50 - 739 Daily Temperatures | 496 Next Greater Element I

## 739 Daily Temperatures
[Description on Leetcode](https://leetcode.com/problems/daily-temperatures/description/)

> ### Intuition
> Using monotonic stack, we use the stack to record the index of elements that we've iterated, and keep maintaining the stack to be that from stack top to bottom, the value of temperature[i] is increasing. This can ensure that, whenever we found an element that larger than previously iterated elements, and became the first larger elements on its right hand side, we record the distance into result array.

### Approach
1. Monotonic stack - using a stack to store the index, and maintain that from stack top to bottom, the value of temperature[i] is increasing. Iterate the temperatures array, keep comparing the temperatures[i] with temperatures[stack.top()], if the stack can maintain that from stack top to bottom, the value of temperature[i] is increasing, we push the index into stack; otherwise, we pop out the stack top and record the distance into result. Only until that temperatures[i] <= temperatures[stack.top()] again, we push the i into stack.

```
/**
 * @param {number[]} temperatures
 * @return {number[]}
 */

/* 
* Monotonic stack - using a stack to store the index, and maintain that from stack top to bottom, the value of temperature[i] is increasing
* Time complexity - O(N)
* Space complexity - O(N)
*/
var dailyTemperatures = function(temperatures) {
    const len = temperatures.length;
    if (len <= 0) return [];
    let result = new Array(len).fill(0);
    let stack = [];
    stack.push(0);

    for (let i = 1; i < len; i++) {
        let top = stack[stack.length - 1];
        if (temperatures[i] <= temperatures[top]) {
            stack.push(i);    // If the stack can maintain that from stack top to bottom, the value of temperature[i] is increasing, we push the index into stack
        } else {
            while (temperatures[i] > temperatures[stack[stack.length - 1]] && stack.length) {
                top = stack.pop();
                result[top] = i - top;   // Otherwise, we pop out the stack top and record the distance in result
            }
            stack.push(i);
        }
    }
    return result;
};
```


## 496 Next Greater Element I
[Description on Leetcode](https://leetcode.com/problems/next-greater-element-i/description/)

> ### Intuition
> Because it requires to find an element in nums2 such that it becomes the next greater element of nums1[i], so we need to initialise the monotonic stack with nums1's length, with initial value of -1.
> And we can utilise a map to record the value and index map of nums1: {key: nums1[i], value: i}, which can help us quickly find out whether this value exists in nums1, and if it does exist, the corresponding index. 
> Same as last excise, we should iterate elements in nums2 and maintain from the stack top to the bottom, the corresponding i of nums2[i], the value of nums[i] should be increasing. And if we find nums2[i] > stack[stack.length - 1] and nums[stack[stack.length - 1]] exists in nums1, meaning that we find the next greated element and we can record the result.

### Approach
1. Monotonic stack - using a stack to store the index, and maintain that from stack top to bottom, the value of temperature[i] is increasing. 

```
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */

/* 
* Monotonic stack - using a stack to store the index, and maintain that from stack top to bottom, the value of temperature[i] is increasing. 
* Time complexity - O(M + N)
* Space complexity - O(N) where N is nums1's length
*/
var nextGreaterElement = function(nums1, nums2) {
    let map = new Map();
    for (let i = 0; i < nums1.length; i++) {
        map.set(nums1[i], i);
    }

    let result = new Array(nums1.length).fill(-1);
    let stack = [];
    
    for (let j = 0; j < nums2.length; j++) {
        while (stack.length && nums2[j] > nums2[stack[stack.length - 1]]) {
            const indexInNums2 = stack.pop();
            const value = nums2[indexInNums2];
            if (map.has(value)) {
                // value exists in nums1
                const indexInNums1 = map.get(value);
                result[indexInNums1] = nums2[j];
            }
        }
        stack.push(j);
    }
    return result;
};
```
