### Challenge: convertme.py
### Points: 100

The author of this challenge gives us a python script. This script defines a function str_xor for performing bitwise XOR operation between two strings. It then initializes a variable flag_enc with a sequence of ASCII characters representing a flag encrypted using XOR.

In each execution, the script generates a random number between 10 and 100 and asks us to convert it from decimal to binary. If we provide the correct binary representation of the random number, it decrypts the encrypted flag using the XOR function and prints it out.

So lets try it:
```bash
User@Github:~$ python3 convertme.py 
If 18 is in decimal base, what is it in binary base?
```
Now in order to solve that we can use the following command on a new terminal:
```bash
echo "obase=2; <number>" | bc
```
In my case the number was 18, so I will replace the `<number>` with 18.
```bash
User@Github:~$ echo "obase=2; 18" | bc
10010
```
And now to type it on the terminal we executed the script:
```bash
If 18 is in decimal base, what is it in binary base?
Answer: 10010
That is correct! Here's your flag: picoCTF{..._...._....._........}
```
