### Challenge: Commitment Issues
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
TOP SECRET
```
Hm. It doesnt seem to show something we need. 
Lets look at the hints.
- Version control can help you recover files if you change or lose them!

That means we have to see the previous versions of the `message.txt` using `git` commands. In order to do that we need to use `git init` to reinitialize the repository.

```bash
User@Github:~$ git init
Reinitialized existing Git repository in PicoCTF-Challenges/General Skills/Commitment Issues/drop-in/.git/
```
Now lets use `git log` to see the commit history.
```bash
User@Github:~$ git log
commit a6dca68e4310585eac3b5c9caf0f75967dfe972c (HEAD -> master)
Author: picoCTF <ops@picoctf.com>
Date:   Sat Mar 9 21:10:06 2024 +0000

    remove sensitive info

commit e720dc26a1a55405fbdf4d338d465335c439fb3e
Author: picoCTF <ops@picoctf.com>
Date:   Sat Mar 9 21:10:06 2024 +0000

    create flag
```
We can see that the second commit is the one that contains the flag. For that reason we can use `git checkout` to check both of these commits.
```bash
User@Github:~$ git checkout a6dca68e4310585eac3b5c9caf0f75967dfe972c
Note: switching to 'a6dca68e4310585eac3b5c9caf0f75967dfe972c'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at a6dca68 remove sensitive info
```
Now that we changed to the first commit lets see if something changed in the directory.
```bash
User@Github:~$ ls
message.txt
```
We have the same file. Time to read it.
```bash
User@Github:~$ cat message.txt 
TOP SECRET
```
It seems the first commit has no changes to the file. Time to swap to the second commit now.
```bash
User@Github:~$ git checkout e720dc26a1a55405fbdf4d338d465335c439fb3e
Previous HEAD position was a6dca68 remove sensitive info
HEAD is now at e720dc2 create flag
```
Again we check for any differences in the directory.
```bash
User@Github:~$ ls
message.txt
```
And read it again.
```bash
User@Github:~$ cat message.txt 
picoCTF{........_........}
```
And we have our flag!

Theres also another way to solve this challenge way faster using `git show message.txt` which shows exactly the differences between all the versions of a file.

```bash
User@Github:~$ git show message.txt
commit e720dc26a1a55405fbdf4d338d465335c439fb3e (HEAD)
Author: picoCTF <ops@picoctf.com>
Date:   Sat Mar 9 21:10:06 2024 +0000

    create flag

diff --git a/message.txt b/message.txt
new file mode 100644
index 0000000..d263841
--- /dev/null
+++ b/message.txt
@@ -0,0 +1 @@
picoCTF{........_........}
```