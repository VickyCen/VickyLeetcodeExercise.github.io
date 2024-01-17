# Day 5 - 242 Valid Anagram | 349 Intersection of Two Arrays | 202 Happy Number | 1 Two Sum

## 242 Valid Anagram
[Description on Leetcode](https://leetcode.com/problems/valid-anagram/description/)

> ### Intuition
> This is a basic has problem. 3 common data structure used for hash problems:
> - Array (fixed length)
> - Set (element only appears once)
> - Map (key / value pair)

### Approach
The description mentions "<em>s and t consist of lowercase English letters.</em>", which implies that we can consider to use array because 26 letters is a fixed length:

```
/**
 * @param {string} s
 * @param {string} t
 * @return {boolean}
 */

/* 
* Time complexity：O(M + N) - M is the length of s, N is the length of t
* Space complexity：O(1)
*/
var isAnagram = function(s, t) {
    if (s.length !== t.length) return false;
    let array = new Array(26).fill(0);
    let base = "a".charCodeAt();

    for (let i = 0; i < s.length; i++) {
        array[s[i].charCodeAt() - base]++;    // Recode each letter's existence
    }

    for (let i = 0; i < t.length; i++) {
        if (!array[t[i].charCodeAt() - base]) return false;    // Because s.length === t.length, if there's a letter existence <0, meaning it's not anagram
        array[t[i].charCodeAt() - base]--;     
    }

    return true;
};
```


## 349 Intersection of Two Arrays
[Description on Leetcode](https://leetcode.com/problems/intersection-of-two-arrays/description/)

> ### Intuition
> Linked list isn't like array, it can't easily get the nth node. It requires to look up nodes one by one and find the target node in linked list. Also be aware that this exercise is to remove the Nth node from **<em>END</em>** of the list

### Approach
We can use set here:
1. Make sure nums1 is always the longer array and initialise a result set - because set costs more than array. As we use set here, we want to use a set to record the result, we need to make the result set should be storing the shorter one
2. Convert nums1 to a set
3. Loop into nums2 and when finding the element exists, add it to result set

```
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */

/* 
* Time complexity：O(M + N) - M is the length of nums, N is the length of nums2
* Space complexity：O(N) - Because a new set is used to store the comparison outcome when looping into nums2
*/
var intersection = function(nums1, nums2) {
    // Making it nums1 always the longer arrat
    if(nums1.length < nums2.length) {
        [nums1, nums2] = [nums2, nums1];
    }

    let set = new Set();
    let nums1Set = new Set(nums1);   // Set can convert to array

    for (const i of nums2) {
        if (nums1Set.has(i)) {
            set.add(i);
        }
    }

    return Array.from(set);
};
```



## 202 Happy Number
[Description on Leetcode](https://leetcode.com/problems/happy-number/description/)

> ### Intuition
> At the first glance, it looks like a math exercise. But if we notice what mentions in the description - "<em>Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.</em>", it implies that the calculation of sum is either 1 or in a loop. "Loop" means the calculation result has existed previously, and it reminds us of using hash to check existence of an element.

### Approach
We can use Set to store the sum:
1. Calculate the sum of each digit, and store it into set
2. For each result, check whether it already exists to form a "loop" or equal to 1. 

Note: Be familiar with how to write a function to calculate the sum of each digit
  
```
/**
 * @param {number} n
 * @return {boolean}
 */

/* 
* Time complexity：O(LogN)
* Space complexity：O(LogN)
*/
function getDigitSum(n) {    // Calculate sum of each digit
     let sum = 0;
     while (n) {
         sum += (n % 10) ** 2;
         n = Math.floor(n / 10);
     }
     return sum;
}
var isHappy = function(n) {
    let set = new Set();

    while (true) {
        let sum = getDigitSum(n);
        if (sum === 1) return true;
        if (set.has(sum)) return false;

        set.add(sum);
        n = sum;
    }
};
```

## 1 Two Sum
[Description on Leetcode](https://leetcode.com/problems/two-sum/description/)

> ### Intuition
> We can consider using Map here, because the exercise not only checks the existence but also requires to return their index. We need a data structure to store both.

### Approach
1. Calculate target - nums[i]
2. Check if target - nums[i] has already exists
  
```
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */

/* 
* Time complexity：O(N)
* Space complexity：O(N)
*/
var twoSum = function(nums, target) {
    let map = new Map();

    for (let i = 0; i < nums.length; i++) {
        let rest = target - nums[i];
        if (map.has(rest)) {
            return [i, map.get(rest)];
        } else {
            map.set(nums[i], i);
        }
    }
    return [];
};
```
