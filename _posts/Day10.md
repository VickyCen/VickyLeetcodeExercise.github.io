# Day 10 - 20 Valid Parentheses | 1047 Remove All Adjacent Duplicates in String | 150 Evaluate Reverse Polish Notation

## 20 Valid Parentheses
[Description on Leetcode](https://leetcode.com/problems/valid-parentheses/description/)

> ### Intuition
> This is a classic problem of using stack. We need to consider these 3 cases when the parentheses are invalid:
> - There are some left parenthese remaining
> - There are no reamining parentheses. but the parenthese type doesn't match
> - There are some right parenthese remaining

### Approach
Using stacks: 
1. If it meets left parenthese, push the corresponding right parenthese into the stack (Here we push the right parenthese instead of the left one, for the convenience of comparing with the current item when popping out the element of the stack. If we just push the left parenthese, when we pop it out for comparison, we need to double check with its right one). Remeber to check the stack length in the end. Only when all parentheses are mathcing && stack.length === 0 means the parentheses are valid

```
/**
 * @param {string} s
 * @return {boolean}
 */

/* 
* Using stack
* Time complexity：O(N)
* Space complexity：O(N)
*/
var isValid = function(s) {
    let stack = [];
    for (let i = 0; i < s.length; i++) {
        let cur = s[i];
        switch (cur) {
            case "(": 
                stack.push(")");
                break;
            case "{": 
                stack.push("}");
                break;
            case "[": 
                stack.push("]");
                break;
            default: 
                let top = stack.pop();
                if (cur !== top) return false;
        }
    }
    return stack.length === 0;
};

/* 
* Simplify version
* Time complexity：O(N)
* Space complexity：O(N)
*/
var isValid = function(s) {
    let stack = [];
    let map = {
        "(": ")",
        "[": "]",
        "{": "}"
    }
    for (const symbol of s) {
        if (symbol in map) {
            stack.push(symbol);
        } else {
            if (map[stack.pop()] !== symbol) return false;
        }
    }
    return stack.length === 0;
};
```


## 1047 Remove All Adjacent Duplicates in String
[Description on Leetcode](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/description/)

> ### Intuition
> Consider to use stack

### Approach
1. Use stack: Once found the iterate element === the top of the stack, pop out the element.
2. Use 2 pointers and simulate stack: a fast pointer to iterate the elements (s[i]), a slow pointer pointing to the top of the stack (s[top]). If s[top] !== s[i], directly pop out the duplicate elements from the original string.

```
/**
 * @param {string} s
 * @return {string}
 */

/*
* Using stack
* Time complexity：O(N)
* Space complexity：O(N)
*/
var removeDuplicates = function(s) {
    const stack = [];
    for (const item of s) {
        if (stack.length && stack[stack.length - 1] === item) {
            stack.pop();
        } else {
            stack.push(item);
        }
    }
    return stack.join("");
};

/*
* Using 2 pointers to simulate stack
* Time complexity：O(N)
* Space complexity：O(1) - because we pop out element directly from the original string
*/
var removeDuplicates = function(s) {
    s = [...s];
    let top = -1;
    for (let i = 0; i < s.length; i++) {
        if (top === -1 || s[top] !== s[i]) {
            s[++top] = s[i]; 
        } else {
            top--;
        }
    }
    s.length = top + 1;
    return s.join("");
};
```


## 150 Evaluate Reverse Polish Notation
[Description on Leetcode](https://leetcode.com/problems/evaluate-reverse-polish-notation/description/)

> ### Intuition
> Consider to use stack

### Approach
Using stack. When it meets number, push into the stack. When it's operator, pop out the last 2 numbers from the stack and calculate the outcome then push back into the stack.

Note: the argument provided is string, so we need to convert string into number when loop into the string. Also, when doing "/" calculation, be aware of the requirement and the cases of positive / negative number. Remember that if we Math.floor the positive numbers, we will do the same by using Math.ceil when it's negative number. 

```
/**
 * @param {string[]} tokens
 * @return {number}
 */

/*
* Using stack
* Time complexity：O(N)
* Space complexity：O(N)
*/
var evalRPN = function(tokens) {
    const stack = [];

    for (const token of tokens) {
        if (isNaN(Number(token))) {
            // If this token is not a number
            let num2 = stack.pop();
            let num1 = stack.pop();
            switch (token) {
                case "+": 
                    stack.push(num1 + num2);
                    break;
                case "-": 
                    stack.push(num1 - num2);
                    break;
                case "*": 
                    stack.push(num1 * num2);
                    break;
                case "/": 
                    stack.push(num1 / num2 > 0 ? Math.floor(num1 / num2) : Math.ceil(num1 / num2));
                    // stack.push(num1 / num2 | 0);  // bitwise operator is binary operator, so it will convert type to int 32 first. So use bitwise operator here will get the integer part of the result
                    break;
            }
        } else {
            stack.push(Number(token));
        }
    }
    return stack[0];
};
```
