## Valid Sudoku

### Problem

Determine if a Sudoku is valid, according to: [Sudoku Puzzles - The Rules.](http://sudoku.com.au/TheRules.aspx)

The Sudoku board could be partially filled, where empty cells are filled with the character `'.'`.

![](/assets/valid-sudoku.png)
*A partially filled sudoku which is valid.*

**Note:**
A valid Sudoku board (partially filled) is not necessarily solvable. Only the filled cells need to be validated.

**Related Topics:**

`Hash Table`

### Analysis

利用 `HashSet` 的特性，判断 `行`、`列`、`块` 有无重复数字。

### Code

```kotlin
class Solution {

    fun isValidSudoku(board: Array<CharArray>): Boolean {

        val valid = HashSet<String>()

        for (i in 0..8) {
            for (j in 0..8) {
                if (board[i][j] != '.' && (
                                !valid.add(board[i][j] + " in block " + i / 3 + " - " + j / 3) ||
                                        !valid.add(board[i][j] + " in row " + i) ||
                                        !valid.add(board[i][j] + " in column " + j))) {

                    return false
                }
            }
        }

        return true
    }
}
```