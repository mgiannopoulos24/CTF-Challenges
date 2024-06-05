### Challenge: Glitch Cat
### Points: 100

The author of this challenge asks you to nc (netcat) on host `saturn.picoctf.net` at the given port. So you should type in your terminal something looking like this  `nc saturn.picoctf.net 55899`.

```bash
User@Github:~$ nc saturn.picoctf.net 55899
'picoCTF{......_.._..._' + chr(0x62) + chr(0x64) + chr(0x61) + chr(0x36) + chr(0x38) + chr(0x66) + chr(0x37) + chr(0x35) + '}'
```
This has given us part of the flag but the following characters `chr(0x62)`, `chr(0x64)`, `chr(0x61)`, `chr(0x36)`, `chr(0x38)`, `chr(0x66)`, `chr(0x37)`, `chr(0x35)` are python expressions that evaluate to characters using their ASCII values.
We can solve this by using printf in the terminal:
```bash
printf "picoCTF{gl17ch_m3_n07_$(printf "\\x62\\x64\\x61\\x36\\x38\\x66\\x37\\x35")}\n"
picoCTF{......_.._..._........}
```
Explanation:
- `printf "\\x62\\x64\\x61\\x36\\x38\\x66\\x37\\x35"`: This inner printf command prints characters corresponding to the given ASCII values (`0x62`, `0x64`, `0x61`, `0x36`, `0x38`, `0x66`, `0x37`, `0x35`).
- `$(...)`: This is command substitution. It allows the output of the inner `printf` command to be used as part of the outer `printf` command.
- `printf "picoCTF{......_.._..._$(...)}"`: This outer `printf` command formats the entire flag string, including the evaluated ASCII characters.