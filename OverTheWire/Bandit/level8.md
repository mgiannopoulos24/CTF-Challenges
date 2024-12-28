Use the `ls` command to list the contents of `bandit8`. We can see that there's a `data.txt` file.
```console
bandit8@bandit:~$ ls
data.txt
```
If we read that text file we will see the following:
```console
bandit8@bandit:~$ cat data.txt
aMKlTMrptUxxTypCHocCTrqYRkR2gT8h
PRerp5EfTVxJHKuCZDXfAfRyCQSdPjMi
0BKVRLEJQcpNx8wnSPxDLFnFKlQafKK6
6Boy6esAjnIxCYn8uI6KZ7VD7zysDM8i
tgHSfEXcbYCejWXfsWDO4VXXbqtTVcqS
.
.
.
sBDaWzvCbXUiXcP9to4j8o716bXI0inx
dPk8jhZUckmUiDsn4fXE28LpV5VTvev7
CgUjZiluCoMEvzNAge1Nbv3g9tpLQQj2
UuNP4xguSOjcTHAzdtHBgm2eNz1Z5133
```
The description of the level wants us to find the string that occurs **only once**. 
We can find it using the `uniq` command but first we need to group the strings with more occurences together.

We will use the `sort` command. 
Sorting the file will have an output like this:
```console
bandit8@bandit:~$ sort data.txt
0BKVRLEJQcpNx8wnSPxDLFnFKlQafKK6
0BKVRLEJQcpNx8wnSPxDLFnFKlQafKK6
0BKVRLEJQcpNx8wnSPxDLFnFKlQafKK6
0BKVRLEJQcpNx8wnSPxDLFnFKlQafKK6
0BKVRLEJQcpNx8wnSPxDLFnFKlQafKK6
.
.
.
zokSjnkcDj1hdGEBE4feukfCtFmv82ZZ
zokSjnkcDj1hdGEBE4feukfCtFmv82ZZ
zokSjnkcDj1hdGEBE4feukfCtFmv82ZZ
zokSjnkcDj1hdGEBE4feukfCtFmv82ZZ
zokSjnkcDj1hdGEBE4feukfCtFmv82ZZ
```
Now that it is sorted we need to redirect the output of `sort` as input to `uniq` with the `-u` option (which will only print unique lines) using a pipe `|`.
```
bandit8@bandit:~$ sort data.txt | uniq -u
```
Doing so we get only one string which is the password for the next level.

