# Day 26 - 332 Reconstruct Itinerary | 51 N Queens | 37 Sudoku Solver

## 332 Reconstruct Itinerary
[Description on Leetcode](https://leetcode.com/problems/reconstruct-itinerary/description/)

> ### Intuition
> Here we use normal backtracking approach to solve this problem. It's not an optimisied solution but it still works, because recursion helps us to try all possible path into each level by iteration all the elements.
> The key here is to find out a suitable data structure to store the relationship between the depart city and the destination city. And we should not that 1 depart city can have multiple destinations and we need to handle this.
> We can use a hash map to map between them. Key; depart city, Value: array of all destination city from the depart city. During the recursion, whenever we try one of the destination, we need to temporarily remove it from the array so that it won't cause infinite loop of picking up the destination.
> However, it will be timeout when submitting the solution using this approach. This is because this backtracking approach is like brute force, it's not effiecnet when the tickets is a larger set. We should consider to use graph to solve this problem.

### Approach
1. Iterate all the flights and put them into map. 1 depart city as key, the value should be an array of all possible destination cities.
2. Starting from the last city into result set, treat it as a new depart city, loop into all destination cities that depart from that city, and check if this can find a path that meets all the requirements
3. Remember after recursion, we need to backtrack the changes.

```
/**
 * @param {string[][]} tickets
 * @return {string[]}
 */

/*
* Time complexity; O(N * N!)
* Space complexity: O(N)
*/
var findItinerary = function(tickets) {
    let result = ['JFK'];
    const ticketNum = tickets.length;
    let map = {}

    for (const ticket of tickets) {
        const [from, to] = ticket;
        if (!map[from]) {
            map[from] = [];
        }
         map[from].push(to);
    }

    for (const from in map) {
        map[from].sort();     // Sorting the destination list with alphabetic order
    }

    const backTracking = () => {
        if (result.length === ticketNum + 1) {
            return true;
        }

        if (!map[result[result.length - 1]] || !map[result[result.length - 1]].length) return false;
        for (let i = 0; i < map[result[result.length - 1]].length; i++) { // Loop into all possible destination and find an approprate path
            const city = map[result[result.length - 1]][i];
            map[result[result.length - 1]].splice(i, 1);     // Remove the element from the array then push into result. Otherwise will cause infinite loop
            result.push(city);
            if (backTracking()) return true;
            result.pop();
            map[result[result.length - 1]].splice(i, 0, city);
        }
    }

    backTracking();
    return result;
};


```


## 51 N Queens
[Description on Leetcode](https://leetcode.com/problems/n-queens/description/)

> ### Intuition
> In this exercise, we need to solve these problems:
> - how to use recursion here - we can loop into each row and each col, using chessboard[row][col] to indicate a position, and if this position has a queen, then we recurse for the next row, then next row, to see if we can find an appropriate path.
> - how to we transform the chessboard into data structure - we can use 2-dimension metric
> - how do we know whether this position can have queen - we can have a isValid function, to check whether same column, 45 angle line, 135 angle line, same row has queen before

### Approach
1. Recursion - the backtracking function will have an argument "row", to record for the current recursion, which row we have been traversed the possibilities. Then for the current row, we loop into each col, to see if we take chessboard[row][col] for a queen, can we find a suitable path for the next level till the end. If we did find a path, we return it and push it into result immediately.

```
/**
 * @param {number} n
 * @return {string[][]}
 */

/* 
* Time complexity: O(N!)
* Space complexity: O(N) 
*/
var solveNQueens = function(n) {
    let result = [];
    let chessboard = new Array(n).fill([]).map(() => new Array(n).fill('.'));

    const transfromChessboard = (chessboard) => {
        let resultBoard = [];
        for (let i = 0; i < chessboard.length; i++) {
            let rowStr = '';
            for (let j = 0; j < chessboard[i].length; j++) {
                if (chessboard[i][j] === 'Q') {
                    rowStr += 'Q';
                } else {
                    rowStr += '.';
                }
            }
            resultBoard.push(rowStr);
        }
        return resultBoard;
    }

    const isValid = (row, col, chessboard, n) => {
        // Check whether repear in the same col
        for (let i = 0; i < row; i++) {
            if (chessboard[i][col] === 'Q') return false;
        }

        // Check 45 angle
        for (let i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
            if (chessboard[i][j] === 'Q') return false;
        }

        // Check 135 angle
        for (let i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
            if (chessboard[i][j] === 'Q') return false;
        }
        return true;
    }

    const backTracking = (row, chessboard) => {
        if (row === n) {
            return result.push(transfromChessboard(chessboard));
        }

        for (let col = 0; col < n; col++) {
            if (isValid(row, col, chessboard, n)) {
                chessboard[row][col] = 'Q';
                backTracking(row + 1, chessboard);
                chessboard[row][col] = '.';       // Backtracking
            }
        }
    }

    backTracking(0, chessboard);
    return result;
};
```

## 37 Sudoku Solver
[Description on Leetcode](https://leetcode.com/problems/sudoku-solver/description/)

> ### Intuition
> Using the backtracking approach here, acting as brute force.
> We need to loop into row and col, using 2 for-loop. For a unit of board[row][col], we loop from 1 to 9 to see, for each value, we call recursion for the rest of empty units to see if we can find an appropriate solution.

### Approach
1. Recursion - using 2 for-loop to loop into each unit, then try possibility of 1-9, and calling recursion to check, for a particular value, can we find a possible solution for that. If we do find one (meaning the recursion doesn't return false), we return to parent and inform the parent. Once we try all the possibility for all empty units, we get the result.

```
/**
 * @param {character[][]} board
 * @return {void} Do not return anything, modify board in-place instead.
 */
/*
* Time complexity: O(9 ^ m), where m is the amount of "."
* Space complexity: O(N ^ 2), because the depth of recursion is N ^ 2
*/
var solveSudoku = function(board) {
    const len = board.length;
    const isValid = (row, col, val, board) => {
        // cannot repeat in the same row
        for (let i = 0; i < len; i++) {
            if (board[row][i] === `${val}`) return false;
        }

        // cannot repeat in the same col
        for (let j = 0; j < len; j++) {
            if (board[j][col] === `${val}`) return false;
        }

        // cannot repeat in the small unit
        let startRow = Math.floor(row / 3) * 3;
        let startCol = Math.floor(col / 3) * 3;
        for (let i = startRow; i < startRow + 3; i++) {
            for (let j = startCol; j < startCol + 3; j++) {
                if (board[i][j] === `${val}`) return false;
            }
        }

        return true;
    }

    const backTracking = () => {
        for (let i = 0; i < len; i++) {
            for (let j = 0; j < len; j++) {
                if (board[i][j] !== ".") continue;
                for (let val = 1; val <= 9; val++) {
                    if (isValid(i , j, val, board)) {
                        board[i][j] = `${val}`;
                        if (backTracking()) return true; 
                        board[i][j] = `.`;
                    }
                }
                return false;
            }
        }
        return true;    // remember to return true here. If we loop into all units and doesn't return false, meaning there's no violation and we should return true to parent, to let parent know for the rest we find a solution
    }

    backTracking();
    return board;
};
```
