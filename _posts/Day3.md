# Day 3 - 203 Remove Linked List Elements | 707 Design Linked List | 206 Reverse Linked List

## 203 Remove Linked List Elements
[Description on Leetcode](https://leetcode.com/problems/remove-linked-list-elements/description/)

> ### Intuition
> This is basic operation on linked list:
> - Previous node's next points to current node (node to be deleted)'s next
> - - Depending on programming language, sometimes it might require to delete current node

### Approach
1. Using a virtual head (dummy head), same operation on the node including head
2. Modify in the original list and requires to handle separate case of deleting the head

```
/**
 * @param {ListNode} head
 * @param {number} val
 * @return {ListNode}
 */

/**
* Using a virtual head
* Time complexity：O(N)
* Space complexity：O(1)
**/
var removeElements = function(head, val) {
    let dummy = new ListNode(0, head);
    let current = dummy;

    while (current.next) {
        if (current.next.val === val) {
            current.next = current.next.next;
            continue; // exit the loop
        }
        current = current.next;
    }
    return dummy.next;
};

/**
* Modify in original list
* Time complexity：O(N)
* Space complexity：O(1)
**/
var removeElements = function(head, val) {
    while (head && head.val === val) head = head.next;
    if (head === null) return head;
    let current = head; 
    while (current.next) {
         if (current.next.val === val) {
            current.next = current.next.next;
            continue; // exit the loop
        }
        current = current.next;
    }

    return head;
};
```


## 707 Design Linked List
[Description on Leetcode](https://leetcode.com/problems/design-linked-list/description/)

> ### Intuition
> This exercise focuses on linked list foundation such as definition, lookup, add node and delete node

### Approach
Two common options when handling linked list:
- Using a virtual head
- Modify the list in-place

This approach is using a virtual head for the linked list. So for each function, will need to take the virtual head position into account when looping within the list

```
/*
* Using a virtual head
* Time complexity：O(index) if it requires to lookup index, others is O(1)
* Space complexity：O(N)
*/
var MyLinkedList = function() {
    this._size = 0;
    this._dummyHead = new ListNode(0);    // virtual head
};

/** 
 * @param {number} index
 * @return {number}
 */
MyLinkedList.prototype.get = function(index) {
    if (index < 0 || index > this._size - 1) return -1;

    let current = this._dummyHead.next;
    while (index--) {
        current = current.next;
    }
    return current.val;
};

/** 
 * @param {number} val
 * @return {void}
 */
MyLinkedList.prototype.addAtHead = function(val) {
    this.addAtIndex(0, val);
};

/** 
 * @param {number} val
 * @return {void}
 */
MyLinkedList.prototype.addAtTail = function(val) {
    this.addAtIndex(this._size, val);
};

/** 
 * @param {number} index 
 * @param {number} val
 * @return {void}
 */
MyLinkedList.prototype.addAtIndex = function(index, val) {
    if (index < 0) index = 0;
    if (index > this._size) return;

    let current = this._dummyHead;
    while (index--) {
        current = current.next;
    }
    let newNode = new ListNode(val);
    newNode.next = current.next;
    current.next = newNode;
    this._size++;
};

/** 
 * @param {number} index
 * @return {void}
 */
MyLinkedList.prototype.deleteAtIndex = function(index) {
    if (index < 0 || index >= this._size) return;
    let current = this._dummyHead;

     while (index--) {
        current = current.next;
    }

    current.next = current.next.next;

    this._size--;

};

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * var obj = new MyLinkedList()
 * var param_1 = obj.get(index)
 * obj.addAtHead(val)
 * obj.addAtTail(val)
 * obj.addAtIndex(index,val)
 * obj.deleteAtIndex(index)
 */
```



## 206 Reverse Linked List
[Description on Leetcode](https://leetcode.com/problems/reverse-linked-list/description/)

> ### Intuition
> Directly simulating the reverse using 2 pointers
> Using recursion: Reverse the first two nodes then recursing from the second node to return a linked list from there

### Approach
1. Two pointers
- 1 pointer points to previous node, 1 pointer points to current node, reverse the nodes by updating their next reference

2. Recursion
- Using recursion to reverse the linked list from the second node. The recursion will always return the new head of reversed linked list from the second node

  
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
 * @return {ListNode}
 */

/*
* Two pointers
* Time complexity：O(N)
* Space complexity：O(1)
*/
var reverseList = function(head) {
    if (!head || !head.next) return head;

    let temp, prev = null, cur = head;
    while (cur) {
        temp = cur.next;
        cur.next = prev;
        prev = cur;
        cur = temp;
    }
    return prev;
};

/*
* Recursion
* Time complexity：O(N) because the recursion tree has N nodes
* Space complexity：O(N) because the recursion tree has N layers (means it calls N of the stack space)
*/
var reverse = function(prev, head) {
    if (head === null) return prev;
    const temp = head.next;
    head.next = prev;
    prev = head;
    return reverse(prev, temp);    // reverse from the second node
}
var reverseList = function(head) {
    return reverse(null, head);
};
```
