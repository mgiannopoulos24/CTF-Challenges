### Challenge: Lets Warm Up
### Points: 50
The author of this challenge asks you `If I told you a word started with 0x70 in hexadecimal, what would it start with in ASCII?`. 
In order to solve that we need to convert this hexadecimal number to decimal using `printf` with the `%d` format specifier on your shell which is used to indicate that the corresponding argument should be formatted as a decimal integer.
```bash
User@Github:~$ printf "%d\n" 0x70
112
```

Then you need to convert this decimal number to its corresponding ASCII character. 
You can do that by typing  `printf "\\$(printf '%03o' 112)\n"` which will print the ASCII character corresponding to the decimal number 112, which is p, followed by a newline character.

```bash
User@Github:~$ printf "\\$(printf '%03o' 112)\n"
p
```