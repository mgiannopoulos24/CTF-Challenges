### Challenge: 2Warm
### Points: 50
The author of this challenge asks you `Can you convert the number 42 (base 10) to binary (base 2)?`.
In order to solve this we need to convert the decimal number to binary using `echo` and the `bc` which stands for bash calculator.

**Note**: If your distribution doesnt have bash calculator installed, you can install it by typing `sudo apt-get install bc`.

```bash
User@Github:~$ echo "obase=2; 42" | bc
101010
```