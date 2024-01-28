# Day 16 - 513 Find Bottom Left Tree Value | 112 Path Sum | 113 Path Sum II | 106 Construct Binary Tree from Inorder And Postorder Traversal | 105 Construct Binary Tree from Preorder And Inorder Traversal

## 513 Find Bottom Left Tree Value
[Description on Leetcode](https://leetcode.com/problems/find-bottom-left-tree-value/description/)

> ### Intuition
> For this exercise, we need to be aware of that we have to find the left node on the bottom level. Using level order traversal is straghtforward because we can return the left node value of each level, and the final value is the result.
> If we use recersion, we need to make sure the left node is from the bottom level. So we have to record the current depth and compare, to check whether this is the maximum depth (the bottom level).

### Approach
1. Level order traversal: return the left node value of each level
2. Recursion: 

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

/* Level order traversal */
var findBottomLeftValue = function(root) {
    let queue = [];
    if (root) queue.push(root);
    let result = null;
    while (queue.length) {
        let size = queue.length;

        for (let i = 0; i < size; i++) {
            let cur = queue.shift();
            if (i === 0) result = cur.val;

            cur.left && queue.push(cur.left);
            cur.right && queue.push(cur.right);
        }
    }

    return result;
};

/* Preorder traversal = recursion */
var findBottomLeftValue = function(root) {
    let result = null;
    let maxDepth = -Infinity;     // Need to record the depth, the maxDepth is the bottom level

    const preorder = (node, depth) => {
        if (node === null) return;

        if (node.left === null && node.right === null) {
            if (depth > maxDepth) {
                result = node.val;
                maxDepth = depth;
            }
        }
        preorder(node.left, depth + 1);
        preorder(node.right, depth + 1);
    }

    preorder(root, 1);
    return result;
};
```


## 112 Path Sum | 113 Path Sum II
[112 Description on Leetcode](https://leetcode.com/problems/path-sum/description/)
[113 Description on Leetcode](https://leetcode.com/problems/path-sum-ii/description/)

> ### Intuition
> We can tackle this exercide from the reverse angle - minus the current node, and pass the sum - node.val to the next level child, for the recursion.
> For the iteration approach, we need to store the sum for the current path

### Approach
1. Recursion - Preorder traversal, and when it meets leaf, and the current value === sum - parent.val, returns true; traverse left and right subtree to check if it can find the path.
2. Iteration - preorder traversal and we need to store the sum of the current path.

```
/* 112 Path Sum */
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
 * @param {number} targetSum
 * @return {boolean}
 */

/* Recursion */
var hasPathSum = function(root, targetSum) {
    if (root === null) return false;
    if (root.left === null && root.right === null && root.val === targetSum) {
        return true;
    }

    if (root.left) {
        if (hasPathSum(root.left, targetSum - root.val)) return true;
    }

    if (root.right) {
        if (hasPathSum(root.right, targetSum - root.val)) return true;
    }
    return false;
};

/* Iteration */
var hasPathSum = function(root, targetSum) {
    let stack = [];
    if (root) stack.push([root, root.val]);

    while (stack.length) {
        let cur = stack.pop();

        if (cur[0].left === null && cur[0].right === null && cur[1] === targetSum) return true;

        cur[0].right && stack.push([cur[0].right, cur[1] + cur[0].right.val]);
        cur[0].left && stack.push([cur[0].left, cur[1] + cur[0].left.val]);
    }

    return false;
}
```

> ### Intuition
> If we can understand the recursion approach of 112, we can extend the solution to 113. We need to think about the traceback here, otherwise it will return the wrong results.

### Approach
1. Recursion - Preorder traversal, but we need to be aware of traceback. Because we try to push current node's value into the curPath. But if this node's child does not contain valid path, then we have to traceback(pop the current node value from the curPath because this is not a valid path). 

```
/* 113 Path Sum II */
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
 * @param {number} targetSum
 * @return {number[][]}
 */
/* Recursion */
var pathSum = function(root, targetSum) {
    let result = [];

    const traverse = (node, sum, curPath, result) => {
        if (node === null) return;
        curPath.push(node.val);
        if (node.left === null && node.right === null && sum === node.val) {
            result.push([...curPath]);      // Using spread operator to deep copy. Otherwise the result is [], it won't copy the curPath array completely
        }

        node.left && traverse(node.left, sum - node.val, curPath, result);
        node.right && traverse(node.right, sum - node.val, curPath, result);
        curPath.pop();
    }
    if (root === null) return result;
    traverse(root, targetSum, [], result);
    return result;
}
```


## 106 Construct Binary Tree from Inorder And Postorder Traversal | 105 Construct Binary Tree from Preorder And Inorder Traversal
[106 Description on Leetcode](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/description/)
[105 Description on Leetcode](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/)

> ### Intuition
> Think about the difference between inroder and postorder. We can know that the last element of postorder array is the root value. Then using the root value we can divide the left subtree array and the right subtree array from the inorder array, and so on to construct a binary tree - divide and conquer.
> 105 is very similar to 106.
> However, preorder and postorder cannot determine a binary tree because we cannot divide left subtree and right subtree arrays from the root index.

### Approach
1. Recursion - find the root node from preorder, then get the root index from inorder array, using the root index to divide left subtree and rigt subtree, then construct the binary with subtrees by finding the root index.

```
/* 106 Construct Binary Tree from Inorder And Postorder Traversal */
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {number[]} inorder
 * @param {number[]} postorder
 * @return {TreeNode}
 */
var buildTree = function(inorder, postorder) {
    if (inorder.length <= 0) return null;
    const rootVal = postorder.pop();
    const rootIndex = inorder.indexOf(rootVal);
    const root = new TreeNode(rootVal);
    root.left = buildTree(inorder.slice(0, rootIndex), postorder.slice(0, rootIndex));
    root.right = buildTree(inorder.slice(rootIndex + 1), postorder.slice(rootIndex));
    return root;
};

/* 105 Construct Binary Tree from Preorder And Inorder Traversal */
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {number[]} preorder
 * @param {number[]} inorder
 * @return {TreeNode}
 */
var buildTree = function(preorder, inorder) {
    if (inorder.length === 0) return null;

    const rootVal = preorder.shift();
    const rootIndex = inorder.indexOf(rootVal);
    const root = new TreeNode(rootVal);

    root.left = buildTree(preorder.slice(0, rootIndex), inorder.slice(0, rootIndex));
    root.right = buildTree(preorder.slice(rootIndex), inorder.slice(rootIndex + 1));
    return root;
};

```
