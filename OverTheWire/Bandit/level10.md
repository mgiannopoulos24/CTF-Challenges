Use the `ls` command to list the contents of `bandit10`. We can see that there's a `data.txt` file.
```console
bandit10@bandit:~$ ls
data.txt
```
If we read that text file we will see the following:
```console
bandit10@bandit:~$ cat data.txt
VGhlIHBhc3N3b3JkIGlzIGR0UjE3M2ZaS2IwUlJzREZTR3NnMlJXbnBOVmozcVJyCg==
```
That looks like a base64 encoded string. We can decode it using the `base64` command with the `-d` option (for decode).
```console
bandit10@bandit:~$ cat data.txt | base64 -d
The password is [REDACTED]
```
And that is the password for the next level.