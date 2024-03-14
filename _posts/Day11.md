# Day 10 - 239 Sliding Window Maximum | 347 Top k Frequent Elements

## 239 Sliding Window Maximum
[Description on Leetcode](https://leetcode.com/problems/sliding-window-maximum/description/)

> ### Intuition
> If using brute force it will cost O(N * K). We can consider to use a monotonic queue, to maintain the maximum value within the sliding window as the window moves. We don't need to care about what's the other value from the previous values, we only stores the maximum value in the monotonic queue. As the sliding window moves, we only push the values which is less than the previous value in the queue. If the value to be pushed is larger than the previous value in the queue, we pop out the values from the queue, till the queue maintains monotonically decrements.

### Approach
Using monotonic queue:
1. Create the monotonic queue and define it's push & shift.
2. Loop into the nums, using 2 pointers, 1 pointer j to iterate the nums. 1 pointer i pointing to the left element of the sliding window, to check whether we need to pop out the values from the monotonic queue as sliding window moves.

```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */

/* 
* Time Complexity - O(N)
* Space Complexity - O(k) - because we define a monotonic queue of k size
*/
class MyQueue {
    queue;
    constructor() {
        this.queue = [];
    }

    push(value) {
        let back = this.queue[this.queue.length - 1];

        while (back !== undefined && back < value) {
            this.queue.pop();
            back  = this.queue[this.queue.length - 1]
        }
        this.queue.push(value);
    }

    shift(value) {
        let front = this.front();
        if (front === value) {
            this.queue.shift();
        }
    }

    front() {
        return this.queue[0];
    }
}

var maxSlidingWindow = function(nums, k) {
    let helpQueue = new MyQueue();

    let i = 0, j = 0;
    const result = [];

    while (j < k) {
        helpQueue.push(nums[j++]);
    }
    result.push(helpQueue.front());

    while (j < nums.length) {
        helpQueue.push(nums[j++]);
        helpQueue.shift(nums[i++]);
        result.push(helpQueue.front());
    }
    return result;
};
```


## 347 Top k Frequent Elements
[Description on Leetcode](https://leetcode.com/problems/top-k-frequent-elements/)

> ### Intuition
> We can utilise the max-heap of k size, to maintain the **minimum** frequecy of elements. Whenever the heap size is larger than k, we pop out elements, so within the heap we always have largest frequency of elements so far. When implementing max-heap, be aware of the rules of swim and sink.

### Approach
1. Use hash table to calculate each element's frequency
2. Construct a max-heap, add frequency into the heap
3. Maintian the max-heap size of k

```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */

/* 
* Time Complexity - O(NLogk)
* Space Complexity - O(N) - Hash table stores element frequency
*/
class Heap {
    constructor(compareFn) {
        this.compareFn = compareFn;
        this.queue = [];
    }

    push(value) {
        this.queue.push(value);
        // Swim
        let index = this.size() - 1;
        let parentIndex = Math.floor((index - 1) / 2);

        while (parentIndex >= 0 && this.compare(parentIndex, index) > 0) {
            [this.queue[index], this.queue[parentIndex]] = [this.queue[parentIndex], this.queue[index]];

            index = parentIndex;
            parentIndex = Math.floor((index - 1) / 2);
        }
    }

    pop() {
        let top = this.queue[0];

        this.queue[0] = this.queue.pop();

        // Sink
        let index = 0;
        let left = 1; // left child index
        let searchChild = this.compare(left, left + 1) > 0 ? left + 1 : left;

        while (searchChild !== undefined && this.compare(index, searchChild) > 0) {
            [this.queue[index], this.queue[searchChild]] = [this.queue[searchChild], this.queue[index]];

            index = searchChild;
            left = index * 2 + 1;
            searchChild = this.compare(left, left + 1) > 0 ? left + 1 : left;
        }
        return top;
    }

    size() {
        return this.queue.length;
    }

    compare(index1, index2) {
        if (this.queue[index1] === undefined) return 1;
        if (this.queue[index2] === undefined) return -1;

        return this.compareFn(this.queue[index1], this.queue[index2]);
    }
}

var topKFrequent = function(nums, k) {
    const map = new Map();
    for (const num of nums) {
        map.set(num, (map.get(num) || 0) + 1);
    }

    const heap = new Heap((a, b) => a[1] - b[1]);

    for (const item of map.entries()) {
        heap.push(item);

        if (heap.size() > k) {
            heap.pop();
        }
    }

    const result = [];

    for (let i = heap.size() - 1; i >= 0; i--) {
        result.push(heap.pop()[0]);
    }

    return result;
};
```
