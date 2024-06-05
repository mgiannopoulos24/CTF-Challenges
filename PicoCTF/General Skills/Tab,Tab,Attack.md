### Challenge: Tab, Tab, Attack
### Points: 20
The author of this challenge provides a zip folder. In order to unzip this you need to install unzip using `sudo apt install unzip`.

After installing it you need to unzip the folder using `unzip Addadshashanammu.zip`.
```bash
User@Github:~$ unzip Addadshashanammu.zip 
Archive:  Addadshashanammu.zip
   creating: Addadshashanammu/
   creating: Addadshashanammu/Almurbalarammi/
   creating: Addadshashanammu/Almurbalarammi/Ashalmimilkala/
   creating: Addadshashanammu/Almurbalarammi/Ashalmimilkala/Assurnabitashpi/
   creating: Addadshashanammu/Almurbalarammi/Ashalmimilkala/Assurnabitashpi/Maelkashishi/
   creating: Addadshashanammu/Almurbalarammi/Ashalmimilkala/Assurnabitashpi/Maelkashishi/Onnissiralis/
   creating: Addadshashanammu/Almurbalarammi/Ashalmimilkala/Assurnabitashpi/Maelkashishi/Onnissiralis/Ularradallaku/
  inflating: Addadshashanammu/Almurbalarammi/Ashalmimilkala/Assurnabitashpi/Maelkashishi/Onnissiralis/Ularradallaku/fang-of-haynekhtnamet 
```
This will extract a bunch of files that are stored one inside another. You have to `cd` inside each folder until you find an executable named `fang-of-haynekhtnamet`. To be more efficient you can `cd` to the first directory and then type `cd` and use Tab to complete the full path without typing.
```bash
User@Github:~$ cd Addadshashanammu/
User@Github:~$ cd Almurbalarammi/Ashalmimilkala/Assurnabitashpi/Maelkashishi/Onnissiralis/Ularradallaku
```
Now lets ensure that we have the executable on the path we are.
```bash
User@Github:~$ ls
fang-of-haynekhtnamet
```
Run the executable and it will print a message with the flag.
```bash
User@Github:~$ ./fang-of-haynekhtnamet 
*ZAP!* picoCTF{....._..._...._._....._........}
```