# Day 30 - 860 Lemonade Change | 406 Queue Reconstruction by Height | 452 Minimum Number of Arrows to Burst Balloons

## 860 Lemonade Change
[Description on Leetcode](https://leetcode.com/problems/lemonade-change/description/)

> ### Intuition
> Consider the scenarios:
> - If bill === 5, we increase 1 of $5
> - If bill === 10, we increase 1 of $10, but descrease 1 of $5 as change
> - If bill === 20, we can change with 1 of $10 + 1 of $5, or 3 of $5. But here we prefer change with 1 of $10 + 1 of $5, because $5 can be changed for bill $10 and $20. We keep as many $5 as possible, it will help us to successfully change all bills.

### Approach
1. Greedy: Based on the scenarios and analysis, if the count $5 or $10 is not enough for change, then return false.

```
/**
 * @param {number[]} bills
 * @return {boolean}
 */

/* 
* Time complexity; O(N)
* Spcace complexity: O(1)
*/
var lemonadeChange = function(bills) {
    let fiveCount = tenCount = 0;
    for (const bill of bills) {
        if (bill === 5) {
            fiveCount++;
        } else if (bill === 10) {
            if (fiveCount > 0) {
                // Can change
                tenCount++;
                fiveCount--;
            } else return false;
        } else {
            if (fiveCount > 0 && tenCount > 0) {
                // Prefer change with ten and five
                tenCount--;
                fiveCount--;
            } else if (fiveCount >= 3) {
                fiveCount -= 3;
            } else return false;
        }
    }
    return true;
};
```


## 406 Queue Reconstruction by Height
[Description on Leetcode](https://leetcode.com/problems/queue-reconstruction-by-height/description/)

> ### Intuition
> Here we need to deal with 2 things: the heights and the position
> Remember when we did the exercie "candy" last day, we acknowledged that we need to deal with 1 thing each time, and deal with all the things one by one. Here we should use the same approach. We should first determine the queue is sorted with descending heights (h). Then we deal with the k.

### Approach
1. Sort the queue with descending heights. This ensures that people who queue before you is >= your height.
2. Then we loop into each element (person), to check the element k. We can directly insert the person into the result array at index k. Because we've sorted the queue before, now we can make sure all people queued before is >= me, and there are only k people who has larger height queueing before me.

```
/**
 * @param {number[][]} people
 * @return {number[][]}
 */

/* 
* Time complexity; O(NLogN + N ^ 2)
* Spcace complexity: O(N)
*/
var reconstructQueue = function(people) {
    let result = [];
    // Sort the queue by h first, descending
    people.sort((a, b) => {
        if (a[0] !== b[0]) return b[0] - a[0];   // if a, b's height is not the same, larger height will queue first
        else {
            return a[1] - b[1];  // if a, b's height is the same, smaller position will queue first
        }
    });

    for (const person of people) {
        // Based on k, we directly insert person at index k of result
        result.splice(person[1], 0, person);
    }
    return result;
};
```

## 452 Minimum Number of Arrows to Burst Balloons
[Description on Leetcode](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/description/)

> ### Intuition
> If ballon A (start1, end1) and allon B (start2, end2) has overlapping area, then we can use 1 arrow to burst them. The strategy here is to sort the balloons and find the balloons with overalapping areas as much as possible. If the neighborhood balloons don't have overlapping areas of their start - end, that means we have to use 2 arrows to burst them.

### Approach
1. Greedy: Sort the ballons by start.
2. if the neighbor balloons don't have overlaps, the number of arrows increase 1.
3. Otherwise, we update the boundary of end for those neighbor balloons and keep comparing next balloon's [start, end].

```
/**
 * @param {number[][]} points
 * @return {number}
 */

/* 
* Time complexity; O(N)
* Spcace complexity: O(1)
*/
var findMinArrowShots = function(points) {
    points.sort((a, b) => a[0] - b[0]);
    let result = 1;     // result should start with one because the for loop below starts from points[1]
    for (let i = 1; i < points.length; i++) {
        if (points[i][0] > points[i - 1][1]) {  // Compare the balloon's start with previous balloon's end, to check if they are overlapping
            result++;
        } else {
            points[i][1] = Math.min(points[i][1], points[i - 1][1]); // Update the end boundary for the comparison of next balloon.
        }
    }
    return result;
};
```
