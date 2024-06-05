### Challenge: Magikarp Ground Mission
### Points: 30
The author of this challenge asks you to ssh as ctf-player to venus.picoctf.net at given port. In order to do that you need to type on your terminal e.g. `ssh ctf-player@venus.picoctf.net -p 49532`. You will be prompted to access a fingerprint. Type yes. And then you will need to enter a password that is on the description of the challenge. 

```bash
User@Github:~$ ssh ctf-player@venus.picoctf.net -p 49532
The authenticity of host '[venus.picoctf.net]:49532 ([3.131.124.143]:49532)' can't be established.
ED25519 key fingerprint is SHA256:P1f6h95BrSVnJbm2AKhphfHHGEyAeThib/rN/AwKs24.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[venus.picoctf.net]:49532' (ED25519) to the list of known hosts.
ctf-player@venus.picoctf.net's password:
Welcome to Ubuntu 18.04.5 LTS (GNU/Linux 5.4.0-1041-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage
This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

ctf-player@pico-chall$
```

After connecting you need to list the files in the directory you are. You can do that by typing `ls`. 

```bash
ctf-player@pico-chall$ ls
1of3.flag.txt  instructions-to-2of3.txt
```


This will show you 2 txt files. In order to read them you need to use the `cat` command. Read the one that says 1of3 and you will get the first part of the flag. 

```bash
ctf-player@pico-chall$ cat 1of3.flag.txt
picoCTF{...._
```

Then read the instructions txt. It will print a message saying that you need to cd to the root file of the shell. 
```bash
ctf-player@pico-chall$ cat instructions-to-2of3.txt
Next, go to the root of all things, more succinctly `/`
```
You can do that by typing `cd /`. Then list all the contents of the directory with `ls`. 
```bash
ctf-player@pico-chall$ cd /
ctf-player@pico-chall$ ls
2of3.flag.txt  boot  etc   instructions-to-3of3.txt  lib64  mnt  proc  run   srv  tmp  var
bin            dev   home  lib                       media  opt  root  sbin  sys  usr
```
You will see another txt with 2of3. Read that with `cat` and you will get the second part of the flag.
```bash
ctf-player@pico-chall$ cat 2of3.flag.txt
..._.._\/\/...._
```
Then read the other instructions txt. It will print a message saying that you need to cd to the ome directory. 
```bash
ctf-player@pico-chall$ cat instructions-to-3of3.txt
Lastly, ctf-player, go home... more succinctly `~`
```
You can do that by typing `cd ~`. Then list again the contents of the directory and you will see a txt with 3of3. 
```bash
ctf-player@pico-chall$ cd ~
ctf-player@pico-chall$ ls
3of3.flag.txt  drop-in
```
Read the final txt and you will get the final part of the flag.
```bash
ctf-player@pico-chall$ cat 3of3.flag.txt
........}
```

