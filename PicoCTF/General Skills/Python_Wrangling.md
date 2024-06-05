### Challenge: Python Wrangling
### Points: 10

The author of this challenge provides a python script, a password txt and a flag txt.

In order to solve this challenge we need to run the `ende.py` file using the `python` command (If you dont have Python on your PC, go to this [website](https://www.python.org/downloads/) and choose the version you want.).

```bash
User@Github:~$ python3 ende.py 
Usage: ende.py (-e/-d) [file]
```
After running the python script you can see that it needs an option and an argument to run.

You can examine the script and you will find out that the `-e` option that will give you the flag but encoded with base64. So the other option `-d` is the one we need. This will give us the decoded message. 

This time we will use the flag.txt.en as argument.

After running the python script for the second time with `python ende.py -d flag.txt.en`, you are prompted to give a password to continue.
```bash
User@Github:~$ python3 ende.py -d flag.txt.en
Please enter the password:
```
Press `Ctrl+C` to stop the program.

Lets read the password which is stored in the `pw.txt` and copy it.
```bash
User@Github:~$ cat pw.txt 
dbd1bea4dbd1bea4dbd1bea4dbd1bea4
```

Now lets run the python script again and paste the password when prompted:
```bash
User@Github:~$ python3 ende.py -d flag.txt.en
Please enter the password:dbd1bea4dbd1bea4dbd1bea4dbd1bea4
picoCTF{......_.._..._....._........}
```