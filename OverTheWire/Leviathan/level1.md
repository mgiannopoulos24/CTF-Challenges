We will start by listing the contents of the home directory.
```console
leviathan1@gibson:~$ ll
total 36
drwxr-xr-x  2 root       root        4096 Oct  5  2023 ./
drwxr-xr-x 83 root       root        4096 Oct  5  2023 ../
-rw-r--r--  1 root       root         220 Jan  6  2022 .bash_logout
-rw-r--r--  1 root       root        3771 Jan  6  2022 .bashrc
-r-sr-x---  1 leviathan2 leviathan1 15072 Oct  5  2023 check*
-rw-r--r--  1 root       root         807 Jan  6  2022 .profile
```
We can see an executable named `check`. Time to run it and see what happens.
```console
leviathan1@gibson:~$ ./check
password: 
```
It asks for a password. What could the password be?
I will go with `leviathan1` and see what happens next.
```console
leviathan1@gibson:~$ ./check
password: leviathan1
Wrong password, Good Bye ...
```
The password was wrong. 
So let's just make a copy of this executable on our local machine by converting it first to base64.

```console
leviathan1@gibson:~$ base64 check
```
Then paste the base64 characters on a file with `.base64` suffix and then decode it to binary with the name you want.
```console
User@Github:~$ nano check.base64
User@Github:~$ base64 -d check.base64 > check
```
Now that we have the executable we can use [DogBolt](https://dogbolt.org/) to decompile the program and see what we need to do.

```C
//----- (080491E6) --------------------------------------------------------
int __cdecl main(int argc, const char **argv, const char **envp)
{
  __uid_t v3; // ebx
  __uid_t v4; // eax
  char s1[4]; // [esp+0h] [ebp-24h] BYREF
  char s2[4]; // [esp+4h] [ebp-20h] BYREF
  int v8; // [esp+8h] [ebp-1Ch]
  char v9[12]; // [esp+Ch] [ebp-18h] BYREF
  unsigned int v10; // [esp+18h] [ebp-Ch]
  int *p_argc; // [esp+1Ch] [ebp-8h]

  p_argc = &argc;
  v10 = __readgsdword(0x14u);
  strcpy(s2, "sex");
  strcpy(&v9[5], "secret");
  v8 = 6582119;
  strcpy(v9, "love");
  printf("password: ");
  s1[0] = getchar();
  s1[1] = getchar();
  s1[2] = getchar();
  s1[3] = 0;
  if ( !strcmp(s1, s2) )
  {
    v3 = geteuid();
    v4 = geteuid();
    setreuid(v4, v3);
    system("/bin/sh");
  }
  else
  {
    puts("Wrong password, Good Bye ...");
  }
  return 0;
}
```
According to the decompiler he program prompts us to enter a three-character password and compares it to the predefined password "sex". If the entered password matches `sex`, the program grants shell access by invoking `/bin/sh`. The core logic involves reading three characters of input, storing them in a string, and checking this string against the predefined password.
```console
leviathan1@gibson:~$ ./check
password: sex
$
```
We got a new shell. Lets run `whoami`.
```console
$ whoami
leviathan2
```
We are `leviathan2`. Now we have permissions to read the flag of `leviathan2`. The passwords of `leviathan` are stored in `/etc/leviathan_pass/`.
```console
$ cat /etc/leviathan_pass/leviathan2
```
We found the password for the next level!


