# Day 18 - 530 Minimum Absolute Difference in Bst | 501 Find Mode in Binary Search Tree | 236 Lowest Common Ancestor of A Binary Tree

## 530 Minimum Absolute Difference in Bst
[Description on Leetcode](https://leetcode.com/problems/minimum-absolute-difference-in-bst/description/)

> ### Intuition
> Remember: for BST, we should use inorder traversal, because we can leverage that BST's nodes are sorted from left to right

### Approach
1. Recursion: inorder traversal and using a pre pointer to calculate adjacent node's value difference
2. Iteration: using stack to mimic recursion

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
var getMinimumDifference = function(root) {
    let result = Infinity;
    let pre = null;

    const inorderTraversal = (cur) => {
        if (cur === null) return;

        inorderTraversal(cur.left);
        if (pre !== null) {
            result = Math.min(cur.val - pre.val, result);
        }
        pre = cur;
        inorderTraversal(cur.right);
    }

    inorderTraversal(root);
    return result;
};


/* Iteration */
var getMinimumDifference = function(root) {
    let stack = []; 
    let result = Infinity;
    let pre = null;

    let cur = root;

    while (cur || stack.length) {
        if (cur !== null) {
            stack.push(cur);
            cur = cur.left;     // Left
        } else {
            cur = stack.pop();   // Mid
            if (pre !== null) {
                result = Math.min(result, cur.val - pre.val);
            }
            pre = cur;
            cur = cur.right;   // Right
        }
    }
    return result;
}
```


## 501 Find Mode in Binary Search Tree
[Description on Leetcode](https://leetcode.com/problems/find-mode-in-binary-search-tree/description/)

> ### Intuition
> If we treat it as a normal binary tree, we can use hash map to record the appearance frequency of each node.
> But because this is a BST and node's value are sorted, that means, if mode exists in the tree, it will appear as adjacent nodes. 

### Approach
1. Normal binary tree - preorder traversal and record appearnce frequency in hash map, then loop into map to get the mode
2. BST - inorder traversal and check if previous node's value is equal to current node's value

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
 * @return {number[]}
 */

/* For normal binary tree - hash map for frequency */
var findMode = function(root) {
    let map = new Map();
    const preorder = (node) => {
        if (node === null) return;
        map.set(node.val, (map.get(node.val) || 0) + 1);
        preorder(node.left);
        preorder(node.right);
    }
    preorder(root);

    let result = [];
    let maxCount = map.get(root.val);   // Use as initial max count
    for (const [key, value] of map) {
        if (value === maxCount) {
            result.push(key);
        } else if (value > maxCount) {
            result = [];
            maxCount = value;
            result.push(key);
        }
    }
    return result;
};

/* inorder traversal - no need to use hash map */
var findMode = function(root) {
    let result = [];
    let pre = null;
    let count = 0;
    let maxCount = -Infinity;

    const inorder = (node) => {
        if (node === null) return;
        inorder(node.left);
        if (pre !== null && pre.val === node.val) {
            count++;
        } else {
            count = 1;
        }
        pre = node;
        if(count === maxCount) {
            result.push(node.val);
        }
        if(count > maxCount) {
            result = [];
            maxCount = count;
            result.push(node.val);
        }
        inorder(node.right);
    }
    inorder(root);
    return result;
};
```


## 236 Lowest Common Ancestor of A Binary Tree
[Description on Leetcode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/)

> ### Intuition
> How to find the lowest common ancestor?
> If in its left subtree or right subtree, both find p and q, even if p / q is the current root node, the current root node is p and q's lowest common ancestor.

### Approach
1. Recursion: find p and q from left and right subtree. If left and right subtree both return found results, then current root is the lowest common ancestor; if either left or right subtree return null, we need to return the other subtree found result, because both p and q can exist in one side subtree, or not found in both subtree. See the original logic and the simplied version.

```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {TreeNode}
 */
var lowestCommonAncestor = function(root, p, q) {
    if(root === null || root === p || root === q) {
        return root;
    }
    let left = lowestCommonAncestor(root.left,p,q);
    let right = lowestCommonAncestor(root.right,p,q);
    if(left !== null && right !== null) {
        return root;
    }
    // else if (left !== null && right === null) {
    //     return left;
    // } else if (left === null && right !== null) {
    //     return right;
    // } else {
    //     return null;   // left === null && right === null
    // }

    // Simplifed
    if(left === null) {
        return right;
    }
    return left;
};

```

