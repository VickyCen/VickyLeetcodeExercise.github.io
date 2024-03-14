# Day 51 - 503 Next Greater Element II | 42 Trapping Rain Water

## 503 Next Greater Element II
[Description on Leetcode](https://leetcode.com/problems/next-greater-element-ii/description/)

> ### Intuition
> This is same as 739 Daily Temperatures, we need to use a monotonic stack and maintain that from stack top to bottom, the value of nums[i] is increasing. The only difference is that here is a circular array. How to deal with circular array?
> One straightforward approach is to add one more same nums array after the current nums array, and initialise the monotonic stack with size of 2 * nums.length. But this means more iteration operation and extra space for the stack.
> A better approach would be, utilise i % len to get the position of element in circular array - in the for-loop, we loop into nums.length * 2 with the terminate condition to be i < nums.length * 2, then use nums[i % len] to map the element's position as in 1 array within the circular array. Doing so, it doesn't need to use an extra spaced stack.

### Approach
1. Monotonic stack - using a stack to store the index, and maintain that from stack top to bottom, the value of nums[i] is increasing. Iterate the temperatures array, keep comparing the nums[i % len] with nums[stack.top()], if the stack can maintain that from stack top to bottom, the value of nums[i] is increasing, we push the index into stack; otherwise, we pop out the stack top and record the distance into result. Only until that nums[i] <= nums[stack.top()] again, we push the i into stack.

```
/**
 * @param {number[]} nums
 * @return {number[]}
 */

/* 
* Monotonic stack - using a stack to store the index, and maintain that from stack top to bottom, the value of nums[i] is increasing
* Time complexity - O(N)
* Space complexity - O(N)
*/
var nextGreaterElements = function(nums) {
    const len = nums.length;
    let result = new Array(len).fill(-1);
    let stack = [];

    for (let i = 0; i < nums.length * 2; i++) { // Loop into 2 * nums.length
        while (stack.length && nums[i % len] > nums[stack[stack.length - 1]]) {    // i % len will get the position of element in circular array
            const index = stack.pop();
            result[index] = nums[i % len];
        }
        stack.push(i % len);
    }
    return result;
};
```


## 42 Trapping Rain Water
[Description on Leetcode](https://leetcode.com/problems/trapping-rain-water/)

> ### Intuition
> Brute Force: if we check each bar individually - consider for each bar, the width of the rain (w) is 1, then we can calculate the height of the rain (h) for the current bar, then the volume of the rain = h * w. Then we can loop into all the bars and calculate all the sum of the volume (only sum the volume when h > 0). For each bar with index i, the h = Math.min(HighestBarOnLeft, HighestBarOnRight) - height[i].
> Please note that for the first bar and last bar, it can store any rain so we don't need to calculate the h for them.
> Monotonic stack: We maintain stack of storing index, where from stack top to bottom, the height[stack[i]]'s value is increasing:
> - If height[i] < height[stack[stack.length - 1]]: stack.push(i);
> - If height[i] === height[stack[stack.length - 1]]: stack.pop(); stack.push(i);   // pop out the old index, push the new index because the rain's height should always calculate based on the right bar
> - If height[i] > height[stack[stack.length - 1]]: meaning there's a valley appears. So height[i] should be the next right bar of the valley, and if we pop up the stack top, it become the valley bottom, let's call it mid and midIndex = stack.pop(); so mid = height[midIndex]. Now the next left bar of the mid would be height[stack[stack.length - 1]]. We can calculate the h = Math.min(height[stack[stack.length - 1]], height[i]) - height[midIndex]. Then w = i - stack[stack.length - 1] - 1. And we can calculate the volume of rain = h * w.

### Approach
1. Brute Force -  2 pointers - loop into each bar, for each bar, the width of rain (w) is 1, we need to calculate the height of the rain (h) such that the water volume = w * h while h = Math.min(HighestBarOnLeft, HighestBarOnRight) - height[i]. But it will be timed out, and we know we are repeatedly checking HighestBarOnLeft and HighestBarOnRight when iterate each bar, which can be optimised.
2. Optimised 2 pointers: Instead of repeatedly checking HighestBarOnLeft and HighestBarOnRight when iterate each bar. We can first loop into the height array, and store HighestBarOnLeft of bar i in array maxLeft as well as HighestBarOnRight of bar i in array maxRight. Here we elimiate the repeated traversal for left and right elements because we know maxLeft[i] = Math.max(maxLeft[i - 1], height[i]) and maxRight[i] = Math.max(maxRight[i + 1], height[i]). By doing so, we sacrifice space for time.
3. Monotonic stack: Calculate the h = Math.min(height[stack[stack.length - 1]], height[i]) - height[midIndex]. Then w = i - stack[stack.length - 1] - 1. And we can calculate the volume of rain = h * w.

```
/**
 * @param {number[]} height
 * @return {number}
 */

/* 
* Brute Force (timed out) - loop into each bar, for each bar, the width of rain (w) is 1, we need to calculate the height of the rain (h) such that the water volume = w * h
* find out the highest bar on the left and on the right. The height of rain for element i is Math.min(leftHighest, rightHighest) - height[i].  
* Time complexity - O(N ^ 2)
* Space complexity - O(1)
*/
var trap = function(height) {
    let sum = 0;
    const len = height.length;

    for (let i = 0; i < len; i++) {
        if (i === 0 || i === len - 1) continue;   // Skip for the first and last bar
        let lMaxHeight = height[i];
        let rMaxHeight = height[i];
        for (let j = i - 1; j >= 0; j--) {
            lMaxHeight = height[j] > lMaxHeight ? height[j] : lMaxHeight;   // Find the highest bar on the left
        }
        for (let j = i + 1; j < len; j++) {
            rMaxHeight = height[j] > rMaxHeight ? height[j] : rMaxHeight;   // Find the highest bar on the right
        }

        let h = Math.min(lMaxHeight, rMaxHeight) - height[i];
        if (h > 0) sum += h * 1;    // volume of rain = h * w where w is considered to be 1
    }
    return sum;
};

/* 
* Optimised Brute Force - instead of repeatedly find lMaxHeight & rMaxHeight when traverse each bar, 
* we can store the lMaxHeight in an array called maxLeftHeight where maxLeftHeight[i] = Math.max(maxLeftHeight[i - 1], height[i]).
* Similarly, store the rMaxHeight in an array called maxLeftHeight where maxRightHeight[i] = Math.max(maxRightHeight[i + 1], height[i]).
* Time complexity - O(N)
* Space complexity - O(N)
*/
var trap = function(height) {
    let sum = 0;
    const len = height.length;
    let maxLeftHeight = new Array(len).fill(0);
    let maxRightHeight = new Array(len).fill(0);
    maxLeftHeight[0] = height[0];
    maxRightHeight[len - 1] = height[len - 1];

    for (let i = 1; i < len; i++) {
        // Find the highest bar on the left of the i-th bar, loop from left to right
        maxLeftHeight[i] = Math.max(maxLeftHeight[i - 1], height[i]);
    }
    for (let i = len - 2; i >= 0; i--) {
        // Find the highest bar on the left of the i-th bar, loop from left to right
        maxRightHeight[i] = Math.max(maxRightHeight[i + 1], height[i]);
    }

    for (let i = 0; i < len; i++) {
        let h = Math.min(maxLeftHeight[i], maxRightHeight[i]) - height[i];
        if (h > 0) sum += h * 1;    // volume of rain = h * w where w is considered to be 1
    }
    return sum;
};

/* 
* Monotonic stack: Calculate the h = Math.min(height[stack[stack.length - 1]], height[i]) - height[midIndex]. 
* Then w = i - stack[stack.length - 1] - 1. And we can calculate the volume of rain = h * w.
* Time complexity - O(N)
* Space complexity - O(N)
*/
var trap = function(height) {
    let sum = 0;
    const len = height.length;
    let stack = [];
    stack.push(0);

    for (let i = 1; i < len; i++) {
        let top = stack[stack.length - 1];
        if (height[i] < height[top]) {
            // Case 1
            stack.push(i);
        } else if (height[i] === height[top]) {
            // Case 2 - pop out old index and push new index, because height always based on the right one
            stack.pop();
            stack.push(i);
        } else {
            while (stack.length && height[i] > height[stack[stack.length - 1]]) {
                const midIndex = stack.pop();
                const h = Math.min(height[stack[stack.length - 1]], height[i]) - height[midIndex];   // h = Math.min(leftBar, rightBar) - valley bottom
                const w = i - stack[stack.length - 1] - 1;
                if (h > 0) {
                    sum += h * w;
                }
            }
            stack.push(i);
        }
    }
    return sum;
};

// Simplified version
var trap = function(height) {
    let sum = 0;
    const len = height.length;
    let stack = [];
    stack.push(0);

    for (let i = 1; i < len; i++) {
        while (stack.length && height[i] > height[stack[stack.length - 1]]) {
            const midIndex = stack.pop();
            const h = Math.min(height[stack[stack.length - 1]], height[i]) - height[midIndex];   // h = Math.min(leftBar, rightBar) - valley bottom
            const w = i - stack[stack.length - 1] - 1;
            if (h > 0) {
                sum += h * w;
            }
        }
        stack.push(i);
    }
    return sum;
};
```
