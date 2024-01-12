# Day 2 - 977 Squares of A Sorted Array | 209 Minimum Size Subarray Sum | 59 Spiral Matrix II

## 977. Squares of A Sorted Array
[Description on Leetcode](https://leetcode.com/problems/squares-of-a-sorted-array/)

> ### Intuition
> The description mentions "<em>sorted in non-decreasing order</em>", which gives some hints on using 2 pointers

### Approach
1. Brute Force:
- Calculate each element's squares
- Sorted the result

2. Two pointers:
- Sorted array - means the maximum squares of the elements is either on the left or the right
- Left pointer points to the left square, Right pointer points to the right square then compare
- Create a result array (legnth === n), place the maximum elements from the right

```
/**
 * @param {number[]} nums
 * @return {number[]}
 */

/**
* Brute force
* Time complexity：O(n + nlogn)
* Space complexity：O(1)
**/
var sortedSquares = function(nums) {
    for (let i = 0; i < nums.length; i++) {
        nums[i] = nums[i] * nums[i];
    }

    nums.sort((a, b) => a - b);
    return nums;
};

/**
* Two Pointers
* Time complexity：O(N)
* Space complexity：O(N)
**/
var sortedSquares = function(nums) {
    let len = nums.length;
    let result = new Array(len).fill(0);

    let left = 0, index = right = len - 1;
    while (left <= right) {
        if (nums[left] * nums[left] < nums[right] * nums[right]) {
            result[index--] = nums[right] * nums[right];
            right--;
        } else {
            result[index--] = nums[left] * nums[left];
            left++;
        }
    }
    return result;
};
```


## 209 Minimum Size Subarray Sum
[Description on Leetcode](https://leetcode.com/problems/minimum-size-subarray-sum/)

> ### Intuition
> - Brute Force requires 2 For loops. Using sliding window can reduce the solution to only 1 For loop.
> - 2 pointers used in sliding window solution; 1 pointer for "expanding" the window, the other one for "shrinking";
> - When using sliding window, we need to know what's inside the window, when to "expand" the window and when to "shrink" the window

### Approach
1. Brute Force
- Use 2 For loops: 1 to iterate all the element, an internal loop to iterate each element and get the sum till sum >= target

2. Sliding window: 1 pointer to interate elements, the other pointer to update new array's index
- What's inside the window: the sum of the elements within the window
- When to expand the window: when the sum of elements < target and iterate each element
- When to shrink the window: when the sum of elements >= target

3. Prefix sum: 
- For each i-th element, calculate the sum of 0th to i-1-th elements before it; Store the sums in a new array
- For each i, using binary search to find out a boudary when sum[bound] = sum[i - 1] + target
- Return the minimum value of bound - i + 1

```
/**
 * @param {number[]} nums
 * @param {number} val
 * @return {number}
 */

/**
* Brute Force
* Time complexity：O(N^2)
* Space complexity：O(1)
**/
var minSubArrayLen = function(target, nums) {
    let subLength = Infinity;
    for (let i = 0; i < nums.length; i++) {
        let sum = 0;
        for (let j = i; j < nums.length; j++) {
            sum += nums[j];
            if (sum >= target) {
                subLength = Math.min(subLength, j - i + 1);
                break;
            }
        }
    }
    return subLength === Infinity ? 0 : subLength;
};

/**
* 2 pointers - left pointer to shrink the window when sum >= target, right pointer to expand the window when sum < target and iterate the elements
* Time complexity：O(N)
* Space complexity：O(1)
**/
var minSubArrayLen = function(target, nums) {
    let subLength = Infinity;
    let left = right = 0;
    let sum = 0;

    for (; right < nums.length; right++) {
        sum += nums[right];
        while(sum >= target) {
            subLength = Math.min(subLength, right - left + 1);
            sum -= nums[left++];
        }
    }
    return subLength === Infinity ? 0 : subLength;
};

/**
* Prefix sum - For each i-th element, calculate the sum of elements before it; then binary search a boundary for sum[bound] - sum[i-1] >= target then return the minimum sublength of k - j + 1
* Time complexity：O(N * logN)
* Space complexity：O(1)
**/
var binarySearchIndex = function(target, array) {
    let left = 0, right = array.length;

    while (left < right) {
        let mid = left + ((right - left) >> 1);
        if (target > array[mid]) {
            left = mid + 1;
        } else if (target < array[mid]) {
            right = mid;
        } else {
            return mid;
        }
    }
    return right === array.length ? -1 : right;
}

var minSubArrayLen = function(target, nums) {
    let len = nums.length;
    if (nums.length === 0) return 0;

    let subLength = Infinity;
    let sums = new Array(len + 1).fill(0);
    for (let i = 1; i <= len; i++) {
        sums[i] = sums[i - 1] + nums[i - 1];
    }

    for (let i = 1; i <= len; i++) {     // Starting from 1 because i-1 will not out of memory range
        let checkSum = target + sums[i - 1];     

        let boundary = binarySearchIndex(checkSum, sums);

        // If boudary < 0, it means there's no such boundary makes sums[boundary] - sums[i - 1]
        if (boundary >= 0 ) {
            subLength = Math.min(subLength, boundary - i + 1); 
        }
    }
    return subLength === Infinity ? 0 : subLength; 
};
```



## 59 Spiral Matrix II
[Description on Leetcode](https://leetcode.com/problems/spiral-matrix-ii/description/)

> ### Intuition
> No special solution to consider. We should simulate the steps of the description
> When doing the simulation, we should make sure each edge of the matrix should use the same boudary. For example, [start, end) for all 4 edge in a round

### Approach
1. Simulation
- Setting the start point (0, 0), offset = 1(in the first round, we don't fill the last element of an edge because it's the starting element of next edge), calculate how many rounds required
- Following with [start, end) to fill the matrix and complete all the rounds
- If n is an odd number, we need to fill in the central unit after the rounds
  
```
/**
 * @param {number} n
 * @return {number[][]}
 */

/**
* Simulation
* Time complexity：O(N^2)
* Space complexity：O(1)
**/
var generateMatrix = function(n) {
    let matrix = new Array(n).fill(0).map(() => new Array(n).fill(0));
    let startX = startY = 0;
    let offset = 1;
    let mid = round = Math.floor(n / 2);
    let count = 1;

    while(round--) {
        let row = startY;
        let col = startX;
        for (; col < n - offset; col++) {
            matrix[row][col] = count++;    // Fill the top edge
        }
        for (; row < n - offset; row++) {
            matrix[row][col] = count++;    // Fill the right edge
        }
        for (; col > startX; col--) {
            matrix[row][col] = count++;    // Fill the bottom edge
        }
        for (; row > startY; row--) {
            matrix[row][col] = count++;    // Fill the left edge
        }

        startX++;     
        startY++;       // After each round, the start point should move to the next unit of the diagonal
        offset++;
    }

    if (n % 2 === 1) {
        matrix[mid][mid] = count;    // If n is an odd number, need to fill in the central unit individually
    }

    return matrix;
};
```
