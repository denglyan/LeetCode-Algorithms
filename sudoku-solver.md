## Sudoku Solver

### Problem

Write a program to solve a Sudoku puzzle by filling the empty cells.

Empty cells are indicated by the character `'.'`.

You may assume that there will be only one unique solution.

![](/assets/valid-sudoku.png)

*A sudoku puzzle...*

![](/assets/sudoku-solver.png)

*...and its solution numbers marked in red.*

**Related Topics:**

`Hash Table` `Backtracking`

### Analysis

方法一：每个空格子，分别设置 `1..9` 进行判断。若成功，判断下一个点，若都失败，则回溯。

方法二：设置约束。给每个格子配备一个约束数组，其约束该格子内，哪些数可以填写。在遍历格子，形成约束的过程中，某些格子可能被约束限定，只能填写某个数。如此一来，不需要尝试、回溯，即可确定填写的数。剩下的空格子，需要尝试约束后的各种可能值。这里通过排序（按照可能值的数量，从小到大），优先处理容易成功的，可以减少尝试判断的次数。

### Code

**暴力：**

```kotlin
class Solution {

    fun solveSudoku(board: Array<CharArray>): Unit {

        if (board.isNotEmpty())
            solve(board, 0, 0)
    }

    private fun solve(board: Array<CharArray>, row: Int, col: Int): Boolean {

        var c = col
        for (i in row..8) {
            for (j in c..8) {
                if (board[i][j] == '.') {
                    for (num in '1'..'9') {
                        if (valid(board, i, j, num)) {

                            board[i][j] = num
                            if (solve(board, row, c + 1)) {
                                return true
                            }

                            board[i][j] = '.'
                        }
                    }

                    return false
                }
            }
            c = 0
        }

        return true
    }

    private fun valid(board: Array<CharArray>, row: Int, col: Int, target: Char): Boolean {

        val blockRow = (row / 3) * 3
        val blockCol = (col / 3) * 3

        return (0..8).none {
            board[it][col] == target ||
                    board[row][it] == target ||
                    board[blockRow + it / 3][blockCol + it % 3] == target
        }
    }
}
```

**设置约束：**

```kotlin
import java.io.ByteArrayInputStream
import java.io.ByteArrayOutputStream
import java.io.ObjectInputStream
import java.io.ObjectOutputStream
import java.io.Serializable

class Solution {

    fun solveSudoku(board: Array<CharArray>): Unit {

        cells = Array(9) { Array(9, { Cell() }) }

        for (i in 0..8) {
            for (j in 0..8) {
                when {
                    board[i][j] != '.' && !set(i, j, board[i][j] - '0') -> return
                }
            }
        }

        when {
            !findValuesForEmptyCells() -> return
        }

        for (i in 0..8) {
            for (j in 0..8) {
                board[i][j] = '0' + cells[i][j].value
            }
        }
    }

    class Cell : Serializable {
        var value = 0
        var numPossibilities = 9
        var constraints = BooleanArray(10)
    }

    private lateinit var cells: Array<Array<Cell>>
    private lateinit var emptyCells: MutableList<Pair<Int, Int>>

    fun set(i: Int, j: Int, value: Int): Boolean {

        val cell = cells[i][j]
        when {
            cell.value == value -> return true
            cell.constraints[value] -> return false
        }

        cell.constraints = BooleanArray(10, { true })
        cell.constraints[value] = false
        cell.numPossibilities = 1
        cell.value = value

        for (it in 0..8) {

            when {
                i != it && !updateConstraints(it, j, value) -> return false
                j != it && !updateConstraints(i, it, value) -> return false
                (i / 3) * 3 + it / 3 != i && (j / 3) * 3 + it % 3 != j &&
                        !updateConstraints((i / 3) * 3 + it / 3, (j / 3) * 3 + it % 3, value) -> return false
            }
        }

        return true
    }

    private fun updateConstraints(i: Int, j: Int, value: Int): Boolean {

        val cell = cells[i][j]
        when {
            cell.constraints[value] -> return true
            cell.value == value -> return false
        }

        cell.constraints[value] = true
        cell.numPossibilities--
        when {
            cell.numPossibilities > 1 -> return true
        }

        for (it in 1..9) {
            when {
                !cell.constraints[it] -> return set(i, j, it)
            }
        }

        return true
    }

    private fun findValuesForEmptyCells(): Boolean {

        emptyCells = mutableListOf()

        for (i in 0..8) {
            for (j in 0..8) {
                when {
                    cells[i][j].value == 0 -> emptyCells.add(Pair(i, j))
                }
            }
        }

        emptyCells.sortBy { cells[it.first][it.second].numPossibilities }

        return backtrack(0)
    }

    private fun backtrack(index: Int): Boolean {

        when {
            index >= emptyCells.size -> return true
            cells[emptyCells[index].first][emptyCells[index].second].value != 0 -> return backtrack(index + 1)
        }

        val snapshot = copy(cells)

        for (value in 1..9) {
            when {
                !cells[emptyCells[index].first][emptyCells[index].second].constraints[value] -> {
                    when {
                        set(emptyCells[index].first, emptyCells[index].second, value) && backtrack(index + 1) -> return true
                    }
                    cells = snapshot
                }
            }
        }

        return false
    }

    private fun copy(cells: Array<Array<Cell>>): Array<Array<Cell>> {

        val byteArrayOutputStream = ByteArrayOutputStream()
        val objectOutputStream = ObjectOutputStream(byteArrayOutputStream)
        objectOutputStream.writeObject(cells)

        val byteArrayInputStream = ByteArrayInputStream(byteArrayOutputStream.toByteArray())
        val objectInputStream = ObjectInputStream(byteArrayInputStream)

        @Suppress("UNCHECKED_CAST")
        return objectInputStream.readObject() as Array<Array<Cell>>
    }
}
```