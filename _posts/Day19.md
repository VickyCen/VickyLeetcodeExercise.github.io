# Day 19 - 235 Lowest Common Ancestor of a Binary Search Tree | 701 Insert into A Binary Search Tree | 450 Delete Node in A BST

## 235 Lowest Common Ancestor of a Binary Search Tree
[Description on Leetcode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)

> ### Intuition
> When we found the lowest common ancestor of a normal binary tree, we know that if in the subtrees can find both p and q, then this node is the lowest common ancestor of p & q. Now when this solution extends to a binary search tree, because all the node value is sorted from small to large, we can extend the solution to be: if we can find a node with value between [p, q], then this node is p & q's lowest common ancestor of the BST.
> And from the description, we don't know if p is larger than q. So when doing the check, we don't do p.val < node.val && q.val > node.val, instead, we check if both p and q is less than node.val, search in the left subtree; if both p and q is larger than node.val, search in the right subtree.

### Approach
1. Recursion: search in BST and find a node's value between [p, q]
2. Iteration: similar to recursion

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

/* Recursion */
var lowestCommonAncestor = function(root, p, q) {
    if (root === null) return null;
    if (root.val > p.val && root.val > q.val) {
        // Search in the left subtree
        return lowestCommonAncestor(root.left, p, q);
    }
    if (root.val < p.val && root.val < q.val) {
        // Search in the right subtree
        return lowestCommonAncestor(root.right, p, q);
    }
    return root;
};

/* Iteration */
var lowestCommonAncestor = function(root, p, q) {
    while (root) {
        if (root.val > p.val && root.val > q.val) {
            root = root.left;
        }
        else if (root.val < p.val && root.val < q.val) {
            root = root.right;
        } else {
            return root;
        }
    }
}
```


## 701 Insert into A Binary Search Tree
[Description on Leetcode](https://leetcode.com/problems/insert-into-a-binary-search-tree/description/)

> ### Intuition
> In the description, it mentions that we might need to change the tree's strcuture, but in fact, we don't have to do that.
> We just need to find an empty node then insert the node. Be aware of that it's an empty node (node === null), not a leaf node. Because we need to accommodate cases where the left subtree is empty but the right subtree exists. If we look for just leaf node (node.left === null && node.right === null), then we'll miss the chances to insert a new node in an empty left subtree, and force us to traverse in the right subtree - this is incorrect!

### Approach
1. Recursion: find an empty node and insert the new node, then use BST has sorted node values
2. Iteration: similar to recursion

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
var insertIntoBST = function(root, val) {
    if (root === null) return new TreeNode(val);

    if (root.val > val) {
        root.left = insertIntoBST(root.left, val);
    } else {
        root.right = insertIntoBST(root.right, val);
    }
    return root;
}

/* Iteration */
var insertIntoBST = function(root, val) {
    if (root === null) return new TreeNode(val);
    
    let parent = null;
    let cur = root;

    while (cur) {           // We should not look for a leaf node, we use a parent node, if meets null node as child node, then can insert the node
        parent = cur;
        if (cur.val > val) {
            cur = cur.left;
        } else {
            cur = cur.right;
        }
    }
    const node = new TreeNode(val);
    if (parent.val > val) {
        parent.left = node;
    } else {
        parent.right = node;
    }
    return root;
};
```


## 450 Delete Node in A BST
[Description on Leetcode](https://leetcode.com/problems/delete-node-in-a-bst/description/)

> ### Intuition
> Delete a node is more difficult than inserting a node, because deletion requires to change tree's structure

### Approach
1. Recursion for BST: we need to be aware there are cases likely to happen:
   - Traverse all the node and found nothing, return null
   - Found the key node & its left and right subtree is null (key node is leaf node), directly delete key node by returning null
   - Found the key node & its left subtree is null but right subtree is not null, return right subtree root to replace key node with its right subtree
   - Found the key node & its left subtree is not null but right subtree is null, return left subtree root to replace key node with its left subtree
   - Found the key node & both its left and right subtree are not null, then we need to move left subtree root to the right subtree as the left leaf - to keep the sorted values in BST

2. Recursion for normal binary tree: if this is just a binary tree (BST is also a binary tree), we have a general approach to deal with:
   - When found the key node, swap the key node with its right subtree's left leaf node, then remove the right subtree's left leaf node.

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
 * @param {number} key
 * @return {TreeNode}
 */
/* Recursion - BST */
var deleteNode = function(root, key) {
    if (root === null) return root;
    if (root.val === key) {
        if (root.left === null && root.right === null) {
            // Case 1: key node is leaf node
            return null;
        } else if (root.left !== null && root.right === null) {
            // Case 2: left child is not null
            return root.left;
        } else if (root.left === null && root.right !== null) {
            // Case 3: right child is not null
            return root.right;
        } else {
            // Case 4: left child & right child is not null
            // Put left subtree to be the right subtree's left leaf, then return right subtree
            let cur = root.right;
            while (cur.left) {
                cur = cur.left;
            }
            cur.left = root.left;
            return root.right;
        }
    } else if (root.val < key) {
        root.right = deleteNode(root.right, key);
    } else {
        root.left = deleteNode(root.left, key);
    }
    return root;
};


/* Recursion - Normal binary tree */
var deleteNode = function(root, key) {
    if (root === null) return root;

    if (root.val === key) {
        // Switch the key node with the left leaf of the right subtree, then delete the key node in the right subtree
        if (root.right === null) return root.left;
        let cur = root.right;
        while (cur.left) {
            cur = cur.left;
        }
        root.val = cur.val;
        root.right = deleteNode(root.right, cur.val);
    } 
    else if (root.val < key) {
        root.right = deleteNode(root.right, key);
    } else {
        root.left = deleteNode(root.left, key);
    }
    return root;
}

```
