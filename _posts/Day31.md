# Day 31 - 435 Non Overlapping Intervals | 763 Partition Labels | 56 Merge Intervals

## 435 Non Overlapping Intervals
[Description on Leetcode](https://leetcode.com/problems/non-overlapping-intervals/description/)

> ### Intuition
> When we see "overlapping range" is mentioned, we should think of sorting the input.
> Consider in this way: we sort the input with its right edge. If we find all the non-overlapping ranges, we add the count. The result = intervals.length - count.

### Approach
1. Greedy: sort the array with right edge
2. Find the non-overlapping ranges by using if (interval[0] >= end) and increase the count
3. Return intervals.length - count

```
/**
 * @param {number[][]} intervals
 * @return {number}
 */

/*
* Time complexity: O(NlogN) because of the sorting
* Space complexity: O(N) because of the worst case of sorting can be reversed order, then the largest space would be N, where average is logN
*/
var eraseOverlapIntervals = function(intervals) {
    intervals.sort((a, b) => a[1] - b[1]);

    let count = 1;    // The count should start from 1
    let end = intervals[0][1];
    for (let i = 1; i < intervals.length; i++) {
        const interval = intervals[i];
        if (interval[0] >= end) {    // non-overalapping
            count++;
            end = interval[1];
        } 
    }
    return intervals.length - count;
};
```


## 763 Partition Labels
[Description on Leetcode](https://leetcode.com/problems/partition-labels/description/)

> ### Intuition
> Because it's asking the appearance of letters, so we need to calculate the appearance of each letter.
> The hardest thing is to find the partition line. But if we draw the picture, we can know that the partition line is actually the largest index that larger than previous letter.

### Approach
1. Iterate each letter within s, and store each letter's maximum index of appearance
2. Loop into s again, and if it meets the largest index that larger than previous letter's index, then push it into result.

```
/**
 * @param {string} s
 * @return {number[]}
 */

/*
* Time complexity: O(N)
* Space complexity: O(1)
*/
var partitionLabels = function(s) {
    let map = new Map();
    // Check the maximum index of appearance for each letter
    for (let i = 0; i < s.length; i++) {
        map.set(s[i], i);
    }

    let result = [];
    let left = right = 0;
    for (let i = 0; i < s.length; i++) {
        right = Math.max(map.get(s[i]), right);
        if (i === right) {
            result.push(right - left + 1);
            left = i + 1;
        }
    }
    return result;
};
```

## 56 Merge Intervals
[Description on Leetcode](https://leetcode.com/problems/merge-intervals/description/)

> ### Intuition
> It's very similar to exercise 435, the only difference is that how do we deal with the overalpping part

### Approach
1. Greedy: sort the array with right edge
2. Find the non-overlapping ranges by using if (cur[0] > prev[1]). If non-overlapping is found, that means previous' merge is complete and can be pushed into result; Otherwise, keep updating the right edge
3. Remember to push the last element

```
/**
 * @param {number[][]} intervals
 * @return {number[][]}
 */

/*
* Time complexity: O(NlogN) because of the sorting
* Space complexity: O(N) because of the worst case of sorting can be reversed order, then the largest space would be N, where average is logN
*/
var merge = function(intervals) {
    intervals.sort((a, b) => a[0] - b[0]);

    // Checking whether previous right edge is less than current's left edge
    // If so, these ranges are overlapping then we can merge them
    let result = [];
    let prev = intervals[0];
    for (let i = 1; i < intervals.length; i++) {
        const cur = intervals[i];
        if (cur[0] > prev[1]) {
            // Not overlapping, than meaning the previous merge is complete and can push into result
            result.push(prev);
            prev = cur;
        } else {
            prev[1] = Math.max(cur[1], prev[1]);
        }
    }   
    reult.push(prev);   // Remember to push the last one
    return result;
};
```
