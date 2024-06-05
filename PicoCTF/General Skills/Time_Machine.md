### Challenge: Time Machine
### Points: 50

The author of this challenge provides a zip folder. Lets unzip it using `unzip` to see the contents inside.
```bash
User@Github:~$ unzip challenge.zip
User@Github:~$ ls
challenge.zip  drop-in
```
We extracted a directory named `drop-in`. Lets navigate in that directory and see what we have.
```bash
User@Github:~$ cd drop-in/
User@Github:~$ ls
message.txt
```
We can see theres a txt file in there. Lets read it.
```bash
User@Github:~$ cat message.txt 
This is what I was working on, but I'd need to look at my commit history to know why...
```
Hm. It doesnt seem to show something we need. 
Lets look at the hints.
- When committing a file with git, a message can (and should) be included.

That means we have to see the commit history of `message.txt` using `git` commands. In order to do that we need to use `git init` to reinitialize the repository.

```bash
User@Github:~$ git init
Reinitialized existing Git repository in PicoCTF-Challenges/General Skills/Time Machine/drop-in/.git/
```
Now lets use `git log` to see the commit history.
```bash
User@Github:~$ git log
commit 712314f105348e295f8cadd7d7dc4e9fa871e9a2 (HEAD -> master)
Author: picoCTF <ops@picoctf.com>
Date:   Tue Mar 12 00:07:26 2024 +0000

    picoCTF{..........._........}
```
We found our flag!