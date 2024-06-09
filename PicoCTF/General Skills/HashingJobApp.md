### Challenge: HashingJobApp
### Points: 100

The author of this challenge asks us to connect with netcat to an IP and Port so lets do it.
```bash
User@Github:~$ nc saturn.picoctf.net 59495
Please md5 hash the text between quotes, excluding the quotes: 'Cinco de Mayo'
Answer:
```

For this first question we can use the md5sum tool on bash. So lets open a new terminal and do it:
```bash
User@Github:~$ echo -n "Cinco de Mayo" | md5sum
3b55fa8f34aae6b045ccec7f9b5d4c89  -
```
Now lets enter it and see what happens.
```bash
Please md5 hash the text between quotes, excluding the quotes: 'Cinco de Mayo'
Answer:
3b55fa8f34aae6b045ccec7f9b5d4c89
3b55fa8f34aae6b045ccec7f9b5d4c89
Correct.
```
It also printed a new question.
```bash
Please md5 hash the text between quotes, excluding the quotes: 'computer hackers'
Answer:
```
Again same procedure on our second terminal:
```bash
User@Github:~$ echo -n "computer hackers" | md5sum
1034abc8025edcc22f58c35abc21e36f  
```
And we enter it on nc:
```bash
Please md5 hash the text between quotes, excluding the quotes: 'computer hackers'
Answer:
1034abc8025edcc22f58c35abc21e36f
1034abc8025edcc22f58c35abc21e36f
Correct.
```
Another question appeared.
```bash
Please md5 hash the text between quotes, excluding the quotes: 'Cleopatra'
Answer:
```
Again we use our second terminal.
```bash
User@Github:~$ echo -n "Cleopatra" | md5sum
f8cbe5a99675fff11ed4d83fc16e2071 
```
We enter on the first terminal:
```bash
Please md5 hash the text between quotes, excluding the quotes: 'Cleopatra'
Answer:
f8cbe5a99675fff11ed4d83fc16e2071
f8cbe5a99675fff11ed4d83fc16e2071
Correct.
```
Finally it prints the flag:
```bash
picoCTF{..........._........_........}
```