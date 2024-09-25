Use the `ls` command to see the contents of `bandit7`. We can see that we only have a `data.txt` file. Use the `cat` command to read the `data.txt` file. In order to filter the contents of the `data.txt` we need to use the `grep` command. There are 2 ways to filter the contents.

- First way: Using a pipe `|`. We can read the `data.txt` with the `cat` command and then send the results of this command through a pipe `|` to a `grep` with the word `"millionth"`. So our command will be `cat data.txt | grep "millionth"`.
```console
bandit7@bandit:~$ cat data.txt | grep "millionth"
```
- Second way: Use directly the `grep` command with the word `"millionth"` on the `data.txt`. The command should be `grep "millionth" data.txt`.
```console
bandit7@bandit:~$ grep "millionth" data.txt
```