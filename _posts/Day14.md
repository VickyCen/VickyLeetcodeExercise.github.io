# Day 14 - 104 Maximum Depth of Binary Tree | 111 Minimum Depth of Binary Tree | 222 Count Complete Tree Nodes

## 104 Maximum Depth of Binary Tree
[Description on Leetcode](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/)

> ### Intuition
> The maximum depth of a tree is actually the maximum levels of a tree.
> For getting depth, we can use DFS (recursion), while we can use BFS(level order traversal) fpr calculating the levels

### Approach
1. Recursion - Preorder / Postorder are both working. Calcaulte the depth of the tree during trversal
2. Level order traversal using queue - straightforward approach to calculate the levels of a tree

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

/* Preorder
*/
var maxDepth = function(root) {
    let result = 0;
    const getDepth = (node, depth) => {
        result = result < depth ? depth : result;

        node.left && getDepth(node.left, depth + 1);      // Backtrace is in depth + 1
        node.right && getDepth(node.right, depth + 1);  
    }

    if (root === null) return result;
    getDepth(root, 1);
    return result;
}

/* Postorder
*/

// Simplified version
var maxDepth = function(root) {
    if (root === null) return 0;

    return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}

var maxDepth = function(root) {
    const getDepth  = (node) => {
        if (node === null) return 0;

        leftDepth = getDepth(node.left);
        rightDepth = getDepth(node.right);
        return 1 + Math.max(leftDepth, rightDepth);
    } 

    return getDepth(root);
}

/* Level order traversal */
var maxDepth = function(root) {
    const queue = [];
    if (root) queue.push(root);
    let depth = 0;
    while (queue.length) {
        const size = queue.length;
        depth++;
        for (let i = 0; i < size; i++) {
            let cur = queue.shift();
            cur.left && queue.push(cur.left);
            cur.right && queue.push(cur.right);
        }
    }
    return depth;
}
```


## 111 Minimum Depth of Binary Tree
[Description on Leetcode](https://leetcode.com/problems/minimum-depth-of-binary-tree/description/)

> ### Intuition
> This is quite different from the last exercise of getting the maximum depth. Here it requires to get the minimum depth, which means we need to return when meeting the leaves. But when left subtree is null for example, we need to remember to traverse the right subtree. Because the leaves can exist in right subtree if left subtree is empty. It is important to be aware of this case.

### Approach
1. Recursion - Preorder (compare current with result and return the minimum one) / Postorder (if left subtree or right subtree is empty, need to traverse the other subtree to find the leaves)
2. Level order traversal using queue - return depth when it meets the first leaf

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

/* Preorder */
var minDepth = function(root) {
    if (root === null) return 0;
    let result = Infinity;

    const getDepth = (node, depth) => {
        if (node === null) return 0;
        if (node.left === null && node.right === null) {
            result = Math.min(result, depth);
        }

        node.left && getDepth(node.left, depth + 1);
        node.right && getDepth(node.right, depth + 1);
    }
    getDepth(root, 1);
    return result;
}

/* Postorder */
var minDepth = function(root) {
    if (root === null) return 0;

    if (root.left === null && root.right !== null) {
        return 1 + minDepth(root.right);
    }

    if (root.left !== null && root.right === null) {
        return 1 + minDepth(root.left);
    }

    return 1 + Math.min(minDepth(root.left), minDepth(root.right));
}

/* Level order traversal */
var minDepth = function(root) {
    const queue = [];
    if (root) queue.push(root);
    let levels = 0;

    while (queue.length) {
        let size = queue.length;
        levels++;
         for (let i = 0 ; i < size; i++) {
            let cur = queue.shift();
            if (!cur.left && !cur.right) return levels;
            cur.left && queue.push(cur.left);
            cur.right && queue.push(cur.right);
        }
    }
    return levels;
}

```


## 222 Count Complete Tree Nodes
[Description on Leetcode](https://leetcode.com/problems/count-complete-tree-nodes/description/)

> ### Intuition
> For an ordinary binary tree, we can simply traverse all the nodes then count the nodes
> For a complete tree nodes, we can notice that it consists of full binary tree (single node is also a full binrary tree ) as left subtree and right subtree. How to determine if it's a full binary tree? Traverse the nodes on the left till the left leaf, and traverse the nodes on the right till the right leaf, if left depth is equal to the right depty, the tree is a complete binary tree. For a binary binary tree, the count of the nodes is 2 ^ k - 1 while k is the levels of the tree.

### Approach
1. Recursion - Postorder traversal and count the tree nodes
2. Level order traversal and count the nodes
3. Recursion to check full binary trees for its subtrees. A full binary tree has 2 ^ k - 1 nodes.

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
/* Postorder for normal binary tree */
var countNodes = function(root) {
    if (root === null) return 0;
    let leftCount = countNodes(root.left);
    let rightCount = countNodes(root.right);

    return leftCount + rightCount + 1;
};

/* Level order traversal */
var countNodes = function(root) {
    let queue = [];
    if (root) queue.push(root);
    let count = 0;
    while (queue.length) {
        let size = queue.length;
        for (let i = 0; i < size; i++) {
            let cur = queue.shift();
            count++;
            cur.left && queue.push(cur.left);
            cur.right && queue.push(cur.right);
        }
    }
    return count;
}

/* Specific for complete binary tree */
var countNodes = function(root) {
    if (root === null) return 0;

    let left = root.left;
    let right = root.right;
    let leftDepth = rightDepth = 0;

    while (left) {
        leftDepth++;
        left = left.left;
    }

    while (right) {
        rightDepth++;
        right = right.right;
    }

    if (leftDepth === rightDepth) {
        return Math.pow(2, leftDepth + 1) - 1;    // remember to  +1 here, because the original depth is calculated from 0, not 1
    }

    return countNodes(root.left) + countNodes(root.right) + 1;
}   

```
