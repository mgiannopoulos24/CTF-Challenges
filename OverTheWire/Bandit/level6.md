Since the password for `bandit7` is **somewhere** on the server you need to find it. Using the `find` command from the previous level and the `size` flag we need to make a command that also reads the user that owns the file we need and the group that this user is in. For this use the flags `user` and `group` as well. Also dont forget to use the `type` since we are looking for a file. Now the command we have formed so far applying all the flags is `find / -type f -user bandit7 -group bandit6 -size 33c` (The `/` means it will search the entire server). 
```console
bandit6@bandit:~$ find / -type f -user bandit7 -group bandit6 -size 33c
find: ‘/etc/ssl/private’: Permission denied
find: ‘/etc/polkit-1/localauthority’: Permission denied
find: ‘/etc/sudoers.d’: Permission denied
find: ‘/etc/multipath’: Permission denied
find: ‘/root’: Permission denied
... (more files)
... (more files)
... (more files)
... (more files)
find: ‘/run/systemd/inaccessible/dir’: Permission denied
find: ‘/run/lock/lvm’: Permission denied
```
Executing this command will give us the answer but not only the file we need. It will also print other files that we dont have permissions to read. These errors can be filtered simply by redirecting standard error (file descriptor 2) to `/dev/null` (which is like a trash bin that discards everything sent to it). You can send the errors using `>` to show the destination. So now the new command we have is `find / -type f -user bandit7 -group bandit6 -size 33c 2> /dev/null`. 
```console
bandit6@bandit:~$ find / -type f -user bandit7 -group bandit6 -size 33c 2> /dev/null
/var/lib/dpkg/info/bandit7.password
```
This will print exactly 1 line. Copy the path and use the `cat` command followed by the path you copied and you will get the password for the next level.
```console
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
```