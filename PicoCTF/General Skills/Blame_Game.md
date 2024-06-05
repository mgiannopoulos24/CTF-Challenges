### Challenge: Blame Game
### Points: 75

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
message.py
```
We can see theres a python file in there.Lets read it.
```bash
User@Github:~$ cat message.py 
print("Hello, World!"
```
Hm. It doesnt seem to show something we need. 
Lets look at the hints.
- In collaborative projects, many users can make many changes. How can you see the changes within one file?

That means we have to see the commit history of `message.py` using `git` commands. In order to do that we need to use `git init` to reinitialize the repository.
```bash
User@Github:~$ git init
Reinitialized existing Git repository in PicoCTF-Challenges/General Skills/Blame Game/drop-in/.git/
```
Now lets use `git log message.py` to see the commit history on this specific file.
```bash
User@Github:~$ git log message.py
commit 23e9d4ce78b3cea725992a0ce6f5eea0bf0bcdd4
Author: picoCTF{..._..._......_........} <ops@picoctf.com>
Date:   Tue Mar 12 00:07:15 2024 +0000

    optimize file size of prod code

commit 3ce5c692e2f9682a866c59ac1aeae38d35d19771
Author: picoCTF <ops@picoctf.com>
Date:   Tue Mar 12 00:07:15 2024 +0000

    create top secret project
```
And that was it. We found the flag.