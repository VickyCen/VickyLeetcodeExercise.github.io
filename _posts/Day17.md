# Day 17 - 654 Maximum Binary Tree | 617 Merge Two Binary Trees | 700 Search in A Binary Search Tree | 98 Validate Binary Search Tree

## 654 Maximum Binary Tree
[Description on Leetcode](https://leetcode.com/problems/maximum-binary-tree/description/)

> ### Intuition
> We can utilise inorder traversal and build the tree from root. The root node is always the largest number of an array from left to right. We pick the largest element as root, and use it to devide into left sub-array and right-array, then build the left and right subtree using the same approach.

### Approach
1. Select the largest element from the array, and make it as root of this subtree.
2. Root will divide the current array into left and right sub-array. Use the same approach to build as left and right subtrees.

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
const buildTree = (array, left, right) => {
    if (left > right) return null;

    let maxValueIndex = -1;
    let maxValue = -Infinity;
    for (let i = left; i <= right; i++) {
        if (array[i] > maxValue) {
            maxValue = array[i];
            maxValueIndex = i;
        }
    }

    const root = new TreeNode(maxValue);
    root.left = buildTree(array, left, maxValueIndex - 1);
    root.right = buildTree(array, maxValueIndex + 1, right);
    return root;
}

var constructMaximumBinaryTree = function(nums) {
    const root = buildTree(nums, 0, nums.length - 1);
    return root;
};
```


## 617 Merge Two Binary Trees
[Description on Leetcode](https://leetcode.com/problems/merge-two-binary-trees/description/)

> ### Intuition
> To construct a tree, we can use inorder traversal or iteration to simulate inorder traversal. The trick here is to check if node1 is empty or node2 is empty, we should return node2 / node1 as a subtree for that node. And we can build new tree directly using node1.

### Approach
1. Recursion - If one of the node is null, return the other node as the new node. Otherwise the new node has the sum of node1 and node2 as the value. Then build its left and right subtree.
2. Iteration - same philosiphy as recursion, we use queue.

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
 * @param {TreeNode} root1
 * @param {TreeNode} root2
 * @return {TreeNode}
 */
/* Recursion */
var mergeTrees = function(root1, root2) {
    if (root1 === null) return root2;
    if (root2 === null) return root1;

    root1.val += root2.val;
    root1.left = mergeTrees(root1.left, root2.left);
    root1.right = mergeTrees(root1.right, root2.right);
    return root1;
};


/* Iteration using queue */
var mergeTrees = function(root1, root2) {
    if (root1 === null) return root2;
    if (root2 === null) return root1;
    let queue = [];
    queue.push(root1);
    queue.push(root2);

    while (queue.length) {
        let node1 = queue.shift();
        let node2 = queue.shift();
        node1.val += node2.val;

        if (node1.left !== null && node2.left !== null) {
            queue.push(node1.left);
            queue.push(node2.left);
        }
        if (node1.right !== null && node2.right !== null) {
            queue.push(node1.right);
            queue.push(node2.right);
        }
        if (node1.left === null && node2.left !== null) {
            node1.left = node2.left;
        }
        if (node1.right === null && node2.right !== null) {
            node1.right = node2.right;
        }
    }
    return root1;
}
```


## 700 Search in A Binary Search Tree
[Description on Leetcode](https://leetcode.com/problems/search-in-a-binary-search-tree/description/)

> ### Intuition
> Binary search tree has sequential nodes. This means we can utilise the characteristic of binary search tree to seach. It's a very foundamental exercise.

### Approach
1. Recursion
2. Iteration - it's even easier. Comparing the value then go to left or right subtree.

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
 * @param {number} val
 * @return {TreeNode}
 */

/* Recursion */
var searchBST = function(root, val) {
    if (root === null || root.val === val) return root;
    if (root.val < val) {
        return searchBST(root.right, val);
    } else if (root.val > val) {
        return searchBST(root.left, val);
    }
    return null;
};

/* Iteration */
var searchBST = function(root, val) {
    while (root) {
        if (root.val === val) return root;
        if (root.val < val) {
            root = root.right;
        } else {
            root = root.left;
        }
    }
    return null;
};

```


## 98 Validate Binary Search Tree
[Description on Leetcode](https://leetcode.com/problems/validate-binary-search-tree/description/)

> ### Intuition
> When dealing with binary search tree, we should think of inorder traversal because we want to leverage the characteristic of the binary search tree.
> This exercise has some traps. The first trap is that we cannot merely compare left child's value with the root or the right child's value with the root, then determine this is a valid binary search tree (Consider this case: preorder 5, 10, 6, 15, 20). We have to ensure all the nodes in the left subtree is less than the root's value. So we need to record the maximum value along the traversal and make sure all the node's value is traversed as the inorder traversal. The second trap is that when comparing values and select the maximum one, the initial value for comparison should be -Infinity, not 0. 

### Approach
1. Recursion: inorder traversal, and record the maximum value (or the previous value, because we are using inorder traversal, the value we come across should be listed from small to large), and check if the values during traversal is from small to large. This is an important criteria of validating a binary search tree.

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
var isValidBST = function(root) {
    let pre = null;

    const inOrder = (root) => {
        if (root === null)
            return true;
        let left = inOrder(root.left);

        if (pre !== null && pre.val >= root.val)
            return false;   // we must make sure all the left subtree is less than the root value, and root value is less than all right subtree
            // As the recursion traverse all the nodes, we can eventually check all the elements are listed from small to large as a tree
        pre = root;

        let right = inOrder(root.right);
        return left && right;
    }
    return inOrder(root);
};

```
