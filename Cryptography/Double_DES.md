# Cryptography
## Double DES - 120 Points
## Question
> I wanted an encryption service that's more secure than regular DES, but not as slow as 3DES... The flag is not in standard format. nc mercury.picoctf.net 5958 ddes.py

## Solution
> We get the encrypted output of the flag. We are also allowed to encrypt our own input, which means that we are able to inspect an encrypted output for which we know the plaintext.

> Since the encryption scheme just encrypts the data twice using two keys, it is vulnerable to a Meet-in-the-middle attack:
> So, we first encrypt a random string, such as "a" using 2DES. We receive the encrypted output from the server, and start attacking it.

> We encrypt a with all possible keys (which are essentially all possible strings of 6 digits, padded to the length of a block) using single DES, and save each encrypted output in a dictionary, together with the key that was used for the encryption.

> Then, we take the encrypted output we got from the server when we encrypted a using 2DES, and try to decrypt it with all possible keys using single DES. Eventually, we should get a result which matches some encrypted output from the first phase. The keys that were used to achieve this are the secret keys also used to encrypt the flag.

```python
#!/usr/bin/python3 -u
from pwn import *

from Crypto.Cipher import DES
import itertools
import string

KEY_LEN = 6

def pad(msg):
    block_len = 8
    over = len(msg) % block_len
    pad = block_len - over
    return (msg + " " * pad).encode()

def double_decrypt(m, key1, key2):
    cipher2 = DES.new(key2, DES.MODE_ECB)
    dec_msg = cipher2.decrypt(m)

    cipher1 = DES.new(key1, DES.MODE_ECB)
    return cipher1.decrypt(dec_msg)

def all_possible_keys():
    return itertools.product(string.digits, repeat=KEY_LEN)

r = remote("mercury.picoctf.net", 5958)
r.recvline()
flag = r.recvlineS().strip()
log.info("Encrypted flag: {}".format(flag))

to_encrypt = b'a'
log.info("Trying to encrypt '{}'".format(to_encrypt))
r.sendlineafter("What data would you like to encrypt?", enhex(to_encrypt))
a_enc = r.recvlineS().strip()
log.info("Encrypted form: {}".format(a_enc))
a_enc = bytes.fromhex(a_enc)

a_padded = pad(to_encrypt.decode())

d = {}

with log.progress('Encrypting plaintext with all possible keys') as p:
    for k1 in all_possible_keys():
        k1 = pad("".join(k1))
        p.status("Key: {}".format(k1))
        cipher1 = DES.new(k1, DES.MODE_ECB)
        enc = cipher1.encrypt(a_padded)
        d[enc] = k1

with log.progress('Decrypting ciphertext with all possible keys') as p:
    for k2 in all_possible_keys():
        k2 = pad("".join(k2))
        p.status("Key: {}".format(k2))
        cipher2 = DES.new(k2, DES.MODE_ECB)
        dec = cipher2.decrypt(a_enc)
        if dec in d:
            k1 = d[dec]
            log.info("Found match, key1 = {}, key2 = {}".format(k1, k2))
            log.success(double_decrypt(unhex(flag), k1, k2))
            break
```

> Flag is: 	e21cf83f64e53a743e685e55852feaf2 Don't use picoCTF{} here.
