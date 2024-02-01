# Day 20 - 669 Trim A Binary Search Tree | 108 Convert Sorted Array to Binary Search Tree | 538 Convert BST to Greater Tree

## 669 Trim A Binary Search Tree
[Description on Leetcode](https://leetcode.com/problems/trim-a-binary-search-tree/description/)

> ### Intuition
> When we meet a child node, for example, left child node, if its value < low, we should not trim the whole left subtree, because the left node's right subtree might fall into [low, high] range. This is something that we should be aware of. In this case, we should trim into its left node's right subtree. If a root node has fall between [low, high], then we need to trim both left and right subtree.

### Approach
1. Recursion: When we found a node outside the range, we should look into its other subtree, instead of trimming the whole subtree.
2. Iteration: First, we need to find the root node that can fall between [low, high], then using inorder traversal from right, mid to left, to trim the tree.

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
 * @param {number} low
 * @param {number} high
 * @return {TreeNode}
 */
/* Recursion */
var trimBST = function(root, low, high) {
    if (root === null) return null;
    if (root.val < low) return trimBST(root.right, low, high);
    if (root.val > high) return trimBST(root.left, low, high);

    root.left = trimBST(root.left, low, high);;
    root.right = trimBST(root.right, low, high);
    return root;
};

/* Iteration */
var trimBST = function(root, low, high) {
    if (root === null) return null;

    while (root !== null && (root.val < low || root.val > high)) {
        if (root.val < low) {
            root = root.right;
        } else {
            root = root.left;
        }
    }
    let cur = root;
    while (cur) {
        while (cur.left && cur.left.val < low) {
            cur.left = cur.left.right;    // Trim left child but remember to keep its right subtree
        }
        cur = cur.left;
    }
    cur = root;
    while (cur) {
        while (cur.right && cur.right.val > high) {
            cur.right = cur.right.left;    // Trim right child but remember to keep its left subtree
        }
        cur = cur.right;
    }
    return root;
}
```


## 108 Convert Sorted Array to Binary Search Tree
[Description on Leetcode](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/)

> ### Intuition
> Very similar to construct a binary tree, we need to first find the root node (finding the mid index), then construct its left and right subtree using the same approach.
> Here we leverage BST has sorted nodes.

### Approach
1. Recursion: find the mid index and use it to construct its left and right subtree
2. Iteration: very similar to recursion

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
 * @param {number[]} nums
 * @return {TreeNode}
 */

/* Find mid and construct the tree using inorder */
var sortedArrayToBST = function(nums) {
    const buildTree = (array, left, right) => {
        if (left > right) return null;
        let mid = Math.floor(left + (right - left) / 2);
        const node = new TreeNode(array[mid]);
        node.left = buildTree(array, left, mid - 1);
        node.right = buildTree(array, mid + 1, right);
        return node;
    }
    
    return buildTree(nums, 0, nums.length - 1);
};

/* Iteration */
var sortedArrayToBST = function(nums) {
    if (nums.length <= 0) return null;

    let queue = [];
    let leftIndex = [];
    let rightIndex = [];
    let root = new TreeNode(0);
    queue.push(root);
    leftIndex.push(0);
    rightIndex.push(nums.length - 1);

    while (queue.length) {
        let cur = queue.shift();
        let left = leftIndex.shift();
        let right = rightIndex.shift();
        let mid = Math.floor(left + (right - left) / 2);
        cur.val = nums[mid];

        // Left subtree
        if (left < mid) {
            cur.left = new TreeNode(0);
            queue.push(cur.left);
            leftIndex.push(left);
            rightIndex.push(mid - 1);
        }

        if (right > mid) {
            cur.right = new TreeNode(0);
            queue.push(cur.right);
            leftIndex.push(mid + 1);
            rightIndex.push(right);
        }
    }
    return root;
}
```


## 538 Convert BST to Greater Tree
[Description on Leetcode](https://leetcode.com/problems/convert-bst-to-greater-tree/description/)

> ### Intuition
> Use inorder traversal (right, mid to left), using a pre pointer to add up the values

### Approach
1. Recursion: right - mid - left inorder traversal to add up the values
2. Iteration: very similar to recursion

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
 * @return {TreeNode}
 */
/* Recursion */
var convertBST = function(root) {
    let pre = 0;
    const traverse = (node) => {
        if (node === null) return 0;
        traverse(node.right);   // inorder but start from right
        node.val += pre;
        pre = node.val;
        traverse(node.left); 
    }
    traverse(root);
    return root;
};

/* Iteration - similiar to inorder */
var convertBST = function(root) {
    let pre = 0;
    let stack = [];
    let cur = root;

    while (cur || stack.length) {
        if (cur !== null) {
            stack.push(cur);
            cur = cur.right;
        } else {
            cur = stack.pop();
            cur.val += pre;
            pre = cur.val;
            cur = cur.left;
        }
    }
    return root;
}

```
