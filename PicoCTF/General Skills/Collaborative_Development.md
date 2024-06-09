### Challenge: Collaborative Development
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
flag.py
```
We can see there's a python script here. Let's see if it runs.
```bash
User@Github:~$ python3 flag.py 
Printing the flag...
```
It printed only that message. That means there must be a problem with it. 
Since we know that this was a collaborative project we should see the log of the git repository and find out the other versions of the `flag.py`. We can do that by doing `git log flag.py`.
```bash
User@Github:~$ git init
User@Github:~$ git log flag.py
commit 2258a0f267d57e8b6025e2a020b77fac7a553c92 (HEAD -> main)
Author: picoCTF <ops@picoctf.com>
Date:   Tue Mar 12 00:07:54 2024 +0000

    init flag printer
```
This branch doesnt seem to have anything useful so we need to see what other branches this repository has using `checkout -a`.
```bash
User@Github:~$ git branch -a
  feature/part-1
  feature/part-2
  feature/part-3
* main
```
We have to individually check each one so lets do it.
```bash
User@Github:~$ git log feature/part-1
commit 8eea0627726fc363246015cb4c7e927e70286e87 (feature/part-1)
Author: picoCTF <ops@picoctf.com>
Date:   Tue Mar 12 00:07:54 2024 +0000

    add part 1

commit 2258a0f267d57e8b6025e2a020b77fac7a553c92 (HEAD -> main)
Author: picoCTF <ops@picoctf.com>
Date:   Tue Mar 12 00:07:54 2024 +0000

    init flag printer
```
We found 2 commits in this branch. Lets check them 
```bash
User@Github:~$ git checkout 8eea0627726fc363246015cb4c7e927e70286e87
Note: switching to '8eea0627726fc363246015cb4c7e927e70286e87'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 8eea062 add part 1
```
Now that we switched to a previous version of the file lets run it.
```bash
User@Github:~$ ls
flag.py
User@Github:~$ python3 flag.py 
Printing the flag...
picoCTF{........_
```
It printed the first part of the flag. Now time to switch to the second branch.
```bash
User@Github:~$ git switch feature/part-2
Previous HEAD position was 8eea062 add part 1
Switched to branch 'feature/part-2'
User@Github:~$ ls
flag.py
```
And again run it.
```bash
User@Github:~$ python3 flag.py 
Printing the flag...
....._..._....._
```
That's the second part of the flag. Now time to find the final part on the third branch.
```bash
User@Github:~$ git switch feature/part-3
Switched to branch 'feature/part-3'
User@Github:~$ ls
flag.py
```
Run it again.
```bash
User@Github:~$ python3 flag.py 
Printing the flag...
...._........}
```
Now that we have all the parts of the flag we can assemble it.
The flag should look like this.

`picoCTF{........_....._..._....._...._........}`.

