# Day 9 - 232 Implement Queue Using Stacks | 225 Implement Stack Using Queues

## 232 Implement Queue Using Stacks
[Description on Leetcode](https://leetcode.com/problems/implement-queue-using-stacks/description/)

> ### Intuition
> Queue is FIFO, but stack is LIFO. We cannot only use 1 stack to implement a queue because they serve different element process sequence. We can consider to use 2 stacks, 1 serve as element in, the other one serves as element out, so that to implement FIFO.

### Approach
Using 2 stacks:
1. StackIn for takng elements pushed in; stackOut for loading elements and pop out the first-in element.
2. When popping out the first elemenet, we need to utilise the stackOut - If stackOut is empty, moving elements from stackIn to stackOut, so that the first-in element in stackIn will now become the last-in element in stackOut. When popping out element from stackOut, it refers to the "first-in" element of the queue.

```
/*
* Time complexity：pop & peek: O(N); others is O(1)
* Space complexity：O(N)
*/
var MyQueue = function() {
    this.stackIn = [];
    this.stackOut = [];
};

/** 
 * @param {number} x
 * @return {void}
 */
MyQueue.prototype.push = function(x) {
    this.stackIn.push(x);
};

/**
 * @return {number}
 */
MyQueue.prototype.pop = function() {
    if (!this.stackOut.length) {
        while (this.stackIn.length) {
            this.stackOut.push(this.stackIn.pop());
        }
    }
    return this.stackOut.pop();
};

/**
 * @return {number}
 */
MyQueue.prototype.peek = function() {
    let top = this.pop();
    this.stackOut.push(top); 
    return top;
};

/**
 * @return {boolean}
 */
MyQueue.prototype.empty = function() {
    return !this.stackIn.length && !this.stackOut.length;
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * var obj = new MyQueue()
 * obj.push(x)
 * var param_2 = obj.pop()
 * var param_3 = obj.peek()
 * var param_4 = obj.empty()
 */
```


## 225 Implement Stack Using Queues
[Description on Leetcode](https://leetcode.com/problems/implement-stack-using-queues/description/)

> ### Intuition
> Unlike last exercise, we cannot perform the job here if we use 1 queue for push in, and the other queue for pop out.
> Instead, we can use 1 queue for push in, the other queue for storing the rest of elements when we need to pop out an element. But if we think about it further, we can know that we don't necessarily need 2 queues. We can actually use only 1 queue for both push and storing the rest of elements. - we can directly push back those elements (the elements before the last element in the queue) to the tail of the queue.

### Approach
Use 1 queue:
1. Initialise a queue
2. For pop, we pop out the elements before the last elements from queue, and push them back at the end of the queue again.
3. Once the pointer points to the "last-in" elements, we pop it out - which is the "first" element of the stack.

```
/*
* Time complexity：pop & top: O(N); others is O(1)
* Space complexity：O(N)
*/
var MyStack = function() {
    this.queue = [];
};

/** 
 * @param {number} x
 * @return {void}
 */
MyStack.prototype.push = function(x) {
    this.queue.push(x);
};

/**
 * @return {number}
 */
MyStack.prototype.pop = function() {
    let size = this.queue.length;
    size--;
    while (size--) {
        let front = this.queue.shift();
        this.queue.push(front);
    }
    return this.queue.shift();
};

/**
 * @return {number}
 */
MyStack.prototype.top = function() {
    const top = this.pop();
    this.queue.push(top);
    return top;
};

/**
 * @return {boolean}
 */
MyStack.prototype.empty = function() {
    return !this.queue.length;
};

/**
 * Your MyStack object will be instantiated and called as such:
 * var obj = new MyStack()
 * obj.push(x)
 * var param_2 = obj.pop()
 * var param_3 = obj.top()
 * var param_4 = obj.empty()
 */
```
