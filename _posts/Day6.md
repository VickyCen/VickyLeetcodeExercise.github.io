# Day 6 - 454 4sum II | 383 Ransom Note | 15 3sum | 18 4sum

## 454 4sum II
[Description on Leetcode](https://leetcode.com/problems/4sum-ii/description/)

> ### Intuition
> Use hash table to record a + b, then loop into array C & D to check if 0 - (c + d) exists in the record.

### Approach
Using Map as the data structure to store a + b, because we want to know how many [a, b] combination exists in the array

```
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @param {number[]} nums3
 * @param {number[]} nums4
 * @return {number}
 */

/* 
* Time complexity：O(N ^ 2)
* Space complexity：O(N ^ 2)
*/
var fourSumCount = function(nums1, nums2, nums3, nums4) {
    let map = new Map();

    for (const i of nums1) {
        for (const j of nums2) {
            map.set(i + j, (map.get(i + j) || 0) + 1);
        }
    }

    let count = 0;

    for (const i of nums3) {
        for (const j of nums4) {
            let rest = 0 - (i + j);
            count += map.get(rest) || 0;
        }
    }

    return count;
};
```


## 383 Ransom Note
[Description on Leetcode](https://leetcode.com/problems/ransom-note/description/)

> ### Intuition
> This exercise asks whether ransom element 1-1 exists in the magazine, so we can consider to use hash. Can choose array as data structure here as letters are lowercase. Array is costless than set and map.

### Approach
1. Brute Force
2. Array to record all letter existence of magazine then loop into ransom note to check its existence

```
/**
 * @param {string} ransomNote
 * @param {string} magazine
 * @return {boolean}
 */

/* 
* Brute Force = timeout
* Time complexity：O(N ^ 2)
* Space complexity：O(1)
*/
var canConstruct = function(ransomNote, magazine) {
    for (let i = 0; i < magazine.length; i++) {
        for (let j = 0; j < ransomNote.length; i++) {
            if (magazine[i] === ransomNote[j]) {
                // Found the matching letter
                ransomNote = ransomNote.removeCharAt(j);
                break;
            }
        }
    }

    return ransomNote.length === 0;
};

/* 
* Hash
* Time complexity：O(N)
* Space complexity：O(1)
*/

var canConstruct = function(ransomNote, magazine) {
    let record = new Array(26).fill(0);
    let base = 'a'.charCodeAt();

    for (const i of magazine) {
        record[i.charCodeAt() - base]++;
    }

    for (const i of ransomNote) {
        if (record[i.charCodeAt() - base] <= 0) return false;
        record[i.charCodeAt() - base]--;
    }
    return true;
}
```



## 15 3sum
[Description on Leetcode](https://leetcode.com/problems/3sum/description/)

> ### Intuition
> If we use hash table to record a + b, then check the existence of 0 - (a+b), then we will have trouble with removing the duplicate of a, b and c because the description requires the result should not have duplicate triplet.
> Instead, we can use 2 pointer approach - sort the array first; iterate each a, then use a left pointer to i + 1, and a right pointer to length - 1, comparing nums[i] + nums[left] + nums[right] to 0. Remember to remove duplicate of a, b & c.

### Approach
2 Pointers:
1. For loop into a, if nums[i] === nums[i -1], continue to remove duplicate of a;
2. Use a left pointer to i + 1, and a right pointer to length - 1, comparing nums[i] + nums[left] + nums[right] to 0. If sum < 0, move left forward; if sum > 0, move right backword; Remeber to remove duplicate of b & c.
  
```
/**
 * @param {number[]} nums
 * @return {number[][]}
 */

/*
* 2 pointers
* Time complexity：O(N ^ 2)
* Space complexity：O(1)
*/
var threeSum = function(nums) {
    let result = [];
    nums.sort((a,b) => a - b);

    for (let i = 0; i < nums.length; i++) {
        if (nums[i] > 0) return result; // because it's a sorted array, if the first number > 0, there should be no other triplets foward
        if (nums[i] === nums[i - 1]) continue;

        let left = i + 1, right = nums.length - 1;

        while (left < right) {
            // nums[i] + nums[left] + nums[right] = 0
            if (nums[i] + nums[left] + nums[right] < 0) {
                left++;
            } else if (nums[i] + nums[left] + nums[right] > 0) {
                right--;
            } else {
                result.push([nums[i], nums[left], nums[right]]);

                // remove duplicate
                while (left < right && nums[left] === nums[left + 1]) left++;
                while (left < right && nums[right] === nums[right - 1]) right--;

                left++;
                right--;
            }
        }
    }
    return result;
};


/*
* recursion using 2 pointers for 2sum
* Time complexity：O(N ^ 2)
* Space complexity：O(N)
*/
function nSum(nums, start, target, n) {
    let result = [];
    //if (nums.length < n) return result;

    if (n === 2) {
        result = TwoSum(nums, start, target);
    } else {
        for (let i = start; i < nums.length; i++) {
            let sumResults = nSum(nums, i + 1, target - nums[i], n - 1); 
                for (const item of sumResults) {
                    result.push([nums[i], ...item]);
                }
                while (i < nums.length && nums[i] === nums[i + 1]) i++;
        }
    }
    return result;
}

function TwoSum(nums, start, target) {
    let result = [];
    let left = start, right = nums.length - 1;

    while (left < right) {
        if (nums[left] + nums[right] < target) {
            while (nums[left] === nums[left + 1]) left++;
            left++;
        }
        else if (nums[left] + nums[right] > target) {
            while (nums[right] === nums[right - 1]) right--;
            right--; 
        }
        else {
            result.push([nums[left], nums[right]]);
            while (left < right && nums[left] === nums[++left]);
            while (left < right && nums[right] === nums[--right]);
        }
    }
    return result;
}

var threeSum = function(nums) {
    nums.sort((a, b) => a - b);
    return nSum(nums, 0, 0, 3);
}
```

## 18 4sum
[Description on Leetcode](https://leetcode.com/problems/4sum/description/)

> ### Intuition
> Similar to 3sum's approach, using 2 pointers

### Approach
We need to use 2 For-loop to calculate a + b.
  
```
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[][]}
 */

/* 
* Time complexity：O(N ^ 3)
* Space complexity：O(1)
*/
var fourSum = function(nums, target) {
    let result = [];
    let len = nums.length;
    if (len < 4) return result;

    nums.sort((a, b) => a - b);

    for (let i = 0; i < len - 3; i++) {
        if (i > 0 && nums[i] === nums[i - 1]) continue;   // remove a's duplicate

        for (let j = i + 1; j < len - 2; j++) {
            if(j > i + 1 && nums[j] === nums[j - 1]) continue;     // remove b's duplicate

            let left = j + 1, right = len - 1;

            while (left < right) {
                let sum = nums[i] + nums[j] + nums[left] + nums[right];

                if (sum < target) {
                    left++;
                }
                else if (sum > target) {
                    right--;
                } else {
                    result.push([nums[i], nums[j], nums[left], nums[right]]);

                    while (left < right && nums[left] === nums[left + 1]) {
                        left++;
                    }
                    while (left < right && nums[right] === nums[right - 1]) {
                        right--;
                    }
                    left++;
                    right--;
                }
            }
        }
    }   
    return result;
};
```
