### Challenge: Serpentine
### Points: 100

The author of this challenge provides a python script. 

Lets run it and see what happens:

```bash
User@Github:~$ python3 serpentine.py 

    Y
  .-^-.
 /     \      .- ~ ~ -.
()     ()    /   _ _   `.                     _ _ _
 \_   _/    /  /     \   \                . ~  _ _  ~ .
   | |     /  /       \   \             .' .~       ~-. `.
   | |    /  /         )   )           /  /             `.`.
   \ \_ _/  /         /   /           /  /                `'
    \_ _ _.'         /   /           (  (
                    /   /             \  \
                   /   /               \  \
                  /   /                 )  )
                 (   (                 /  /
                  `.  `.             .'  /
                    `.   ~ - - - - ~   .'
                       ~ . _ _ _ _ . ~

Welcome to the serpentine encourager!


a) Print encouragement
b) Print flag
c) Quit

What would you like to do? (a/b/c)
```
Since we want the flag we have to give as input the letter `b`.
```bash

What would you like to do? (a/b/c) b

Oops! I must have misplaced the print_flag function! Check my source code!


a) Print encouragement
b) Print flag
c) Quit
```
The program says that there's an error with the source code and a missplaced `print_flag` function.

After examining the python script we can see the flag function that we need.

```python

def print_flag():
  flag = str_xor(flag_enc, 'enkidu')
  print(flag)

```

Now how can we fix the main function to give us the flag when the option `b` is chosen?
We should modify the current elif condition:
```python
elif choice == 'b':
    print('\nOops! I must have misplaced the print_flag function! Check my source code!\n\n')
```
In order to call the `print_flag` function.
We can do that by modifying it to this:
```python
elif choice == 'b':
      print_flag()
```
Now time to run the program again and choose `b` again:
```bash
User@Github:~$ python3 serpentine.py 

    Y
  .-^-.
 /     \      .- ~ ~ -.
()     ()    /   _ _   `.                     _ _ _
 \_   _/    /  /     \   \                . ~  _ _  ~ .
   | |     /  /       \   \             .' .~       ~-. `.
   | |    /  /         )   )           /  /             `.`.
   \ \_ _/  /         /   /           /  /                `'
    \_ _ _.'         /   /           (  (
                    /   /             \  \
                   /   /               \  \
                  /   /                 )  )
                 (   (                 /  /
                  `.  `.             .'  /
                    `.   ~ - - - - ~   .'
                       ~ . _ _ _ _ . ~

Welcome to the serpentine encourager!


a) Print encouragement
b) Print flag
c) Quit

What would you like to do? (a/b/c) b
picoCTF{..._...._...._........_........}
a) Print encouragement
b) Print flag
c) Quit
```
That was it. We found our flag!