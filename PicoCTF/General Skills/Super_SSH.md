### Challenge: Super SSH
### Points: 25
The author of this challenge asks you to ssh as ctf-player to titan.picoctf.net at at given port. In order to do that you need to type on your terminal a command looking like this `ssh ctf-player@titan.picoctf.net -p 51362`. You will be prompted to accept a fingerprint. Type yes. And then you will need to enter a password that is on the description of the challenge. After successful connection you will see a message with your flag.

```bash
User@Github:~$ ssh ctf-player@titan.picoctf.net -p 51362
The authenticity of host '[titan.picoctf.net]:51362 ([3.139.174.234]:51362)' can't be established.
ED25519 key fingerprint is SHA256:4S9EbTSSRZm32I+cdM5TyzthpQryv5kudRP9PIKT7XQ.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:10: [hashed name]
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[titan.picoctf.net]:51362' (ED25519) to the list of known hosts.
ctf-player@titan.picoctf.net's password:
Welcome ctf-player, here's your flag: picoCTF{......_.........._........}
Connection to titan.picoctf.net closed.
```