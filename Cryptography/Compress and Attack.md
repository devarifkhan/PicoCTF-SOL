# Cryptography
## Compress and Attack - 130 Points
## Question
> Your goal is to find the flag. compress_and_attack.py nc mercury.picoctf.net 2431

## Solution
> We get access to an oracle where we can give some input m and we get back Enc(k, compress(flag + m)) where k is a random key (changes across oracle calls) and Enc is the encryption procedure of the stream cipher Salsa20.

> We will not be able to guess the key or the plaintext by breaking Salsa20 (as it seems secure, and I don't know a same plaintext different key attack).

> So we will attack the compression scheme.

## Zlib compress
> I Googled how Zlib compression works. It seems to use DEFLATE. The idea behind DEFLATE is twofold:

> The matching and replacement of duplicate strings with pointers.

> Replacing symbols with new, weighted symbols based on frequency of use.

> The Salsa20 stream cipher leaks the length of the plaintext. So given an input given, we can know how much the flag plus input was compressed.

> I test my idea locally. I input a dummy flag picoCTF{dummy_flag} and launch the program.

> If I input no m, I get back a length of 27 bytes. Now if I input picoCTF{ which is the known part of the plaintext, this gives me a ciphertext of length 30 instead of 35, so some compression happened!

> I try this time to input picoCTF{a: I get back a 31 bytes ciphertext (as the a is not compressed), but if I input picoCTF{d (with d being the following character of the flag), I get only 30 bytes of ciphertext!

> Yeah I have an exploit! Here is the code to find the flag remotely.

```python
from pwn import *
import string

sh = remote("mercury.picoctf.net", 2431)

def oracle(text):
    sh.recvuntil("encrypted:")
    sh.sendline(text)
    sh.recvline()
    sh.recvline()
    return int(sh.recvline().decode())

known = "picoCTF{"

length = oracle(known)
print(known, end="")

current = ""
while current != "}":
    for c in string.printable:
        if oracle(known + c) == length:
            known += c
            current = c
            print(c, end="")
            break
```
> Flag is: 	picoCTF{sheriff_you_solved_the_crime}
