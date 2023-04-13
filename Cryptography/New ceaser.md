# Cryptography
## New Caesar - 60 Points
## Question
> We found a brand new type of encryption, can you break the secret code? (Wrap with picoCTF{}) dcebcmebecamcmanaedbacdaanafagapdaaoabaaafdbapdpaaapadanandcafaadbdaapdpandcac
## Solution
> Source Code.Let's analyze the code.
```python

import string
LOWERCASE_OFFSET = ord("a") 
ALPHABET = string.ascii_lowercase[:16]


> So here the offset is 97 and alphabet is from a to p => [abcdefghijklmnop]

```python
def b16_encode(plain):
    enc = ""
    for c in plain:
        binary = "{0:08b}".format(ord(c))
        enc += ALPHABET[int(binary[:4], 2)]
        enc += ALPHABET[int(binary[4:], 2)]
    return enc
```
> The code might look confusing,let's break it down.Here binary value is 01100001 . The value in decimal is 97 which means a in ascii.
```text
┌──(kali㉿kali)-[~]
└─$ python
Python 3.11.2 (main, Mar 13 2023, 12:18:29) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> "{0:08b}".format(ord('a'))
'01100001'
>>> int(f'{ord("a"):08b}',2)
97

```
> Let's see what's happend next.So now a turn into gb.
```python
>>> alphabet = string.ascii_lowercase[:16]
>>> a = f'{ord("a"):08b}'
>>> int(a[:4],2)
6
>>> int(a[4:],2)
1
>>> alphabet[6]+alphabet[1]
'gb'
```
> Let's see what shift does.
```python
def shift(c, k):
    t1 = ord(c) - LOWERCASE_OFFSET
    t2 = ord(k) - LOWERCASE_OFFSET
    return ALPHABET[(t1 + t2) % len(ALPHABET)]
```

> For the two variables we get two characters, and substract 97 from their ascii values. Then just add those together (modulo length of the alphabet so we stay in bounds) and use that value as an index for the alphabet list.

```python
flag = "redacted"
key = "redacted"
assert all([k in ALPHABET for k in key])
assert len(key) == 1

b16 = b16_encode(flag)
enc = ""
for i, c in enumerate(b16):
    enc += shift(c, key[i % len(key)])
print(enc)
```
> This is all put together here. The initial assertions are really telling. The assert makes sure that a condition is met and if not it raises an assertion error. So we can tell that the key is made up of letters from the alphabet only! Next we know that the length of this key is 1. The flag is then put into b16_encode, so it doubles in length! And here is the enumerate method:
```python
>>> c = 'lkmjkemjmkiekeijiiigljlhilihliikiliginliljimiklligljiflhiniiiniiihlhilimlhijil'
>>> for i,c in enumerate(c):
...     print(f'{i} {c}')
...
0 l
1 k
2 m
3 j
4 k
5 e
6 m
7 j
8 m
9 k
...
...
```
## Solution
```Python
# import string
import string

# constants
LOWERCASE_OFFSET = ord("a")
ALPHABET = string.ascii_lowercase[:16]

# decode function
def b16_decode(cipher):
    dec = ""
    # loop through the cipher 2 characters at a time
    for c in range(0, len(cipher), 2):
        # turn the two characters into one binary string
        b = ""
        b += "{0:b}".format(ALPHABET.index(cipher[c])).zfill(4)
        b += "{0:b}".format(ALPHABET.index(cipher[c+1])).zfill(4)
        # turn the binary string to a character and add
        dec += chr(int(b,2))
    
    # return
    return dec

# unshift the text
def unshift(c, k):
    t1 = ord(c) - LOWERCASE_OFFSET
    t2 = ord(k) - LOWERCASE_OFFSET
    return ALPHABET[(t1 - t2) % len(ALPHABET)]

# encrypted flag
enc = "mlnklfnknljflfmhjimkmhjhmljhjomhmmjkjpmmjmjkjpjojgjmjpjojojnjojmmkmlmijimhjmmj"

# loop through all possible keys
for key in ALPHABET:
    # initialize string
    s = ""

    # loop through the encrypted text
    for i,c in enumerate(enc):
        # unshift it based on key
        s += unshift(c, key[i % len(key)])

    # decode
    s = b16_decode(s)

    # print key
    print(s)  

```
> Flag is: picoCTF{et_tu?_a2da1e18af49f649806988786deb2a6c}
