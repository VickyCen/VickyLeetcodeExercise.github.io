# Day 12 - 144 Binary Tree Preorder Traversal | 94 Binary Tree Inorder Traversal | 145 Binary Tree Postorder Traversal

## 144 Binary Tree Preorder Traversal
[Description on Leetcode](https://leetcode.com/problems/binary-tree-preorder-traversal/)

## 94 Binary Tree Inorder Traversal
[Description on Leetcode](https://leetcode.com/problems/binary-tree-inorder-traversal/)

## 145 Binary Tree Postorder Traversal
[Description on Leetcode](https://leetcode.com/problems/binary-tree-postorder-traversal/)

> ### Intuition
> These exercises are all about tree traversal. There are 3 common approaches for tree traversal:
> - Recursion
> - Iteration by using stack
> - Unified way of iteration for tree traversal
> 

## Recursion

### Approach
Depending on which order of traversal it is required, we can call recursion functions on its left and right child node, and push the node value into the result array.

```js
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
 * @return {number[]}
 */

/* Recursion */

// Preorder Traversal
var traversal = (cur, array) => {
    if (cur === null) return;
    array.push(cur.val);
    traversal(cur.left, array);
    traversal(cur.right, array);
}
var preorderTraversal = function(root) {
    let result = [];
    traversal(root, result);
    return result;
};

// Inorder Traversal
const traversal = (node, result) => {
    if (node === null) return null;
    node.left && traversal(node.left, result);    // left
    result.push(node.val);    // mid
    node.right && traversal(node.right, result);    // right
}

var inorderTraversal = function(root) {
    const result = [];
    traversal(root, result);
    return result;
}

// Postorder Traversal
const traversal = (node, result) => {
    if (node === null) return null;
    node.left && traversal(node.left, result);    // left
    node.right && traversal(node.right, result);    // right
    result.push(node.val);    // mid
}

var postorderTraversal = function(root) {
    const result = [];
    traversal(root, result);
    return result;
}
```


## Iteration

### Approach
Because the underlying implementation of recursion is stack, so we can utilise stack to implement the traversal. Please remember, stack is LIFO, so when we push the tree node into stack, we need to do that in reverse order. For example, for preorder, we need to push right before left.

Preorder: 
1. Stack pop, then push right subtree into stack, then push left subtree into stack

Inorder:
The approach is quite different from preorder - Because we need traversal till the left leave first, then pop elements into stack from left, mid, to right. Here, we will use a pointer to iterate the left subtree till the left leaves, then we pop out the elements, then iteration the mid element, then to the right.

Postorder:
Reverse the preorder traversal, such as mid - right - left to left - right - mid. To ensure the preorder traversal result printing mid - right - left, we need to make sure pushing the element into stack by left first then right

```js
/* Iteration */

// Preorder Traversal
var preorderTraversal = function(root) {
    const stack = [];
    const result = [];
    if (root) stack.push(root);
    while(stack.length) {
        let node = stack.pop();
        result.push(node.val);
        node.right && stack.push(node.right);
        node.left && stack.push(node.left);
    }

    return result;
}

// Inorder Traversal
var inorderTraversal = function(root) {
    const stack = [];
    const result = [];
    let cur = root;
    while (stack.length || cur) {
        if (cur) {
            stack.push(cur);
            cur = cur.left;
            //continue;
        } else {
            cur = stack.pop();
            result.push(cur.val);
            cur = cur.right;
        }
    }

    return result;
}

// Postorder Traversal
var postorderTraversal = function(root) {
    const stack = [];
    const result = [];
    if (root) stack.push(root);
    while(stack.length) {
        let node = stack.pop();
        result.push(node.val);
        node.left && stack.push(node.left);
        node.right && stack.push(node.right);
    }
    return result.reverse();
}

```


## Unified way of Iteration

### Approach
For convenience, we should come up with a unified way for tree traversal when using iteration approach. In fact, we can still do it by using a stack. We can use stack to traverse each element by the reverse order, then when we need to pop out element from stack and write it into result array, we need to use a "null" signal to indicate this is the element to be pushed into result.


```js
/* Unified way of iteration */

// Preorder Traversal
var preTraversal = function(root) {
    const stack = [];
    const result = [];
    if (root) stack.push(root);

    while (stack.length) {
        let cur = stack.pop();

        if (cur === null) {
            result.push(stack.pop().val);
            continue;
        }
        stack.push(cur);     // mid
        stack.push(null);
        cur.right && stack.push(cur.right);    // right
        cur.left && stack.push(cur.left);    // left
    }
    return result;
}


// Inorder Traversal
var inorderTraversal = function(root) {
    const stack = [];
    const result = [];
    if (root) stack.push(root);

    while (stack.length) {
        let cur = stack.pop();

        if (cur === null) {
            result.push(stack.pop().val);
            continue;
        }
        cur.right && stack.push(cur.right);    // right
        stack.push(cur);     // mid
        stack.push(null);
        cur.left && stack.push(cur.left);    // left
    }
    return result;
}

// Postorder Traversal
var postorderTraversal = function(root) {
    const stack = [];
    const result = [];
    if (root) stack.push(root);

    while (stack.length) {
        let cur = stack.pop();

        if (cur === null) {
            result.push(stack.pop().val);
            continue;
        }
        stack.push(cur);     // mid
        stack.push(null);
        cur.right && stack.push(cur.right);    // right
        cur.left && stack.push(cur.left);    // left
    }
    return result;
}
```
