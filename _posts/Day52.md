# Day 52 - 84 Largest Rectangle in Histogram

## 84 Largest Rectangle in Histogram
[Description on Leetcode](https://leetcode.com/problems/largest-rectangle-in-histogram/description/)

> ### Intuition
> This is very similar to trapping rain water, but details are different. the volume = w * h. For each bar, the h = height[i], we need to check its left side, and right side to find out the width, if left side / right side height > height[i], we can continue on expanding the width, otherwise the width should be stopped expanding.
> Brute Force: if we check each bar individually - consider for each bar, we keep expanding the width to left and right, until we found a bar's height < height[i], then stop, calculate the volume and get the maxium value.
> 2 Pointers: we first store for each bar, the index of the first bar on the left that has a lower height than the i-th bar (minLeftIndex), and he index of the first bar on the right that has a lower height than the i-th bar (minRightIndex). Then calculate the volume for each bar and return the largest one.
> Remember to initialise minLeftIndex[0] and minRightIndex[heights.length - 1] because for the first bar, it does not have any bar on the left while for the last bar, it doesn't have any bar on the right.
> Monotonic stack: We maintain stack of storing index, where from stack top to bottom, the height[stack[i]]'s value is decreasing, to ensure when we find a bar that has lower height, we should start calcualte the volume.
> - If height[i] > height[stack[stack.length - 1]]: stack.push(i);
> - If height[i] === height[stack[stack.length - 1]]: stack.pop(); stack.push(i);   // pop out the old index, push the new index because the bar's height should always calculate based on the right bar
> - If height[i] < height[stack[stack.length - 1]]: meaning we should stop expanding the width. So right = heights[i], midIndex = stack.pop(), h = heights[mid], left = stack.top(). w = right - left - 1; volume = w * h.
> - Note that using monotonic stack approach, we need to add 0 to heights array start and end: heights = [0, ...heights, 0] to ensure all the values in stack and be pop out for calculating the volume (this is also a bit different from trap rain water because for rain water, we don't need to onsider the first and last bar for the volume calculation).

### Approach
1. Brute Force (timed out) - Loop into each bar, check each bar's left and right bars until find a one that has lower height than height[i], w = right - left - 1; h = heights[i]; calculate the volume and return the largest one.
2. 2 pointers: We can first loop into the height array, and store minLeftIndex of bar i to indicate the first bar index that has lower height than height[i] as well as minRightIndex of bar i. Then for each bar, calculating the w = right - left - 1; h = heights[i] and return the largest volume.
3. Monotonic stack: When height[i] < height[stack[stack.length - 1]], calculate right = heights[i], midIndex = stack.pop(), h = heights[mid], left = stack.top(). w = right - left - 1; volume = w * h.

```
/**
 * @param {number[]} heights
 * @return {number}
 */

/* 
* Brute Force (timed out) - Keep checking each bar's left and right bars until find a one that has lower height than height[i], w = right - left - 1;
* h = heights[i]; calculate the volume and return the largest one.
* Time complexity - O(N ^ 2)
* Space complexity - O(1)
*/
var largestRectangleArea = function(heights) {
    let sum = 0;
    for (let i = 0; i < heights.length; i++) {
        let left = right = i;

        for (; left >= 0; left--) {
            if (heights[left] < heights[i]) break;
        }
        for (; right < heights.length; right++) {
            if (heights[right] < heights[i]) break;
        }

        const w = right - left - 1;
        sum = Math.max(sum, w * heights[i]);
    }
    return sum;
};

/* 
* 2 Pointers - store minLeftIndex of bar i to indicate the first bar index that has lower height than height[i] as well as minRightIndex of bar i.
* For each bar, calculating the w = right - left - 1; h = heights[i] and return the largest volume.
* Time complexity - O(N)
* Space complexity - O(N)
*/
var largestRectangleArea = function(heights) {
    const len = heights.length;
    let sum = 0;
    let minLeftIndex = new Array(len).fill(-1);
    let minRightIndex = new Array(len).fill(len);    // -1 means the index doesn't exist

    for (let i = 1; i < len; i++) {
        let t = i - 1;
        while (t >= 0 && heights[i] <= heights[t]) {  // Need to check if t is valid index
            t = minLeftIndex[t];
        }
        minLeftIndex[i] = t;
    }
    for (let i = len - 2; i >= 0; i--) {
        let t = i + 1;
        while (t < len && heights[i] <= heights[t]) {
            t = minRightIndex[t];
        }
        minRightIndex[i] = t;
    }
    for (let i = 0; i < len; i++) {
        const w = minRightIndex[i] - minLeftIndex[i] - 1;
        sum = Math.max(sum, w * heights[i]);
    }
    return sum;
};

/* 
* Monotonic stack - When height[i] < height[stack[stack.length - 1]], calculate right = heights[i], midIndex = stack.pop(), h = heights[mid], left = stack.top(). w = right - left - 1; volume = w * h.
* Time complexity - O(N)
* Space complexity - O(N)
*/
var largestRectangleArea = function(heights) {
    heights = [0, ...heights, 0];
    let sum = 0;
    let stack = [];

    for (let i = 0; i < heights.length; i++) {
        if (heights[i] > heights[stack[stack.length - 1]]) {
            stack.push(i);
        } else if (heights[i] === heights[stack[stack.length - 1]]) {
            stack.pop();
            stack.push(i);
        } else {
            while (stack.length && heights[i] < heights[stack[stack.length - 1]]) {
                const midIndex = stack.pop();
                const h = heights[midIndex];
                const w = i - stack[stack.length - 1] - 1;
                sum = Math.max(sum, w * h);
            }
            stack.push(i);
        }
    }

    return sum;
};

// Simiplied version
var largestRectangleArea = function(heights) {
    heights = [0, ...heights, 0];
    let sum = 0;
    let stack = [];

    for (let i = 0; i < heights.length; i++) {
        while (stack.length && heights[i] < heights[stack[stack.length - 1]]) {
            const midIndex = stack.pop();
            const h = heights[midIndex];
            const w = i - stack[stack.length - 1] - 1;
            sum = Math.max(sum, w * h);
        }
        stack.push(i);
    }

    return sum;
};
```
