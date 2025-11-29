![img](https://i.imgur.com/Xw2DVAT.png)

<img src='https://i.imgur.com/heKOInX.png' style='zoom: 80%;' align=left /> <font size='10'>Sudoking</font>

29<sup>th</sup> November 2025

Prepared By: [deathwish24](https://app.hackthebox.com/users/2024290)

Challenge Author(s): [MonkeyPrincess22](https://app.hackthebox.com/users/2172205)

Difficulty: <font color='green'>Easy</font> 
<br><br><br><br><br><br>


# Synopsis

- The user is presented with a standard 9x9 Sudoku puzzle via a text-based interface. The goal is to programmatically parse the input grid, solve the puzzle using a backtracking algorithm, and output the completed grid to retrieve the flag.

## Description
- Welcome to the SUDOku Tournament! Conquer all puzzles in record time to claim the title of SUDOking and rule the grid!

## Skills Required

- Programming
- Standard Input/Output (stdin/stdout) parsing
- Recursion and Backtracking algorithms
- Logic puzzle solving

## Skills Learned

- Implementing a recursive backtracking solver.
- Efficiently parsing ASCII-art based data structures.
- Array manipulation in C.

# Enumeration

## Analyzing the Input

The challenge provides the Sudoku puzzle in a specific ASCII format. Based on the provided code's parsing logic, the input looks like a visual grid with separators.

Example Input:
```text
+-------+-------+-------+
| . . 3 | . 2 . | 6 . . |
| 9 . . | 3 . 5 | . . 1 |
| . . 1 | 8 . 6 | 4 . . |
+-------+-------+-------+
| . . 8 | 1 . 2 | 9 . . |
| 7 . . | . . . | . . 8 |
| . . 6 | 7 . 8 | 2 . . |
+-------+-------+-------+
| . . 2 | 6 . 9 | 5 . . |
| 8 . . | 2 . 3 | . . 9 |
| . . 5 | . 1 . | 3 . . |
+-------+-------+-------+
```

Key observations:
1.  The grid contains `.` for empty cells.
2.  The grid is divided by `+-------+` lines and `|` characters.
3.  We need to strip these formatting characters to process the raw numerical data.

# Solution

## The Algorithm: Backtracking

To solve Sudoku programmatically, **Backtracking** is the standard approach. It works by:
1.  Finding the first empty cell (`.`).
2.  Trying numbers 1 through 9.
3.  Checking if the number is valid (doesn't exist in the current row, column, or 3x3 subgrid).
4.  If valid, place the number and recursively try to solve the rest of the grid.
5.  If the recursion fails (leads to a dead end), reset the cell to 0 (backtrack) and try the next number.

## The Code

I implemented the solution in **C** for performance.

### Parsing the Input
The `parse_input` function reads 13 lines from `stdin`. It ignores the horizontal separator lines (indices divisible by 4) and ignores the vertical separators (`|`). It converts `.` characters to `0` for internal processing.

### Validation
The `is_valid` function ensures the rules of Sudoku are met before placing a number:
```c
int is_valid(int grid[SIZE][SIZE], int row, int col, int num) {
    // Check Row and Column
    for (int i = 0; i < SIZE; i++) {
        if (grid[row][i] == num || grid[i][col] == num) return 0;
    }
    // Check 3x3 Subgrid
    int start_row = (row / 3) * 3, start_col = (col / 3) * 3;
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            if (grid[start_row + i][start_col + j] == num) return 0;
        }
    }
    return 1;
}
```

### Solving
The `solve` function implements the recursive backtracking logic:
```c
int solve(int grid[SIZE][SIZE]) {
    for (int row = 0; row < SIZE; row++) {
        for (int col = 0; col < SIZE; col++) {
            if (grid[row][col] == 0) { // Found empty cell
                for (int num = 1; num <= 9; num++) {
                    if (is_valid(grid, row, col, num)) {
                        grid[row][col] = num; // Place number
                        if (solve(grid)) return 1; // Success
                        grid[row][col] = 0; // Backtrack
                    }
                }
                return 0; // Trigger backtrack
            }
        }
    }
    return 1; // Grid full, solved
}
```

## Exploitation

We compile the C program:
```bash
gcc sudoking_solve.c -o sudoking_solve
```

Depending on how the challenge is hosted (netcat or file download), we pipe the puzzle into our solver. Assuming a remote connection:

```bash
nc <TARGET_IP> <PORT> | ./sudoking_solve
```

### Getting the Flag

The program parses the input, solves the grid instantly, and prints the completed board in the exact format required by the server.

Output:
```text
+-------+-------+-------+
| 4 8 3 | 9 2 1 | 6 5 7 |
| 9 6 7 | 3 4 5 | 8 2 1 |
| 2 5 1 | 8 7 6 | 4 9 3 |
+-------+-------+-------+
... (completed grid)
```

Upon receiving the correct solution, the server responds with the flag.
```