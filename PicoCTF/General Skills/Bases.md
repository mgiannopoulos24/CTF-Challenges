### Challenge: Bases
### Points: 100

The author of this challenge provides us a string. The string is `bDNhcm5fdGgzX3IwcDM1`. It looks like a base64 encoded string but just to be sure lets check if the length of the string is a multiple of 4. We will use the `wc` command with the `-c` flag that counts the characters in combination the `printf` command.

```bash
User@Github:~$ printf "bDNhcm5fdGgzX3IwcDM1" | wc -c
20
```
The length is indeed a multiple of 4. So now lets try decoding it using `base64` and the `-d` flag:
```bash
User@Github:~$ printf "bDNhcm5fdGgzX3IwcDM1" | base64 -d
....._..._.....
```
This is our flag. Dont forget to wrap it in `picoCTF{...}`.