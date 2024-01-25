# Day 13 - 226 Invert Binary Tree | 101 Symmetric Tree

## 226 Invert Binary Tree 
[Description on Leetcode](https://leetcode.com/problems/invert-binary-tree/description/)

> ### Intuition
> This exercises is actually asking us to swap the left node and right node.
> We need to traverse the tree and invert the left node and right node. For tree traversal, we have 3 common ways:
> - ecursion (DFS)
> - Iteration using stack (DFS), as recursion is using stack underlying
> - Using queue for level order traversal (BFS)
>
> But be careful, when using DFS, we can solve the problem by either preorder or postorder traversal, but **NOT** inorder. This is because if we swap left node & right node using in order, some left nodes / right nodes will be swapped more than twice. 

### Approach
Basic tree traversal, when traverse a node, swap its left node & right node.

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

/* Recursion - but cannot use inorder traversal because it will accidentially swap left, right twice */
var invertTree = function(root) {
    if (root === null) return root;

    [root.left, root.right] = [root.right, root.left];
    invertTree(root.left);
    invertTree(root.right);
    return root;
};

/* Using stack for DFS - like recursion */
var invertTree = function(root) {
    let stack = [];
    if (root) stack.push(root);

    while (stack.length) {
        let cur = stack.pop();
        if (cur === null) {
            cur = stack.pop();
            [cur.left, cur.right] = [cur.right, cur.left];
        } else {
            stack.push(cur);
            stack.push(null);
            cur.right && stack.push(cur.right);
            cur.left && stack.push(cur.left);
        }
    }
    return root;
}

/* BFS traversal nodes level by level */
var invertTree = function(root) {
    let queue = [];
    if (root) queue.push(root);

    while (queue.length) {
        let size = queue.length;

        for (let i = 0; i < size; i++) {
            let cur = queue.shift();
            [cur.left, cur.right] = [cur.right, cur.left];
            cur.left && queue.push(cur.left);
            cur.right && queue.push(cur.right);
        }
    }
    return root;
}

```


## 101 Symmetric Tree
[Description on Leetcode](https://leetcode.com/problems/symmetric-tree/description/)

> ### Intuition
> The approach to check whether a tree is symmetric is quite different from inverting a tree. Because for checking symmetric tree, we are not just comparing left node and right node. Instead, we are comparing nodes from outside to inside (left.left -- right.right, left.right -- right.left). In this case, we need to think about a few case when comparing the nodes from left subtree and the right subtree:
> - left === null && right === null   - symmetric
> - left !== null && right === null || left === null && right !== null   - not symmetric
> - left.val !== right.val  - not symmetric
>
> These exercise is actually asking us to compre root.left and root.right (2 subtrees of root node) to see if they are inverted. So the traversal sequence would be for left tree, left -> right -> mid, and for the right tree: right -> left -> mid. Pick up a left node from left subtree, and pick up a right node from right subtree, and compare them to see if they are symmetric.

### Approach
1. Recursion: by comparing left node from left subtree and right node from right subtree to see if it's symmetric.
2. Using queue to push nodes from left.left, right.right, left.right and right.left, and comparing 2 nodes in pairs. (Note this is different from level order traversal (BFS). The approach here is actually DFS, very similiar to recursion but we can use either queue or stack).

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

/* Recursion */
const compare = (left, right) => {
    if (left === null && right === null) return true;
    else if (left === null || right === null) return false;
    else if (left.val !== right.val) return false;

    let outSideCompare = compare(left.left, right.right);
    let inSideCompare = compare(left.right, right.left);

    return outSideCompare && inSideCompare;
}

var isSymmetric = function(root) {
    if (root === null) return true;
    return compare(root.left, root.right);
};


/* DFS - compare outside nodes && inside nodes */
var isSymmetric = function(root) {
   let queue = [];
   if (root) {
       queue.push(root.left);
       queue.push(root.right);
    }

    while (queue.length) {
           let left = queue.shift();
           let right = queue.shift();

           if (left === null && right === null) {
               continue;
           }
           if (left === null || right === null || left.val !== right.val) return false;

           queue.push(left.left);
           queue.push(right.right);
           queue.push(left.right);
           queue.push(right.left);
    }
    return true;
};

```
