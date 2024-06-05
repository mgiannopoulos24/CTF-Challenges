### Challenge: Static ain't always noise
### Points: 20
The author of this challenge provides a bash script and a binary file. 

Lets executing the static first.

```bash
User@Github:~$ ./static
Oh hai! Wait what? A flag? Yes, it's around here somewhere!
```

Now lets try executing the bash script:
```bash
User@Github:~$ ./ltdis.sh 
Attempting disassembly of  ...
objdump: 'a.out': No such file
objdump: section '.text' mentioned in a -j option, but not found in any input file
Disassembly failed!
Usage: ltdis.sh <program-file>
Bye!
```
This will create the `.ltdis.x86_64.txt` and we also get a message saying that the correct way to run it is `Usage: ltdis.sh <program-file>`.
So now we can run it again with the static as the argument. `./ltdis.sh static`.
```bash
User@Github:~$ ./ltdis.sh static 
Attempting disassembly of static ...
Disassembly successful! Available at: static.ltdis.x86_64.txt
Ripping strings from binary with file offsets...
Any strings found in static have been written to static.ltdis.strings.txt with file offset
```
This will print another message and it will also produce 2 more txt files. 

In order to find the flag you need to filter out of the `static.ltdis.strings.txt` file the flag. You can do that by using `grep`.

```bash
User@Github:~$ grep "picoCTF" static.ltdis.strings.txt 
1020 picoCTF{......_......_........}
```
