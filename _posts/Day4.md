# Day 4 - 24 Swap Nodes in Pairs | 19 Remove Nth Node from End of List | 160 Intersection of Two Linked Lists | 142 Linked List Cycle II

## 24 Swap Nodes in Pairs
[Description on Leetcode](https://leetcode.cn/problems/swap-nodes-in-pairs/)

> ### Intuition
> Two options of approach when modifying linked list:
> - Use a virtual head
> - Directly modify in the original linked list
>
> Using a virtual head here is easier

### Approach
1. Create a virtual head (dummy head) and set a current pointer
2. For every 2 nodes, swap them by updating their next reference
3. Update cur pointer to point to the next second node

```
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
/* Simlutating
* Time complexity：O(N)
* Space complexity：O(1)
*/
var swapPairs = function(head) {
    let dummy = new ListNode(0, head);     // Using a virtual head
    let cur = dummy;

    while (cur && cur.next && cur.next.next) {
        let temp = cur.next;
        let temp2 = cur.next.next.next;
        cur.next = cur.next.next; 
        cur.next.next = temp;
        cur.next.next.next = temp2;

        cur = cur.next.next;
    }
    return dummy.next;
};
```


## 19 Remove Nth Node from End of List
[Description on Leetcode](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/)

> ### Intuition
> Linked list isn't like array, it can't easily get the nth node. It requires to look up nodes one by one and find the target node in linked list. Also be aware that this exercise is to remove the Nth node from **<em>END</em>** of the list

### Approach
To get the nth node from the end of list, using 2 pointers approach:
- Fast pointer loop up the first N nodes from the start of the list
- Fast pointer move 1 node forward, or without moving forward using a virtual head (!Important: this is to ensure the slow pointer can point the node before the nth node from end of the list at its convenience when deleting it)
- Then slow pointer moves with the fast pointer together, until fast pointer reaches the end of the list
- Now the slow pointer is pointing to the node left to the nth node from the end of the list and can delete it

Note: the key of the approach is to make sure slow pointer is pointing to the previous node of the node to be deleted.

```
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} n
 * @return {ListNode}
 */

/* Two pointers
* Time complexity：O(N)
* Space complexity：O(1)
*/
var removeNthFromEnd = function(head, n) {
    let dummy = new ListNode(0, head);     // Using a virtual head
    let fast = slow = dummy;

    while (n-- && fast) {
        fast = fast.next;
    }

    while (fast.next) {
        fast = fast.next;
        slow = slow.next;
    }
    slow.next = slow.next.next;
    
    return dummy.next;
};
```



## 160 Intersection of Two Linked Lists
[Description on Leetcode](https://leetcode.com/problems/intersection-of-two-linked-lists/description/)

> ### Intuition
> Intersection doesn't mean the value of the node is equal, it means the reference of the node is the same

### Approach
Make list A and list B align with their end of the list first:
1. Move curA till the position of node that aligns with the start of list B.
2. Move curA and curB together till the end of list, if there are nodeA === nodeB, it means we've found the intersection

Note: the key of approach is to make sure list A is always the longer list. If it is, swap list A and list B as well as their length.
  
```
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} headA
 * @param {ListNode} headB
 * @return {ListNode}
 */

/* 
* Time complexity：O(M + N) - M is the length of list A, N is the length of list B
* Space complexity：O(1)
*/
function getListLength(head) {
    let len = 0;
    let cur = head;

    while (cur) {
        cur = cur.next;
        len++;
    }
    return len;
}
var getIntersectionNode = function(headA, headB) {
    let lenA = getListLength(headA);
    let lenB = getListLength(headB);

    if (lenA < lenB) {
        // Make headA the longer list
        [headA, headB] = [headB, headA];
        [lenA, lenB] = [lenB, lenA];
    }

    let curA = headA;
    let curB = headB;

    let diff = lenA - lenB;
    while (diff--) {
        curA = curA.next;      // Move the curA to the same position of list B's start node
    }

    while (curA && curA !== curB) {
        curA = curA.next;
        curB = curB.next;
    }

    return curA;
};
```

## 142 Linked List Cycle II
[Description on Leetcode](https://leetcode.com/problems/linked-list-cycle-ii/description/)

> ### Intuition
> To check whether there's a cycle in a linked list, we need to use two pointers. The fast pointer moves 2 nodes each step while the slow pointer moves only 1 node in a step, if they find the same node, it means there's a cycle in the linked list.
> To find the node of the list begin, we also needs 2 pointers. One pointer starts from the list head, the other pointer starts from the intersection of fast & slow pointer when finding the cycle. The intersection of these 2 pointers is the node of the cycle begin

### Approach
1. Using a fast pointer and a slow pointer to check if there's a cycle in the list.
2. If the cycle exists, use 1 pointer to start from the list head, the other pointer (fast pointer) continues on moving with 1 node each step, till they find the intersection. The intersection node is the node of cycle begin.
  
```
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} head
 * @return {ListNode}
 */

/* 
* Time complexity：O(N)
* Space complexity：O(1)
*/
var detectCycle = function(head) {
    let fast = slow = head;
    while (fast && fast.next) {
        fast = fast.next.next;
        slow = slow.next;

        if (fast === slow) {
            // If fast = slow, get the node of cycle begin
            let index1 = head;
            let index2 = fast;

            while (index1 !== index2) {
                index1 = index1.next;
                index2 = index2.next;
            }
            return index2;
        }
    }

    return null;
};
```
