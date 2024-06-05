We will start by listing the contents of the home directory.
```bash
leviathan0@gibson:~$ ll
total 24
drwxr-xr-x  3 root       root       4096 Oct  5  2023 ./
drwxr-xr-x 83 root       root       4096 Oct  5  2023 ../
drwxr-x---  2 leviathan1 leviathan0 4096 Oct  5  2023 .backup/
-rw-r--r--  1 root       root        220 Jan  6  2022 .bash_logout
-rw-r--r--  1 root       root       3771 Jan  6  2022 .bashrc
-rw-r--r--  1 root       root        807 Jan  6  2022 .profile
```
We can see that the `.backup` directory isn't something you usually find in a home directory. So let's navigate in it and list the contents.
```bash
leviathan0@gibson:~$ cd .backup/
leviathan0@gibson:~/.backup$ ll
total 140
drwxr-x--- 2 leviathan1 leviathan0   4096 Oct  5  2023 ./
drwxr-xr-x 3 root       root         4096 Oct  5  2023 ../
-rw-r----- 1 leviathan1 leviathan0 133259 Oct  5  2023 bookmarks.html
```
The `bookmarks.html` file looks very interesting. 
Lets read it.
```bash
leviathan0@gibson:~/.backup$ cat bookmarks.html
<!DOCTYPE NETSCAPE-Bookmark-file-1>
<!-- This is an automatically generated file.
     It will be read and overwritten.
     DO NOT EDIT! -->
<META HTTP-EQUIV="Content-Type" CONTENT="text/html; charset=UTF-8">
<TITLE>Bookmarks</TITLE>
<H1 LAST_MODIFIED="1160271046">Bookmarks</H1>

.
.
.
.
.

<DT><A HREF="http://www.warholstars.org/" ADD_DATE="1151503884" LAST_CHARSET="ISO-8859-1" ID="rdf:#$2wIU71">Warhol</A>
<DT><A HREF="http://www.x-rayspex.com/" ADD_DATE="1121479563" LAST_CHARSET="ISO-8859-1" ID="rdf:#$2wIU71">X-Ray Spex</A>


</DL><p>
```
We cant understand a lot from that. What if we filter the output with and look for something that contains the word `password`.
```bash
leviathan0@gibson:~/.backup$ cat bookmarks.html | grep "password"
<DT><A HREF="http://leviathan.labs.overthewire.org/passwordus.html | This will be fixed later, the password for leviathan1 is PPIfm....." ADD_DATE="1155384634" LAST_CHARSET="ISO-8859-1" ID="rdf:#$2wIU71">password to leviathan1</A>
```
It seems like the trick worked and we found the password!

`This will be fixed later, the password for leviathan1 is PPIfm.....`