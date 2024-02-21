# Day 32 - 738 Monotone Increasing Digits | 968 Binary Tree Cameras

## 738 Monotone Increasing Digits
[Description on Leetcode](https://leetcode.com/problems/monotone-increasing-digits/description/)

> ### Intuition
> Think this way, we check the digit from right to left, if we find nums[i - 1] > nums[i], then we make nums[i - 1]--, and make nums[i] and all other digit on the right to be 9. Then we will get the largest number.

### Approach
1. Brute force - will time out
2. Greedy: treat the number to be an array, check the digit from right to left, and if (nums[i - 1] > nums[i]) make nums[i - 1]-- and make all other digit on the right to be 9

```
/**
 * @param {number} n
 * @return {number}
 */

/* Brute force 
* Time complexity: O(N * M) where M is the length of num
* Space complexity: O(1) 
*/
const checkNum = (num) => {
    let max = Infinity;
    while (num) {
        let digit = num % 10;
        if (max >= digit) {
            max = digit;
            num = parseInt(num / 10);
        } else {
            // If the number is not monotone increasing
            return false
        }
    }
}
var monotoneIncreasingDigits = function(n) {
    for (let i = n; i >= 0; i++) {
        if (checkNum(i)) return i;
    }
    return 0;
};

/* Greedy
* Time complexity: O(N) where N is the length of num
* Space complexity: O(N) because number is converted into array
*/
var monotoneIncreasingDigits = function(n) {
    n = n.toString();
    const nums = n.split('').map(digit => +digit);

    let flag = Infinity;
    for (let i = nums.length; i > 0; i--) {
        if (nums[i - 1] > nums[i]) {
            flag = i;
            nums[i - 1]--;
            nums[i] = 9;
        }
    }

    for (let j = flag; j < nums.length; j++) {
        nums[j] = 9;
    }
    return nums.join('');
};
```


## 968 Binary Tree Cameras
[Description on Leetcode](https://leetcode.com/problems/binary-tree-cameras/description/)

> ### Intuition
> So we shouldn't put camera on the leaf nodes, instead, we should set the camera of leaf node's parent, which can help to save the amount of cameras (leaf nodes' amount)
> The main difficulties of this exercise are:
> - Traverse the tree
> - Find a way to set the camera
> And because we say we want to set camera at the parents of leaf node, this means we need to traverse the tree from bottom of top. Post-order with left - right - mid can meet our requirements.

### Approach
1. Traverse the tree
2. Define the state of the node: 0 means no camera covers, 1 means having a camera, 2 means being covered by camera. We need to get the returned value from both left and right child, to determine which state of their parent node should be
3. If cur === null, it should return 2, because we don't want to return 0 at least to set its parent with a camera, this will waste at least 1 camera
4. If left === 2 && right === 2, parent should return 0
5. If left === 0 || right === 0, that means parent has to set a camera, return 1
6. If left === 1 || right === 1, that means parent is covered, return 2
7. We need to check root state after traversal. Because we don't want to miss the root

```
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */

/* 
* Time complexity: O(N) because it needs to traverse all nodes of the tree
* Space complexity: O(N) 
*/

/* 
* 0 - means no cover
* 1 - means camera
* 2 - means being covered by camera
*/
var minCameraCover = function(root) {
    let result = 0;
    // const traverse = cur => {
    //     if (cur === null) return 2;
    //     const left = traverse(cur.left);
    //     const right = traverse(cur.right);
    //     if (left === 2 && right === 2) return 0;
    //     if (left === 0 || right === 0) {
    //         result++;
    //         return 1;
    //     }
    //     if (left === 1 || right === 1) return 2;
    //     else return -1;   // but it won't come to this
    // }
    
    // Simplied version
    const traverse = cur => {
        if (cur === null) return 2;
        const left = traverse(cur.left);
        const right = traverse(cur.right);
        if (left === 2 && right === 2) return 0;
        if (left === 0 || right === 0) {
            result++;
            return 1;
        }
        else return 2;
    }
    if (traverse(root) === 0) result++;
    return result;
};
```
