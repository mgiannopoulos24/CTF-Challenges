### Challenge: Nice netcat...
### Points: 15

The author of this challenge provides a netcat address. In order to connect there you need to type on your terminal `nc mercury.picoctf.net 22902` and after connecting you will receive a bunch of numbers. 
```bash
User@Github:~$ nc mercury.picoctf.net 22902
112
105
99
111
67
84
70
123
103
48
48
100
95
107
49
116
116
121
33
95
110
49
99
51
95
107
49
116
116
121
33
95
100
51
100
102
100
54
100
102
125
10
```
Each number represents an ASCII character. You can use this [tool](https://www.prepostseo.com/tool/decimal-to-ascii) to decode the decimal numbers to ASCII characters and you will get your flag.