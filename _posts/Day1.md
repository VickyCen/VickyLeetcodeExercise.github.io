# Day 1 - 704 Binary Search | 27 Remove Element

## 704. Binary Search
[Description on Leetcode](https://leetcode.com/problems/binary-search/description/)

> ### Intuition
> Basic binary search:
> - Array must be sorted - the description mentions "<em>sorted in ascending order</em>"
> - Using left & right index to calculate the middle index

### Approach
Compare the target with the middle element. 
- Be aware of the range boundary in the loop: the slight difference between [left, right) & [left, right] 

```
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */

/**
* [left, right)
* Time complexity：O(logN)
* Space complexity：O(1)
**/
 var search = function(nums, target) {
    let left = 0, right = nums.length;
    while (left < right) {
        let mid = left + ((right - left) >> 1);

        if (nums[mid] > target) {
            right = mid;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            return mid;
        }
    }
    return -1;
 };

/**
* OR [left, right]
* Time complexity：O(logN)
* Space complexity：O(1)
**/
var search = function(nums, target) {
   let left = 0, right = nums.length - 1;
   while (left <= right) {
       let mid = left + ((right - left) >> 1);

       if (nums[mid] > target) {
           right = mid - 1;
       } else if (nums[mid] < target) {
           left = mid + 1;
       } else {
           return mid;
       }
   }
   return -1;
};
```


## 27. Remove Element
[Description on Leetcode](https://leetcode.com/problems/remove-element/description/)

> ### Intuition
> - Array element cannot be "remove". It can only be overridden.
> - Ovveridden using swap might update element position and change corresponding index. The exercise description mentions "<em>The order of the elements may be changed</em>" so index update is accepted
> - This exercise requires **in-place** removal, so we should **NOT** consider constructing a new array to return result
> - Although it asks for returning the size of the new arry, the test still checks the updated array copy. So **remember** the updated element has to moved to the left and the new array should start from the 0 index.

### Approach
1. Brute Force
- Use 2 For loops: 1 to iterate all the element, an internal loop to "remove" the element by moving right elements to the left

Or
2. Using 2 pointers: 1 pointer to interate elements, the other pointer to update new array's index

```
/**
 * @param {number[]} nums
 * @param {number} val
 * @return {number}
 */

/**
* Brute Force
* Time complexity：O(N^2)
* Space complexity：O(1)
**/
var removeElement = function(nums, val) {
    let len = nums.length;
    for (let i = 0; i < len; i++) {
        // Remove the element if equal to val
        if (nums[i] === val) {
            // Move the next element to left till the end (remove)
            for (let j = i + 1; j < len; j++) {
                nums[j - 1] = nums[j];
            }
            i--;    // When removing 1 element, the target element has been moved the the tail of the array. No need to check these elements in the last few loops
            len--;
        }
    }
    return len;
};

/**
* OR 2 pointers - fast pointer to iterate all elements, slow pointer to update new array's index
* this approach won't change the position of element from original array
* Time complexity：O(N)
* Space complexity：O(1)
**/
var removeElement = function(nums, val) {
    let slowIndex = fastIndex = 0;
    for (; fastIndex < nums.length; fastIndex++) {
        if (nums[fastIndex] !== val) {
            nums[slowIndex++] = nums[fastIndex];
        }
    }
    return slowIndex;
}

/**
* OR 2 pointers - left pointer to find the euqal value element, right pointer to find the swappable element (not equal to target) then swap left & right element
* this approach has minimum swap between elements but will change the position of element from original array
* Time complexity：O(N^2)
* Space complexity：O(1)
**/
var removeElement = function(nums, val) {
    let left = 0, right = nums.length - 1;

    while (left <= right) {
        if (nums[left] === val) {
            // Check rightIndex
            while (nums[right] === val && left < right) {
                right--;
            }
            [nums[left], nums[right]] = [nums[right], nums[left]];   
            right--;
        } 
        left++;
    }
    return right + 1;
}
```
