### Challenge: Mod 26

The author of this challenge provides us a string that is encoded with [ROT13](https://en.wikipedia.org/wiki/ROT13). 
ROT13 is a simple letter substitution cipher that replaces a letter with the 13th letter after it in the Latin alphabet.
In order to solve this challenge you can use this [tool](https://rot13.com/) or use the `tr` command. The `tr` command in Linux is used for translating or deleting characters. Its name stands for "translate" and it's primarily used to translate characters from one set to another.
In the context of the ROT13 encryption example:
- The first set of characters 'A-Za-z' represents all uppercase and lowercase letters in the English alphabet.
- The second set of characters 'N-ZA-Mn-za-m' represents the corresponding characters in ROT13. In ROT13, each letter is replaced by the letter 13 positions ahead of it in the alphabet. 

For example, 'A' becomes 'N', 'B' becomes 'O', and so on.

Let's see it in action:
```bash
User@Github:~$ echo "cvpbPGS{arkg_gvzr_V'yy_gel_2_ebhaqf_bs_ebg13_GYpXOHqX}" | tr 'A-Za-z' 'N-ZA-Mn-za-m'
picoCTF{...._...._...._..._._......_.._....._........}
```