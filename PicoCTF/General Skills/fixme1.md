### Challenge: fixme1.py
### Points: 100

The author of this challenge provides us a python script. Let's run it and see what it does:
```bash
User@Github:~$ python3 fixme1.py 
  File "PicoCTF-Challenges/General Skills/fixme1.py/fixme1.py", line 20
    print('That is correct! Here\'s your flag: ' + flag)
IndentationError: unexpected indent
```
It seems there's an error in line 20. 
```python
flag = str_xor(flag_enc, 'enkidu')
  print('That is correct! Here\'s your flag: ' + flag)
```
The print is not properly aligned so thats why it prints the error.
After removing that unwanted whitespace character in the front our last 2 lines should look like this:
```python
flag = str_xor(flag_enc, 'enkidu')
print('That is correct! Here\'s your flag: ' + flag)
```
Now let's run the program again:
```bash
User@Github:~$ python3 fixme1.py 
That is correct! Here's your flag: picoCTF{........._......_........}
```