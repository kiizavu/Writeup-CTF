# Challenge name: sneeki snek


## Code
```
  4           0 LOAD_CONST               1 ('')
              2 STORE_FAST               0 (f)

  5           4 LOAD_CONST               2 ('rwhxi}eomr\\^`Y')
              6 STORE_FAST               1 (a)

  6           8 LOAD_CONST               3 ('f]XdThbQd^TYL&\x13g')
             10 STORE_FAST               2 (z)

  7          12 LOAD_FAST                1 (a)
             14 LOAD_FAST                2 (z)
             16 BINARY_ADD
             18 STORE_FAST               1 (a)

  8          20 LOAD_GLOBAL              0 (enumerate)
             22 LOAD_FAST                1 (a)
             24 CALL_FUNCTION            1
             26 GET_ITER
        >>   28 FOR_ITER                48 (to 78)
             30 UNPACK_SEQUENCE          2
             32 STORE_FAST               3 (i)
             34 STORE_FAST               4 (b)

  9          36 LOAD_GLOBAL              1 (ord)
             38 LOAD_FAST                4 (b)
             40 CALL_FUNCTION            1
             42 STORE_FAST               5 (c)

 10          44 LOAD_FAST                5 (c)
             46 LOAD_CONST               4 (7)
             48 BINARY_SUBTRACT
             50 STORE_FAST               5 (c)

 11          52 LOAD_FAST                5 (c)
             54 LOAD_FAST                3 (i)
             56 BINARY_ADD
             58 STORE_FAST               5 (c)

 12          60 LOAD_GLOBAL              2 (chr)
             62 LOAD_FAST                5 (c)
             64 CALL_FUNCTION            1
             66 STORE_FAST               5 (c)

 13          68 LOAD_FAST                0 (f)
             70 LOAD_FAST                5 (c)
             72 INPLACE_ADD
             74 STORE_FAST               0 (f)
             76 JUMP_ABSOLUTE           28

 14     >>   78 LOAD_GLOBAL              3 (print)
             80 LOAD_FAST                0 (f)
             82 CALL_FUNCTION            1
             84 POP_TOP
             86 LOAD_CONST               0 (None)
             88 RETURN_VALUE
```

## Methodology
This python bytecode, each block seperated by a line is one line in python. The `STORE_FAST` instruction is call a variable, the `LOAD_CONST` instruction is set value for variable called by `STORE_FAST` instruction. `LOAD_FAST` is load value of a variable, `BINARY_SUBTRACT`, `BINARY_ADD`, etc.. is operator, `LOAD_GLOBAL` is function of python, `CALL_FUNCTION` call `CALL_FUNCTION`. `FOR_ITER` is loop function like for or while.
I read this document https://docs.python.org/3/library/dis.html and reverse it by hand.

## Code reverse
```python
f = ''
a = 'rwhxi}eomr\\^`Y'
z = 'f]XdThbQd^TYL&\x13g'

a = a + z

for i, b in enumerate(a):
    c = ord(b)
    c = c - 7
    c = c + i
    c = chr(c)
    f += c

print(f)
```

## Flag
kqctf{dont_be_mean_to_snek_:(}

# ==========================================================================================

# Challenge name: sneeki snek 2 oh no what did i do


## Code
```
  4           0 BUILD_LIST               0
              2 STORE_FAST               0 (a)

  5           4 LOAD_FAST                0 (a)
              6 LOAD_METHOD              0 (append)
              8 LOAD_CONST               1 (1739411)
             10 CALL_METHOD              1
             12 POP_TOP

  6          14 LOAD_FAST                0 (a)
             16 LOAD_METHOD              0 (append)
             18 LOAD_CONST               2 (1762811)
             20 CALL_METHOD              1
             22 POP_TOP

  7          24 LOAD_FAST                0 (a)
             26 LOAD_METHOD              0 (append)
             28 LOAD_CONST               3 (1794011)
             30 CALL_METHOD              1
             32 POP_TOP

  8          34 LOAD_FAST                0 (a)
             36 LOAD_METHOD              0 (append)
             38 LOAD_CONST               4 (1039911)
             40 CALL_METHOD              1
             42 POP_TOP

  9          44 LOAD_FAST                0 (a)
             46 LOAD_METHOD              0 (append)
             48 LOAD_CONST               5 (1061211)
             50 CALL_METHOD              1
             52 POP_TOP

 10          54 LOAD_FAST                0 (a)
             56 LOAD_METHOD              0 (append)
             58 LOAD_CONST               6 (1718321)
             60 CALL_METHOD              1
             62 POP_TOP

 11          64 LOAD_FAST                0 (a)
             66 LOAD_METHOD              0 (append)
             68 LOAD_CONST               7 (1773911)
             70 CALL_METHOD              1
             72 POP_TOP

 12          74 LOAD_FAST                0 (a)
             76 LOAD_METHOD              0 (append)
             78 LOAD_CONST               8 (1006611)
             80 CALL_METHOD              1
             82 POP_TOP

 13          84 LOAD_FAST                0 (a)
             86 LOAD_METHOD              0 (append)
             88 LOAD_CONST               9 (1516111)
             90 CALL_METHOD              1
             92 POP_TOP

 14          94 LOAD_FAST                0 (a)
             96 LOAD_METHOD              0 (append)
             98 LOAD_CONST               1 (1739411)
            100 CALL_METHOD              1
            102 POP_TOP

 15         104 LOAD_FAST                0 (a)
            106 LOAD_METHOD              0 (append)
            108 LOAD_CONST              10 (1582801)
            110 CALL_METHOD              1
            112 POP_TOP

 16         114 LOAD_FAST                0 (a)
            116 LOAD_METHOD              0 (append)
            118 LOAD_CONST              11 (1506121)
            120 CALL_METHOD              1
            122 POP_TOP

 17         124 LOAD_FAST                0 (a)
            126 LOAD_METHOD              0 (append)
            128 LOAD_CONST              12 (1783901)
            130 CALL_METHOD              1
            132 POP_TOP

 18         134 LOAD_FAST                0 (a)
            136 LOAD_METHOD              0 (append)
            138 LOAD_CONST              12 (1783901)
            140 CALL_METHOD              1
            142 POP_TOP

 19         144 LOAD_FAST                0 (a)
            146 LOAD_METHOD              0 (append)
            148 LOAD_CONST               7 (1773911)
            150 CALL_METHOD              1
            152 POP_TOP

 20         154 LOAD_FAST                0 (a)
            156 LOAD_METHOD              0 (append)
            158 LOAD_CONST              10 (1582801)
            160 CALL_METHOD              1
            162 POP_TOP

 21         164 LOAD_FAST                0 (a)
            166 LOAD_METHOD              0 (append)
            168 LOAD_CONST               8 (1006611)
            170 CALL_METHOD              1
            172 POP_TOP

 22         174 LOAD_FAST                0 (a)
            176 LOAD_METHOD              0 (append)
            178 LOAD_CONST              13 (1561711)
            180 CALL_METHOD              1
            182 POP_TOP

 23         184 LOAD_FAST                0 (a)
            186 LOAD_METHOD              0 (append)
            188 LOAD_CONST               4 (1039911)
            190 CALL_METHOD              1
            192 POP_TOP

 24         194 LOAD_FAST                0 (a)
            196 LOAD_METHOD              0 (append)
            198 LOAD_CONST              10 (1582801)
            200 CALL_METHOD              1
            202 POP_TOP

 25         204 LOAD_FAST                0 (a)
            206 LOAD_METHOD              0 (append)
            208 LOAD_CONST               7 (1773911)
            210 CALL_METHOD              1
            212 POP_TOP

 26         214 LOAD_FAST                0 (a)
            216 LOAD_METHOD              0 (append)
            218 LOAD_CONST              13 (1561711)
            220 CALL_METHOD              1
            222 POP_TOP

 27         224 LOAD_FAST                0 (a)
            226 LOAD_METHOD              0 (append)
            228 LOAD_CONST              10 (1582801)
            230 CALL_METHOD              1
            232 POP_TOP

 28         234 LOAD_FAST                0 (a)
            236 LOAD_METHOD              0 (append)
            238 LOAD_CONST               7 (1773911)
            240 CALL_METHOD              1
            242 POP_TOP

 29         244 LOAD_FAST                0 (a)
            246 LOAD_METHOD              0 (append)
            248 LOAD_CONST               8 (1006611)
            250 CALL_METHOD              1
            252 POP_TOP

 30         254 LOAD_FAST                0 (a)
            256 LOAD_METHOD              0 (append)
            258 LOAD_CONST               9 (1516111)
            260 CALL_METHOD              1
            262 POP_TOP

 31         264 LOAD_FAST                0 (a)
            266 LOAD_METHOD              0 (append)
            268 LOAD_CONST               9 (1516111)
            270 CALL_METHOD              1
            272 POP_TOP

 32         274 LOAD_FAST                0 (a)
            276 LOAD_METHOD              0 (append)
            278 LOAD_CONST               1 (1739411)
            280 CALL_METHOD              1
            282 POP_TOP

 33         284 LOAD_FAST                0 (a)
            286 LOAD_METHOD              0 (append)
            288 LOAD_CONST              14 (1728311)
            290 CALL_METHOD              1
            292 POP_TOP

 34         294 LOAD_FAST                0 (a)
            296 LOAD_METHOD              0 (append)
            298 LOAD_CONST              15 (1539421)
            300 CALL_METHOD              1
            302 POP_TOP

 36         304 LOAD_CONST              16 ('')
            306 STORE_FAST               1 (b)

 37         308 LOAD_FAST                0 (a)
            310 GET_ITER
        >>  312 FOR_ITER                80 (to 394)
            314 STORE_FAST               2 (i)

 38         316 LOAD_GLOBAL              1 (str)
            318 LOAD_FAST                2 (i)
            320 CALL_FUNCTION            1
            322 LOAD_CONST               0 (None)
            324 LOAD_CONST               0 (None)
            326 LOAD_CONST              17 (-1)
            328 BUILD_SLICE              3
            330 BINARY_SUBSCR
            332 STORE_FAST               3 (c)

 39         334 LOAD_FAST                3 (c)
            336 LOAD_CONST               0 (None)
            338 LOAD_CONST              17 (-1)
            340 BUILD_SLICE              2
            342 BINARY_SUBSCR
            344 STORE_FAST               3 (c)

 40         346 LOAD_GLOBAL              2 (int)
            348 LOAD_FAST                3 (c)
            350 CALL_FUNCTION            1
            352 STORE_FAST               3 (c)

 41         354 LOAD_FAST                3 (c)
            356 LOAD_CONST              18 (5)
            358 BINARY_XOR
            360 STORE_FAST               3 (c)

 42         362 LOAD_FAST                3 (c)
            364 LOAD_CONST              19 (55555)
            366 BINARY_SUBTRACT
            368 STORE_FAST               3 (c)

 43         370 LOAD_FAST                3 (c)
            372 LOAD_CONST              20 (555)
            374 BINARY_FLOOR_DIVIDE
            376 STORE_FAST               3 (c)

 44         378 LOAD_FAST                1 (b)
            380 LOAD_GLOBAL              3 (chr)
            382 LOAD_FAST                3 (c)
            384 CALL_FUNCTION            1
            386 INPLACE_ADD
            388 STORE_FAST               1 (b)
            390 EXTENDED_ARG             1
            392 JUMP_ABSOLUTE          312

 45     >>  394 LOAD_GLOBAL              4 (print)
            396 LOAD_FAST                1 (b)
            398 CALL_FUNCTION            1
            400 POP_TOP
            402 LOAD_CONST               0 (None)
            404 RETURN_VALUE

```

## Methodology
This python bytecode, each block seperated by a line is one line in python. The `STORE_FAST` instruction is call a variable, the `LOAD_CONST` instruction is set value for variable called by `STORE_FAST` instruction. `LOAD_FAST` is load value of a variable, `BINARY_SUBTRACT`, `BINARY_ADD`, etc.. is operator, `LOAD_GLOBAL` is function of python, `CALL_FUNCTION` call `CALL_FUNCTION`. `FOR_ITER` is loop function like for or while.
I read this document https://docs.python.org/3/library/dis.html and reverse it by hand.

## Code reverse
```python
a = []
a.append(1739411)
a.append(1762811)
a.append(1794011)
a.append(1039911)
a.append(1061211)
a.append(1718321)
a.append(1773911)
a.append(1006611)
a.append(1516111)
a.append(1739411)
a.append(1582801)
a.append(1506121)
a.append(1783901)
a.append(1783901)
a.append(1773911)
a.append(1582801)
a.append(1006611)
a.append(1561711)
a.append(1039911)
a.append(1582801)
a.append(1773911)
a.append(1561711)
a.append(1582801)
a.append(1773911)
a.append(1006611)
a.append(1516111)
a.append(1516111)
a.append(1739411)
a.append(1728311)
a.append(1539421)
b = ''
for i in a:
    c = str(i)[::-1]
    c = c[:-1]
    c = int(c)
    c = c ^ 5
    c = c - 55555
    c = c // 555
    b += chr(c)
print(b)
```

## Flag
kqctf{snek_waas_not_so_sneeki}
