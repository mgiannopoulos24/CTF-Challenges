![img](https://i.imgur.com/Xw2DVAT.png)

<img src='https://i.imgur.com/heKOInX.png' style='zoom: 80%;' align=left /> <font size='10'>Graverobber</font>

23<sup>rd</sup> April 2025

Prepared By: [deathwish24](https://app.hackthebox.com/users/2024290)

Challenge Author(s): [clubby789](https://app.hackthebox.com/users/83743)

Difficulty: <font color='lightgreen'>Very Easy</font> 

<br><br><br><br><br><br>

# Synopsis

- Analyze the provided `robber` binary, identify that it stores the flag components as ASCII values in an array, and extract the flag by directly interpreting these values, bypassing the binary's file checking logic.

## Skills Required

- Basic Reverse Engineering (using a decompiler)
- Understanding C code
- Knowledge of ASCII encoding

## Skills Learned

- Extracting embedded data from binaries.
- Bypassing unnecessary program logic to retrieve information directly.

# Enumeration

## Analyzing the binary

- We are given a binary executable named `robber`.

Upon decompiling the `robber` binary, we analyze the `main` function:

1.  An array named `parts` holds a sequence of integer values.
2.  The program iterates through the `parts` array, treating each value as a character. It constructs file paths by alternating these characters with `/` separators (e.g., `H/T/B/{/b/r/...`).
3.  It uses the `stat` system call to check if each constructed path exists as a file.
4.  If any `stat` call fails (file doesn't exist), it prints "We took a wrong turning!" and exits.
5.  If all `stat` calls succeed, it prints "We found the treasure! (I hope it's not cursed)" but doesn't reveal the flag.

The `parts` array contains the following integer values:

```c
_DWORD parts[32] = {
    72,  84,  66, 123, 98,  114, 51,  52,
    107, 49,  110, 57,  95,  100, 48,  119,
    110, 95,  116, 104, 51,  95,  115, 121,
    115, 99,  52,  108, 108, 53,  125, 0
};
```

# Solution

## Finding the vulnerability

The core "vulnerability" or rather, the key to the solution, lies in recognizing that the `parts` array directly contains the ASCII representation of the flag. The file checking mechanism is a distraction. By converting the integer values in the `parts` array to their corresponding ASCII characters, we can reconstruct the flag without needing to satisfy the file existence checks.

ASCII conversion of the `parts` array:
```console
72  -> H
84  -> T
66  -> B
123 -> {
...
125 -> }
0   -> NULL terminator
```
Resulting string: `HTB{...}`

## Exploitation

Instead of creating the dummy files required by the `robber` binary, we can write a simple C program to directly print the characters represented by the values in the `parts` array.

The exploit code (`solve.c`):

````c
#include <stdio.h>

int main() {
    // The array directly copied from the decompiled binary
    const char parts[32] = {
        72,  84,  66, 123, 98,  114, 51,  52,
        107, 49,  110, 57,  95,  100, 48,  119,
        110, 95,  116, 104, 51,  95,  115, 121,
        115, 99,  52,  108, 108, 53,  125, 0 // Null terminator included
    };

    // Print the string formed by the ASCII values
    printf("Treasure found: %s\n", parts);

    return 0;
}
````

### Getting the flag

A final summary:

1. Identified the parts array in the robber binary containing ASCII values.
2. Created a separate program (solve.c) to interpret and print the string represented by these values.

Compile and run the exploit:
```bash
gcc solve.c -o solve
./solve
```

This produces the output:
```console
Treasure found: HTB{...}
```