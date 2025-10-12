Use the `ls` command to list the contents of `bandit11`. We can see that there's a `data.txt` file.
```console
bandit11@bandit:~$ ls
data.txt
```
If we read that text file we will see the following:
```
bandit11@bandit:~$ cat data.txt
Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4
```
Just by experience I can tell that this is encoded with the ROT13 cipher. But we can also understand that by reading the Useful Material section of the level description.

If we check the [ROT13 Wikipedia page](https://en.wikipedia.org/wiki/ROT13) we can see that it is a special case of the Caesar cipher where each letter is replaced by the letter 13 places down the alphabet and also there's a command on how to do it.

We can use the `tr` command to decode the text. The `tr` command is used to translate or delete characters.
We will use it to translate the letters from A-M to N-Z and from N-Z to A-M.
```console
bandit11@bandit:~$ cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
The password is [REDACTED]
```