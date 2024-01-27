# Day 15 - 110 Balanced Binary Tree | 257 Binary Tree Paths | 404 Sum of Left Leaves

## 110 Balanced Binary Tree
[Description on Leetcode](https://leetcode.com/problems/balanced-binary-tree/description/)

> ### Intuition
> When referring to balanced binary tree, it does not only refer to the difference of left and right subtree's height of root should less than 1, it also refers to all combination of left and right subtrees. So the recursion will need to go through all left and right subtrees. 

### Approach
1. Recursion - If its left subtree or right subtree's height difference is > 1, it should return -1 to its last stack (parent) to indicate this is not a balanced binary tree.
2. Iteration - using preorder traversal to get a node's depth, then traverse the node again using preorder, to compare the height difference with its left and right subtree.

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
 * @return {boolean}
 */

// Here we cannot just compare the left substree's height and right substree's height of root. Because we have to ensure all combination of left subtree and right subtree should be balanced
/* Postorder - recursion */
const getDepth = (node) => {
    if (node === null) return 0;
    let leftDepth = getDepth(node.left);
    if (leftDepth === -1) return -1;
    let rightDepth = getDepth(node.right);
    if (rightDepth === -1) return -1;

    return Math.abs(leftDepth - rightDepth) > 1 ? -1 : 1 + Math.max(leftDepth, rightDepth);
}

var isBalanced = function(root) {
    return getDepth(root) !== -1;
};

/* Iteration - get depth */
const getDepth = (node) => {
    let stack = [];
    if (node) stack.push(node);
    let depth = 0;
    let res = 0;
    while (stack.length) {
        let cur = stack.pop();
        if (cur !== null) {
            stack.push(cur);
            stack.push(null);
            depth++;
            cur.right && stack.push(cur.right);
            cur.left && stack.push(cur.left);
        } else {
            cur = stack.pop();
            depth--;
        }

        res = res > depth ? res : depth;
    }
    return res;
}

var isBalanced = function(root) {
    let stack = [];
    if (root === null) return true;
    stack.push(root);

    while (stack.length) {
        let cur = stack.pop();
        if (Math.abs(getDepth(cur.left) - getDepth(cur.right)) > 1) { // Check left & right subtree's depth difference
            return false;
        }
        cur.right && stack.push(cur.right);
        cur.left && stack.push(cur.left);
    }
    return true;
}
```


## 257 Binary Tree Paths
[Description on Leetcode](https://leetcode.com/problems/binary-tree-paths/description/)

> ### Intuition
> We can traverse the nodes using preorder traversal, and add nodes into path during traversal.

### Approach
1. Recursion - Preorder traversal, and when it meets leaf, add the path into result
2. Iteration - preorder traversal.

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
 * @return {string[]}
 */
var binaryTreePaths = function(root) {
    let result = [];
    const getPath = (cur, curPath) => {
        if (cur.left === null && cur.right === null) {
            curPath += cur.val;
            return result.push(curPath);
        }
        curPath += cur.val + '->';
        cur.left && getPath(cur.left, curPath);
        cur.right && getPath(cur.right, curPath);
    }
    getPath(root, '');
    return result;
};


var binaryTreePaths = function(root) {
    if (root === null) return '';
    let result = []; 
    let stack = [root];
    let pathStack = [''];

    while (stack.length) {
        let cur = stack.pop();
        let path = pathStack.pop();       // cur's corresponding path, starting with cur

        if (cur.left === null && cur.right === null) {
            path += cur.val;
            result.push(path);
        } else {
            path += cur.val + '->';

            if (cur.right) {
                stack.push(cur.right);
                pathStack.push(path);
            }
            if (cur.left) {
                stack.push(cur.left);
                pathStack.push(path);
            }
        }
    }

    return result;
}
```


## 404 Sum of Left Leaves
[Description on Leetcode](https://leetcode.com/problems/sum-of-left-leaves/description/)

> ### Intuition
> We need to understand what "left leaves" means. It means:
> Node A's left child is not null, and node A's left child's left and right child is null. (if (root.left !== null && root.left.left === null && root.left.right === null))
> This implies the left leaves can also exist in right subtree. So we need to sum both left and right subtree's left leaves.

### Approach
1. Recursion - understand when the recursion will return: when root is null or its left and right child is null, this means this node doesn't have any subtrees and no left leaves in its child. And when the node has 1 left leaf, this node's left leave's sum is node.left.val.
2. Iteration - preorder traversal and sum up all left leaves' values.

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
/* Recursion */
var sumOfLeftLeaves = function(root) {
    if (root === null) return 0;
    if (root.left === null && root.right === null) return 0;

    let leftValue = sumOfLeftLeaves(root.left);
    if (root.left !== null && root.left.left === null && root.left.right === null) leftValue = root.left.val;
    let rightValue = sumOfLeftLeaves(root.right);

    return leftValue + rightValue;
};

/* Iteration */
var sumOfLeftLeaves = function(root) {
    let stack = [];
    if (root) stack.push(root);

    let result = 0;
    while (stack.length) {
        let cur = stack.pop();
        if (cur.left !== null && cur.left.left === null && cur.left.right === null) {
            result += cur.left.val;
        }
        cur.left && stack.push(cur.left);
        cur.right && stack.push(cur.right);
    }
    return result;
}

```
