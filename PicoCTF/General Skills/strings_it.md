### Challenge: strings it

The author of this challenge provides a file with a lot of strings. We need to read it and at the same time filter out only the flag. 
We can do it by using `strings` which is used to extract printable strings from a file and `grep` to search for specific pattern in a file. 
In order to do this at the same time we need to use the pipe `|` symbol. 
In Linux, the `|` symbol is called a pipe. It is used to redirect the output of one command as the input to another command.
So now we will pipe the output of the `strings` command to be the input of `grep`. 

```bash
User@Github:~$ strings strings | grep "picoCTF"
picoCTF{......._.._........}
```