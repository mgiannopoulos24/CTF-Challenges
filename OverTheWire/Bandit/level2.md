There are two ways to solve this level:
  1. First way: Use the `cat` command to read the file using the backslash key ("\") to escape every space.
  ```shell
  bandit2@bandit:~$ ls
spaces in this filename
bandit2@bandit:~$ cat spaces\ in\ this\ filename
aBZ0W5Em........................
  ```
  2. Second way: Use the `cat` command to read the file using the quotes (" ") to wrap the whole filename.
```shell
bandit2@bandit:~$ ls
spaces in this filename
bandit2@bandit:~$ cat "spaces in this filename"
aBZ0W5Em........................
```
