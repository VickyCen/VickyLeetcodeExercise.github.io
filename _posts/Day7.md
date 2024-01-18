# Day 7 - 344 Reverse String | 541 Reverse String II | Karma 54 Replace Number | 151 Reverse Words in A String | Karma 55 Rotate string to the right

## 344 Reverse String
[Description on Leetcode](https://leetcode.com/problems/reverse-string/description/)

### Approach
Swap the letter from the left and right, pointer moves from 2 end to the centre.

```
/**
 * @param {character[]} s
 * @return {void} Do not return anything, modify s in-place instead.
 */
// var reverseString = function(s) {
//     let round = Math.floor(s.length / 2);
//     for (let i = 0; i < round; i++) {
//         [s[i], s[s.length - 1 -i]] = [s[s.length - 1 -i], s[i]];
//     }
// };

/* 
* Time complexity：O(N)
* Space complexity：O(1)
*/
var reverseString = function(s) {
    let left = 0, right = s.length - 1;

    while (left < right) {
        [s[left], s[right]] = [s[right], s[left]];
        left++;
        right--;
    }
};
```


## 541 Reverse String II
[Description on Leetcode](https://leetcode.com/problems/reverse-string-ii/description/)

> ### Intuition
> Remeber when incrementing i after each loop, increment by 2k.

### Approach
Similar to reverse string approach. 

```
/**
 * @param {string} s
 * @param {number} k
 * @return {string}
 */

/* 
* Time complexity：O(N)
* Space complexity：O(1)
*/
var reverseStr = function(s, k) {
    let result = s.split("");
    for (let i = 0; i < result.length; i += 2 * k) {
        let left = i - 1, right = i + k > result.length ? result.length : i + k;

        while (++left < --right) {
            [result[left], result[right]] = [result[right], result[left]];
        }
    }
    return result.join("");
};
```



## Karma 54 Replace Number
[Description on Leetcode](https://kamacoder.com/problempage.php?pid=1064)

> ### Intuition
> In JS, we can create a new array. Loop into the string, and whenever it meets number, append "number" into the new array.
> But think about this exercise in another angle, we can first loop into the string and determine the total length of the result array, and first assign the appropriate space for the new array, then fill in the array element from the right to the left. This approach of using a fixed length array sometimes costless than dynamically updating an array's length.

### Approach
1. Two pointers: Expand the array and assign element from right to left.
2. Use a new array and append with "number" string
  
```
const readline = require('readline');

const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

/*
* Two pointers: Expand the array size
* Time complexity：O(N)
* Space complexity：O(N)
*/
function replaceStr (str) {
    let numCount = 0;
    for (let i = 0; i < str.length; i++) {
        if (str[i].charCodeAt() >= 48 && str[i].charCodeAt() <= 57) {
            numCount++;
        }
    }
    let array = new Array(str.length + numCount * 5);
    
    for (let i = str.length - 1, j = array.length - 1; i >= 0; i--, j--) {
        if (str[i].charCodeAt() >= 48 && str[i].charCodeAt() <= 57) {
            array[j] = 'r';
            array[j - 1] = 'e';
            array[j - 2] = 'b';
            array[j - 3] = 'm';
            array[j - 4] = 'u';
            array[j - 5] = 'n';
            j -= 5;
        } else {
          array[j] = str[i]; 
        }
        
    }
    
    return array.join("");
}

/*
* Use a new array and append with "number" string
* Time complexity：O(N)
* Space complexity：O(N)
*/
function replaceStr (str) {
    let newStr = '';
    for (let i = 0; i < str.length; i++) {
        if (str[i].charCodeAt() >= 48 && str[i].charCodeAt() <= 57) {
            newStr += 'number';
        } else {
            newStr += str[i];
        }
    }
    return newStr;
}

function proceedInput() {
     rl.on('line', (input) => {
         console.log(replaceStr(input));
     });
}

proceedInput();
```

## 151 Reverse Words in A String
[Description on Leetcode](https://leetcode.com/problems/reverse-words-in-a-string/description/)

> ### Intuition
> We should consider reverse the overll string vs reverse the string portion.
> The description requires to remove extra space. When implementing it, we need to be aware of dividing the task into 3 parts: space in the begin, space between words & space at the end

### Approach
1. Remove extra space
2. Reverse the overall string
3. Reverse each word individually
  
```
/**
 * @param {string} s
 * @return {string}
 */

/*
* Time complexity：O(N)
* Space complexity：O(1)
*/
function reverse(array, start, end) {
    let left = start, right = end;

    while (left < right) {
        [array[left], array[right]] = [array[right], array[left]];
        left++;
        right--;
    }
}

function removeExtraSpace(strArray) {
    let fastIndex = slowIndex = 0;

    // Remove the space at the beginning
    while(strArray[fastIndex] === ' ') {
        fastIndex++;
    }

    while (fastIndex < strArray.length) {
        // This implies fastIndex > 1 when meets the space
        if (strArray[fastIndex] === ' ' && strArray[fastIndex - 1] === ' ') {
            fastIndex++;
        } else {
            strArray[slowIndex++] = strArray[fastIndex++];
        }
    }

    strArray.length = slowIndex;

    // Remove the space in the end
    while (strArray[strArray.length - 1] === " ") {
        strArray.length--;
    }
}
var reverseWords = function(s) {
    let strArr = Array.from(s);

    removeExtraSpace(strArr);

    // reverse the whole array
    reverse(strArr, 0, strArr.length - 1);

    let start = 0;
    for (let i = 0; i <= strArr.length; i++) {
        if (strArr[i] === " " || i === strArr.length) {
            // Reverse the word before this space
            reverse(strArr, start, i - 1);
            start = i + 1;
        }
    }

    return strArr.join("");
};
```

## Karma 55 Rotate string to the right
[Description on Leetcode](https://kamacoder.com/problempage.php?pid=1065)

> ### Intuition
> Very similar to the previous exercise

### Approach
The exercise looks difficult at the first sight. But actually if we utilise reverse overall string then reverse string portion, we can easily solve the problem:
1. Reverse the overall string
2. reverse the first k letters
3. reverse the last string.length - k letters
  
```
const readline = require('readline');

const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

/*
* Time complexity：O(N)
* Space complexity：O(1)
*/
function reverse(strArray, start, end) {
    let left = start, right = end;
    while (left < right) {
        [strArray[left], strArray[right]] = [strArray[right], strArray[left]];
        left++;
        right--;
    }
}

function rightRotate(k, str) {
    let strArray = Array.from(str);
    
    reverse(strArray, 0, strArray.length - 1);
    
    reverse(strArray, 0, k - 1);
    reverse(strArray, k, strArray.length - 1);
    
    return strArray.join("");
}

let inputLines = [];
function proceedInput() {
    rl.on('line', input => {
        inputLines.push(input);
    }).on('close', () => {
        const k = Number(inputLines[0]);
        let str = inputLines[1];
        
        console.log(rightRotate(k, str));
    });
}

proceedInput();

```
