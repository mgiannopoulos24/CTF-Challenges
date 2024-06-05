### Challenge: fixme2.py
### Points: 100

The author of this challenge provides us a python script. Let's run it and see what it does:
```bash
User@Github:~$ python3 fixme2.py 
  File "PicoCTF-Challenges/General Skills/fixme2.py/fixme2.py", line 22
    if flag = "":
       ^^^^^^^^^
SyntaxError: invalid syntax. Maybe you meant '==' or ':=' instead of '='?
```
It seems there's an error in line 22. 
Let's fix the syntax as the bash says. 
With that small fix line 22 should look like this:
```python
if flag == "":
```
Now time to run it again:
```bash
User@Github:~$ python3 fixme2.py 
That is correct! Here's your flag: picoCTF{........_..._.........._........}
```