![img](https://i.imgur.com/Xw2DVAT.png)

<img src='https://i.imgur.com/heKOInX.png' style='zoom: 80%;' align=left /> <font size='10'>Photon Lockdown</font>

22<sup>nd</sup> April 2025

Prepared By: [deathwish24](https://app.hackthebox.com/users/2024290)

Challenge Author(s): [41fr3d0](https://app.hackthebox.com/users/459485)

Difficulty: <font color='green'>Very Easy</font> 
<br><br><br><br><br><br>


# Synopsis

- The user must unzip the provided file, identify a SquashFS filesystem image, mount it, and search within the mounted filesystem for a flag matching the `HTB{...}` pattern.

## Description
- We've located the adversary's location and must now secure access to their Optical Network Terminal to disable their internet connection. Fortunately, we've obtained a copy of the device's firmware, which is suspected to contain hardcoded credentials. Can you extract the password from it?

## Skills Required

- Basic Linux command line usage (`unzip`, `cd`, `ls`, `file`, `mkdir`, `mount`, `grep`)
- Understanding of file types
- Filesystem mounting

## Skills Learned

- Learn how to identify SquashFS filesystems.
- Learn how to mount a SquashFS filesystem image.
- Learn how to recursively search for text patterns within a directory structure using `grep`.

# Enumeration

## Analyzing the provided files

- After unzipping `Photon_Lockdown.zip` (password: `hackthebox`), we get a folder named `ONT`. Inside this folder, there are three files: `fwu_ver`, `hw_ver`, and `rootfs`.

```bash
┌──(kali㉿kali)-[~/Desktop]
└─$ cd ONT    
                                                                                                                                      
┌──(kali㉿kali)-[~/Desktop/ONT]
└─$ ls
fwu_ver  hw_ver  rootfs
```

Using the `file` command reveals the nature of these files:

```bash
fwu_ver:    ASCII text
hw_ver:     X1 archive data
rootfs:     Squashfs filesystem, little endian, version 4.0, zlib compressed, 10936182 bytes, 910 inodes, blocksize: 131072 bytes, created: Sun Oct  1 07:02:43 2023
```

The `rootfs` file is identified as a Squashfs filesystem.

# Solution

## Mounting the filesystem

To explore the contents of the `rootfs` filesystem, it needs to be mounted. First, create a directory to serve as the mount point, then use the `mount` command (requiring `sudo` privileges) to mount the image:

```bash
┌──(kali㉿kali)-[~/Desktop/ONT]
└─$ mkdir hackthebox 
                                                                                                                                      
┌──(kali㉿kali)-[~/Desktop/ONT]
└─$ sudo mount rootfs hackthebox 
```

## Exploitation

With the filesystem mounted, we can navigate into the mount point directory (`hackthebox`) to access its contents:

```bash
┌──(kali㉿kali)-[~/Desktop/ONT]
└─$ cd hackthebox 

──(kali㉿kali)-[~/Desktop/ONT/hackthebox]
└─$ ls
bin  config  dev  etc  home  image  lib  mnt  overlay  proc  run  sbin  sys  tmp  usr  var
```

### Getting the flag

The goal is to find a flag with the format `HTB{...}`. The `grep` command with the `-rnw` flags can be used to recursively (`r`) search for the pattern (`HTB`) within all files in the current directory (`.`), showing the line numbers (`n`) and matching whole words (`w`).

```bash
┌──(kali㉿kali)-[~/Desktop/ONT/hackthebox]
└─$ grep -rnw 'HTB'       
grep: bin/tc: binary file matches
# Redacted
```

Executing this command within the `hackthebox` directory will search the entire mounted filesystem and output the file(s) and line(s) containing the flag pattern.