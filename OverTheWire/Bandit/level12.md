Use the ```ls``` command to see the contents of ```bandit12```. As shown, there is only one file, ```data.txt```.  

Then, use the `cat` command to read it. As it is a Hex Dump of a file that was repeatedly compressed using different methods, the goal is to retrieve the original file from it.  

You should now create a temporary file using the command ```mktemp -d``` (using it will show the name of the temporary directory for you to keep), where you will copy the file using the ```cp``` command. Finally, go to that directory with the command ```cd```. This will be your working directory.  

Use the ```xxd``` command to reconstruct the binary file from the hex dump in the following manner:
```console
xxd -r data.txt reconstructed_file
```
In this way, a file named precisely ```reconstructed_file``` has been created, which contains the original binary data.  

Then, use the ```file``` command to determine the compression format of the reconstructed file. The output should indicate the file type, such as:
    - gzip compressed data
    - bzip2 compressed data
    - etc.

Next, decompress the file accordingly, based on its compression format. This can be performed by renaming the file to one with the appropriate extension and then using the proper decompression command.

Take the following example, which is also the first step of this particular challenge for decompression:
```console
bandit12@bandit:/tmp/[...] file reconstructed_file
reconstructed_file: gzip compressed data, was "data2.bin", last modified: [...]
bandit12@bandit:/tmp/[...] mv reconstructed_file data4.gz
bandit12@bandit:/tmp/[...] gzip -d data4.gz
```
The result of this decompression is a file, whose type you can check by using the command ```file``` once more. Repeat the process as many times as needed. The file will not need any more decompressions when it is of the type ```ASCII```.