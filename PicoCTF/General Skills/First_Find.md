### Challenge: First Find
### Points: 100

The author of this challenge provides a zip folder. Lets unzip it using `unzip` to see the contents inside.
```bash
User@Github:~$ unzip files.zip
User@Github:~$ ls
files  files.zip
```
Now we need to find a txt file named `uber-secret.txt`. For that reason we will use the `-name` flag of the `find` command on the `files` directory:
```bash
User@Github:~$ find files/ -name uber-secret.txt
files/adequate_books/more_books/.secret/deeper_secrets/deepest_secrets/uber-secret.txt
```
We found the path where the txt we need is located. So now lets just read it with the `cat` command:
```bash
User@Github:~$ cat files/adequate_books/more_books/.secret/deeper_secrets/deepest_secrets/uber-secret.txt
picoCTF{...._.._...._........}
```
