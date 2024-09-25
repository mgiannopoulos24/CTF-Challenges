Use the `ls` command to list all the contents of `bandit4` and use the `cd` (change directory) command to access the directory named `inhere`. 
```console
bandit4@bandit:~$ ls
inhere
bandit4@bandit:~$ cd inhere/
bandit4@bandit:~/inhere$
```
Then use `ls -la` or `ll` to see the contents of the `inhere` directory. 
```console
bandit4@bandit:~/inhere$ ll
total 48
drwxr-xr-x 2 root    root    4096 Oct  5  2023 ./
drwxr-xr-x 3 root    root    4096 Oct  5  2023 ../
-rw-r----- 1 bandit5 bandit4   33 Oct  5  2023 -file00
-rw-r----- 1 bandit5 bandit4   33 Oct  5  2023 -file01
-rw-r----- 1 bandit5 bandit4   33 Oct  5  2023 -file02
-rw-r----- 1 bandit5 bandit4   33 Oct  5  2023 -file03
-rw-r----- 1 bandit5 bandit4   33 Oct  5  2023 -file04
-rw-r----- 1 bandit5 bandit4   33 Oct  5  2023 -file05
-rw-r----- 1 bandit5 bandit4   33 Oct  5  2023 -file06
-rw-r----- 1 bandit5 bandit4   33 Oct  5  2023 -file07
-rw-r----- 1 bandit5 bandit4   33 Oct  5  2023 -file08
-rw-r----- 1 bandit5 bandit4   33 Oct  5  2023 -file09
```
As you can see there are 10 files in here. Since we need to find the one that is human-readable you have to use the `file` command followed by `./-file*` which will return the data type of each file. 
```console
bandit4@bandit:~/inhere$ file ./-file*
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data
```
After that you can see that the file `-file07` has ASCII text. 
Finally use the `cat` command followed by `./-file07` and to read it.
```console
bandit4@bandit:~/inhere$ cat ./-file07
```