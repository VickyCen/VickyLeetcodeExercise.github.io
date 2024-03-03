# Day 40 - 139 Word Break | Karma 56 Multiple Bag Weight Problem 

## 139 Word Break
[Description on Leetcode](https://leetcode.com/problems/word-break/description/)

> ### Intuition
> We can use backtracking to check all the partition possibility to see if those partitioned words exist in dictionary.
> Or treat it as complete bag weight problem: the str is the bag size, the words in wordDict is items, we are picking items from the wordDict to see we can fill the bag size str with items.
> Be aware of here, we are checking sequence instead of cobinations. Take "applepenapple" as example, result ["apple", "pen"," "apple"] is found, but it's different from ["pen", "apple"," "apple"] or ["apple", "apple"," "pen"]. Here the sequence of words from partition matters.
 
### Approach
1. Backtracking - try all the parition possibility and check whether the partitioned word exists in the wordDict. But this will be timed out. An optimised backtracking solution is to use a memory array to record the checking result of str [startIndex, str.length - 1] starting from startIndex to the end.
2. Dynamic Programming - 1 dimension dp array, using dp[i] will become true if dp[j] is true && str [j, i] exists in dictionary

```
/**
 * @param {string} s
 * @param {string[]} wordDict
 * @return {boolean}
 */

/* BackTracking - iterate all substr and check if they are all found in dictonary 
* Time Complexity: O(2 ^ N) because for each word, it can be partitioned or no partition
* Space Complexity: O(N)
*/
// It will be timed out because the exponencial time complexity
// const backTracking = (str, dicSet, startIndex) => {
//     if (startIndex >= str.length) return true;   // The check has reached the str ends
//     for (let i = startIndex; i < str.length; i++) {
//         const word = str.substr(startIndex, i - startIndex + 1);
//         if (dicSet.has(word) && backTracking(str, dicSet, i + 1)) {
//             // If str [startIndex, i] can be found in wordDict, and str [i, str.length - 1] can be also found in wordDict
//             return true; 
//         }
//     }
//     return false;
// }

// Optimised backtracking using memory to record the result of str [startIndex, str.length - 1]
const backTracking = (str, dicSet, memory, startIndex) => {
    if (startIndex >= str.length) return true;   // The check has reached the str ends
    if (memory[startIndex] !== -1) return memory[startIndex];     // If this startIndex has been calculated, directly return the result 
    for (let i = startIndex; i < str.length; i++) {
        const word = str.substr(startIndex, i - startIndex + 1);
        if (dicSet.has(word) && backTracking(str, dicSet, memory, i + 1)) {
            // If str [startIndex, i] can be found in wordDict, and str [i, str.length - 1] can be also found in wordDict
            return true; 
        }
    }
    memory[startIndex] = false;
    return false;
}
var wordBreak = function(s, wordDict) {
    let dicSet = new Set(wordDict);
    let memory = new Array(s.length).fill(-1);   // -1 is the initial status for all memory[i]
    return backTracking(s, dicSet, memory, 0);
};

/* Dynamic Programming - 1 dimension dp array
* Time Complexity: O(N ^ 3) because time complexity of substr is also N
* Space Complexity: O(N)
*/
var wordBreak = function(s, wordDict) {
    let dp = new Array(s.length + 1).fill(false);
    dp[0] = true;
    // Here we need to get sequences instead of combination
    for (let i = 0; i <= s.length; i++) {
        for (let j = 0; j < wordDict.length; j++) {
            if (i >= wordDict[j].length) {
                if (s.slice(i - wordDict[j].length, i) === wordDict[j] && dp[i - wordDict[j].length] === true) {
                    dp[i] = true;
                }
            }
        }
    }
    return dp[s.length];
}
```


## Karma 56 Multiple Bag Weight Problem 
[Description on Leetcode](https://kamacoder.com/problempage.php?pid=1066)

> ### Intuition
> Multiple bag weight problems: There are N types of items and a bag with size V. For each type of items, you can select at most Mi items to fill the bag. Each item has weight Weight[i] and value[i], how to fill the bag to derive maximum value?
> This is very similar to 01 bag weight problem. Because if we list the Mi items for each type, it becomes 01 bag weight problem, with multiple duplicate items with same weight and value.

### Approach
1. Dynamic Programming - 1 dimension dp array, using dp[j] = Math.min(dp[j], dp[j - coins[i]] + 1).

```
const rl = require('readline').createInterface({ input: process.stdin });
var iter = rl[Symbol.asyncIterator]();
const readline = async () => (await iter.next()).value;

/* Dynamic Programming - 1 dimension dp array
* Time Complexity: O(M × N × K) where M is the number of item types, N is the bag size, k is the number of items for each type
* Space Complexity: O(N)
*/
function multipleBagWeightProblem(bagWeight, weight, value, nums) {
    let dp = new Array(bagWeight + 1).fill(0);
    for (let i = 0; i < weight.length; i++) {  // Iterate items
        for (j = bagWeight; j >= weight[i]; j--) {   // Iterate bag size
            for (k = 1; k <= nums[i] && j >= k * weight[i]; k++) {   // Iterate item amount
                dp[j] = Math.max(dp[j], dp[j - k * weight[i]] + k * value[i]);
            }
        }
    }
    return dp[bagWeight];
}

async function main(){
    let line = await readline();
    const num  = line.split(' ').map(Number);
    const bagWeight = num[0];
    line = await readline();
    let weight = line.split(' ').map(Number);
    line = await readline();
    let value = line.split(' ').map(Number);
    line = await readline();
    let nums = line.split(' ').map(Number);
    
    console.log(multipleBagWeightProblem(bagWeight, weight, value, nums));
}

main();
```
