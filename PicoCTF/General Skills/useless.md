### Challenge: useless
### Points: 100

The author of this challenge asks you to `ssh` to `saturn.picoctf.net` on a given port, for me it was `51252` as user `picoplayer` and with the password `password`. You can do that by typing the following:

```bash
User@Github:~$ ssh picoplayer@saturn.picoctf.net -p 51252
The authenticity of host '[saturn.picoctf.net]:51252 ([13.59.203.175]:51252)' can't be established.
ED25519 key fingerprint is SHA256:DiJcS90U9QussLS8HLR6l6BGJb5eCA0vRmA18IvDvw8.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:6: [hashed name]
    ~/.ssh/known_hosts:21: [hashed name]
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[saturn.picoctf.net]:51252' (ED25519) to the list of known hosts.
picoplayer@saturn.picoctf.net's password:
Welcome to Ubuntu 20.04.6 LTS (GNU/Linux 6.5.0-1016-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.
```
We have the directories so we have to use `ls -al` or `ll`.
```bash
picoplayer@challenge:~$ ll
total 16
drwxr-xr-x 1 picoplayer picoplayer   20 May 12 17:51 ./
drwxr-xr-x 1 root       root         24 Aug  4  2023 ../
-rw-r--r-- 1 picoplayer picoplayer  220 Feb 25  2020 .bash_logout
-rw-r--r-- 1 picoplayer picoplayer 3771 Feb 25  2020 .bashrc
drwx------ 2 picoplayer picoplayer   34 May 12 17:51 .cache/
-rw-r--r-- 1 picoplayer picoplayer  807 Feb 25  2020 .profile
-rwxr-xr-x 1 root       root        517 Mar 16  2023 useless*
```
We have an executable named `useless`. According to the description this script performs basic calculations. 
Firstly let's run it.
```bash
picoplayer@challenge:~$ ./useless
Read the code first
```
We should read it to see the execution syntax.
```bash
picoplayer@challenge:~$ cat useless
#!/bin/bash
# Basic mathematical operations via command-line arguments

if [ $# != 3 ]
then
  echo "Read the code first"
else
        if [[ "$1" == "add" ]]
        then
          sum=$(( $2 + $3 ))
          echo "The Sum is: $sum"

        elif [[ "$1" == "sub" ]]
        then
          sub=$(( $2 - $3 ))
          echo "The Substract is: $sub"

        elif [[ "$1" == "div" ]]
        then
          div=$(( $2 / $3 ))
          echo "The quotient is: $div"

        elif [[ "$1" == "mul" ]]
        then
          mul=$(( $2 * $3 ))
          echo "The product is: $mul"

        else
          echo "Read the manual"

        fi
fi
```
After reading it we can see that the syntax is `./useless <operation> <num_1> <num_2>`.
We can try a few operations to see how it responds:
```bash
picoplayer@challenge:~$ ./useless mul 1 2
The product is: 2
picoplayer@challenge:~$ ./useless add 5 8
The Sum is: 13
picoplayer@challenge:~$ ./useless sub 9 2
The Substract is: 7
picoplayer@challenge:~$ ./useless div 9 2
The quotient is: 4
```
Nothing interesting here. Time to try something different.
```bash
picoplayer@challenge:~$ ./useless echo "hello" "world"
Read the manual
```
The program suggests to read the manual. Lets find the manual entry for `useless`:
```bash
picoplayer@challenge:~$ man useless

useless
     useless, — This is a simple calculator script

SYNOPSIS
     useless, [add sub mul div] number1 number2

DESCRIPTION
     Use the useless, macro to make simple calulations like addition,subtraction, multiplication and division.

Examples
     ./useless add 1 2
       This will add 1 and 2 and return 3

     ./useless mul 2 3
       This will return 6 as a product of 2 and 3

     ./useless div 6 3
       This will return 2 as a quotient of 6 and 3

     ./useless sub 6 5
       This will return 1 as a remainder of substraction of 5 from 6

Authors
     This script was designed and developed by Cylab Africa

     picoCTF{......._........._........._....}
```
We can see that on the bottom of the manual entry it has our flag.