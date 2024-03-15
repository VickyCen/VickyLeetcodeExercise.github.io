# Day 53 - Summary

First of all, congratulations! We have walked through and successfully completed 150 Leetcode exercises! Here we'll summarise the data structure or approches that we've commonly applied for solving Leetcode's exercise. 

## Array

Array has continuous memory for storage, dealing with array we'll usually use loop to traverse the elements within array. 
Some classic approad to solve array related problems:
- Brute Force: Time complexity: O(n)
- Binary Search: Time complexity: O(n)
- Two Pointers: Time complexity: O(n) (with O(n) of time if using Brute Force)
- Sliding Window: Time complexity: O(n) (with O(n) of time if using Brute Force)


## Linked List

We have different types of linked list: Single Linked List, Double Linked List, Circular Linked List. And we should familiarise with add / delete / search node operation for a linked list.
Some classic approad to solve array related problems:
- Using a dummy head
- Deleting a node requires to update the pointer from previous node to next node
- Comparing node
- Determine if it's a circular linked list - fast / slow pointers, it they intersect, there's a linked list.


## Hash Table

We should know that array, set and map, are forms of hash table. Array will have fixed length, and search with complexity of O(N). Set and map has search with complexity of O(1). Set in Javascript means element will become unique and only appears once, while map is represents in key / value pairs. Map in Javascript can have different type for keys, while Object keys has to be string.
We should choose appropriate data structure when dealing with hash table. 


## String

String is a simple data type, but it can link to complex problems. 
Some classic approad to solve array related problems:
- 2 Pointers - treat string as an array
- KMP Algorithm: generate a prefix table and use this algorithm to match substring from 1 string with another string.


## Stack & Queue

Stack - FILO; Queue - FIFO
We need to understand the difference between stack and queue and leverage their characteristic to solve problems by using appropriate data structure.


## Tree

3 Traversal for tree: Pre-order, In-order, Post-order. 
2 types of traversal:
- Depth-first traversal: Recursion / Stack
- Breath-first traversal: Queue
Choosing between traversal can be complicated so we need to familiarise with their differences.


## BackTracking

Backtracking is a side effect of recursion. Whenever recursion exists, there will be backtracking behind it. In terms of whether we need to revert back using backtracking for recursion, we need to check for each level of recursion, whether the data has been updated after that level recursion. If it does, we need to do backtracking; otherwise, we don't need to backtracking.
Classic steps to write backTracking function:
- Determine return value and arguments
- Condition for terminating the recursion
- Logic for each level of recursion



## Greedy Algorithm

The thought process behind Greedy Algorithm is that, we want to utilise partial best result to achieve global best result. So we need to think about how can we partial best result then simulate the whole process to see if we can achieve global best result by naming some examples.


## Dynamic Programming

Dynamic Programming requires us to use a dp array to store the result for each state, the next state's result relies on the last state results - we need to find out the logic and relationship between states transition.
Classic steps to use Dynamic Programming approach:
- Determine what dp array represents and the meaning of index
- Determine how state can be transited from last state
- Understand the initial value of dp array
- Determine the state traversal sequence (depending on if the state transits from last state or the state after it)
- Simulate the state transition and see if the result is valid by naming examples


## Monotonic Stack

Monotonic Stack requires us to maintain a stack that from stack top to bottom, the value of elements keeps increasing or descreasing. This can help us to find out the first larger / less value on the left / right hand side as array traversal. 
The main trick of using Monotonic Stack is that we need understand whether we should maintain the stack value to increasing or decreasing. 

    
