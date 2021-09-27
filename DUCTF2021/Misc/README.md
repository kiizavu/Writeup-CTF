# Challenge name: General Skills Quiz
### Description:
QUIZ TIME! Just answer the questions. Pretty easy right?

## Methodology
![image](https://user-images.githubusercontent.com/69805864/134935095-8e0d367e-1901-46a3-acbd-fe4f838092ca.png)

As the welcome banner shown we just have 30 second to answer all question. It is impossible !!!
So I wrote the code to solve it.

## Exploit code

```
from pwn import *
import urllib
import base64
import codecs

def encode_base64(message):
    base64_bytes = base64.b64encode(message)
    base64_message = base64_bytes.decode('ascii')
    return base64_message

r = remote('pwn-2021.duc.tf','31905')
print(r.recvuntil("for the quiz..."))
r.sendline()

print(r.recvuntil("1+1=?"))
r.sendline("2")

print(r.recvuntil(": "))
hex_2_dec = r.recvline()
r.sendline(str(int(hex_2_dec, 16)))

print(r.recvuntil(": "))
hex_2_ascii = r.recvline()[:-1].decode("ASCII")
r.sendline(bytearray.fromhex(str(hex_2_ascii)).decode())

print(r.recvuntil(": "))
decode_URL = r.recvline()[:-1]
r.sendline(urllib.parse.unquote(decode_URL))

print(r.recvuntil(": "))
base64_string = r.recvline()[:-1]
r.sendline(base64.b64decode(base64_string))

print(r.recvuntil(": "))
to_base64 = r.recvline()[:-1]
r.sendline(encode_base64(to_base64))

print(r.recvuntil(": "))
rot13_string = r.recvline()[:-1]
rot13_pt = codecs.decode(rot13_string.decode('utf-8'), 'rot_13')
r.sendline(rot13_pt)

print(r.recvuntil(": "))
to_rot13 = r.recvline()[:-1]
r.sendline(codecs.encode(to_rot13.decode('utf-8'), 'rot_13'))

print(r.recvuntil(": "))
bin_num = r.recvline()[:-1]
r.sendline(str(int(bin_num, 2)))

print(r.recvuntil(": "))
dec_num = r.recvline()[:-1]
r.sendline(str(bin(int(dec_num))))

print(r.recvuntil("CTF competition in the universe?\n"))

r.sendline("DUCTF")

while(True):
    print(r.recvline())
```
Run it and we will get the flag.

![image](https://user-images.githubusercontent.com/69805864/134935701-7e3ef53c-c908-405d-82de-be6cada6c3ae.png)

## Flag
DUCTF{you_aced_the_quiz!\_have_a_gold_star_champion}
