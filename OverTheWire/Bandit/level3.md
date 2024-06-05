Use the `ls` command to list all the contents of `bandit3` and use the `cd` (change directory) command to access the directory named `inhere`. 
```bash
bandit3@bandit:~$ ls
inhere
bandit3@bandit:~$ cd inhere/
bandit3@bandit:~/inhere$
```
Then use `ls -la` or `ll` to see the contents of the `inhere` directory. 
```bash
bandit3@bandit:~/inhere$ ls -la
total 12
drwxr-xr-x 2 root    root    4096 Oct  5  2023 .
drwxr-xr-x 3 root    root    4096 Oct  5  2023 ..
-rw-r----- 1 bandit4 bandit3   33 Oct  5  2023 .hidden
```
Finally use the `cat` command followed by the `.hidden` file to read it.
```bash
bandit3@bandit:~/inhere$ cat .hidden
2EW7BBsr........................
```